# Script-Supported Reproducibility and Metadata Validation Suggestions

## Purpose

This document does not provide a full cleaning or preprocessing pipeline. Instead, it identifies lightweight checks that future users or curators could implement in their own Python scripts to support reproducibility, metadata validation, and transparent reuse of ArWISE participant-level CSV files.

These snippets are intended to help users inspect files consistently before analysis. They should be treated as curation supporting checks rather than automatic corrections to the dataset.

---

## 1. Schema Inspection

### Curation issue addressed

Users need to confirm that participant-level CSV files have the expected columns and compatible data types before combining or analyzing files.

### Example Python snippet

```python
import pandas as pd

df = pd.read_csv("c15.p014.csv")

schema_profile = pd.DataFrame({
    "column_name": df.columns,
    "observed_dtype": [str(df[col].dtype) for col in df.columns],
    "missing_count": [df[col].isna().sum() for col in df.columns],
    "missing_percent": [round(df[col].isna().mean() * 100, 2) for col in df.columns]
})

schema_profile.to_csv("schema_profile.csv", index=False)
```
Sample output can be found in `output/schema_provile.csv`.

### Curation value

This helps document file structure and supports transparent comparison across participant files or cohorts.

---

## 2. Missing-Value Profiling

### Curation issue addressed

The dataset uses both empty values and `-1` values in different contexts. A script can help users identify how missingness is represented in each file.

### Example Python snippet

```python
missing_summary = pd.DataFrame({
    "column_name": df.columns,
    "empty_or_nan_count": [df[col].isna().sum() for col in df.columns],
    "empty_or_nan_percent": [round(df[col].isna().mean() * 100, 2) for col in df.columns]
})

missing_summary.to_csv("missing_value_summary.csv", index=False)
```

### Optional GPS-specific check

```python
gps_columns = ["speed_mean", "speed_std", "course_mode", "course_std"]

for col in gps_columns:
    if col in df.columns:
        minus_one_count = (df[col] == -1).sum()
        empty_count = df[col].isna().sum()
        print(f"{col}: -1 count = {minus_one_count}, empty/NaN count = {empty_count}")
```

### Curation value

This helps clarify whether missing values are consistently represented and whether the documented missing-value policy matches the exported data.

---

## 3. Activity Label Coverage

### Curation issue addressed

Some cohorts are documented as having no activity labels. A script can help users quickly determine whether a file supports supervised activity-recognition analysis.

### Example Python snippet

```python
if "activity_label" in df.columns:
    total_rows = len(df)
    missing_labels = df["activity_label"].isna().sum()
    label_coverage = 100 - round((missing_labels / total_rows) * 100, 2)

    print(f"Total rows: {total_rows}")
    print(f"Missing activity labels: {missing_labels}")
    print(f"Label coverage: {label_coverage}%")

    label_counts = df["activity_label"].value_counts(dropna=False)
    label_counts.to_csv("activity_label_counts.csv")
```

### Curation value

This supports reuse decisions by showing which files are useful for supervised learning and which files may only support unlabeled or feature-level analysis.

---

## 4. Valid Activity Label Check

### Curation issue addressed

The documentation defines twelve possible activity labels. A script can check whether non-missing labels fall within this controlled vocabulary.

### Example Python snippet

```python
expected_labels = {
    "Eat", "Errands", "Exercise", "Hobby", "Housework", "Hygiene",
    "Relax", "Sleep", "Socialize", "Travel", "Work", "Other"
}

if "activity_label" in df.columns:
    observed_labels = set(df["activity_label"].dropna().unique())
    unexpected_labels = observed_labels - expected_labels

    print("Unexpected labels:", unexpected_labels)
```

### Curation value

This supports metadata validation and helps detect label spelling, encoding, or documentation inconsistencies.

---

## 5. Timestamp and Window-Duration Check

### Curation issue addressed

The feature description states that `stamp_start` and `stamp_end` define the time window used to compute features. A script can identify zero-duration or negative-duration windows.

### Example Python snippet

```python
df["stamp_start_parsed"] = pd.to_datetime(df["stamp_start"], errors="coerce")
df["stamp_end_parsed"] = pd.to_datetime(df["stamp_end"], errors="coerce")

df["window_duration_seconds"] = (
    df["stamp_end_parsed"] - df["stamp_start_parsed"]
).dt.total_seconds()

print("Zero-duration windows:", (df["window_duration_seconds"] == 0).sum())
print("Negative-duration windows:", (df["window_duration_seconds"] < 0).sum())

duration_summary = df["window_duration_seconds"].describe()
duration_summary.to_csv("window_duration_summary.csv")
```

### Curation value

This helps users identify edge cases in feature windows and decide whether additional documentation or filtering guidance is needed.

---

## 6. Standard Deviation Missingness in Zero-Duration Windows

### Curation issue addressed

In the inspected subset, zero-duration windows were associated with missing standard deviation features. A script can check whether this pattern occurs in other files.

### Example Python snippet

```python
std_columns = [col for col in df.columns if col.endswith("_std")]

if "window_duration_seconds" in df.columns and std_columns:
    zero_duration_rows = df["window_duration_seconds"] == 0
    all_std_missing = df[std_columns].isna().all(axis=1)

    both_count = (zero_duration_rows & all_std_missing).sum()

    print("Rows with zero-duration windows:", zero_duration_rows.sum())
    print("Rows where all *_std fields are missing:", all_std_missing.sum())
    print("Rows with both conditions:", both_count)
```

### Curation value

This supports investigation of whether missing standard deviations are expected for single-sample or zero-duration windows.

---

## 7. `time_of_day_cos` Metadata Validation

### Curation issue addressed

The feature description states that `time_of_day_cos` equals `sin(time_of_day_radians)`, but the inspected data suggest it corresponds to cosine. A script can validate this relationship.

### Example Python snippet

```python
import numpy as np

if "time_of_day_radians" in df.columns and "time_of_day_cos" in df.columns:
    cos_matches = np.isclose(
        df["time_of_day_cos"],
        np.cos(df["time_of_day_radians"]),
        atol=1e-9
    ).sum()

    sin_matches = np.isclose(
        df["time_of_day_cos"],
        np.sin(df["time_of_day_radians"]),
        atol=1e-9
    ).sum()

    print("Rows matching cos(time_of_day_radians):", cos_matches)
    print("Rows matching sin(time_of_day_radians):", sin_matches)
```

### Curation value

This supports evidence-based correction of the feature documentation.

---

## 8. `time_of_day_sin` Validation

### Curation issue addressed

Since `time_of_day_sin` is defined as `sin(time_of_day_radians)`, users can validate whether this derived feature is consistent with the documentation.

### Example Python snippet

```python
if "time_of_day_radians" in df.columns and "time_of_day_sin" in df.columns:
    sin_matches = np.isclose(
        df["time_of_day_sin"],
        np.sin(df["time_of_day_radians"]),
        atol=1e-9
    ).sum()

    print("Rows matching sin(time_of_day_radians):", sin_matches)
```

### Curation value

This helps distinguish isolated documentation errors from broader feature-generation problems.

---

## 9. X/Y Acceleration Equality Check

### Curation issue addressed

In the inspected subset, `user_acceleration_x_mean` and `user_acceleration_y_mean` were identical across all rows. A script can check whether this pattern appears in other files.

### Example Python snippet

```python
pairs_to_check = [
    ("user_acceleration_x_mean", "user_acceleration_y_mean"),
    ("user_acceleration_x_std", "user_acceleration_y_std")
]

for col_x, col_y in pairs_to_check:
    if col_x in df.columns and col_y in df.columns:
        equal_count = df[col_x].fillna("__MISSING__").eq(
            df[col_y].fillna("__MISSING__")
        ).sum()

        print(f"{col_x} equals {col_y}: {equal_count} of {len(df)} rows")
```

### Curation value

This does not prove an error, but it helps identify feature behavior that may require author clarification or additional provenance documentation.

---

## 10. Cohort-Level Label Suitability Table

### Curation issue addressed

Cohorts differ in whether activity labels are available. A script can help create a label coverage table when applied across participant files.

### Example Python snippet

```python
from pathlib import Path

rows = []

for csv_file in Path("data").glob("*.csv"):
    temp_df = pd.read_csv(csv_file)

    if "activity_label" in temp_df.columns:
        total = len(temp_df)
        missing = temp_df["activity_label"].isna().sum()
        coverage_percent = 100 - round((missing / total) * 100, 2)

        rows.append({
            "file": csv_file.name,
            "rows": total,
            "missing_activity_labels": missing,
            "label_coverage_percent": coverage_percent
        })

label_coverage = pd.DataFrame(rows)
label_coverage.to_csv("label_coverage_by_file.csv", index=False)
```

### Curation value

This would help users quickly identify which files are appropriate for supervised modeling and which are not.

---

## 11. File-Level Curation Summary

### Curation issue addressed

Researchers benefit from a concise summary of each file before deciding whether it is suitable for reuse.

### Example Python snippet

```python
summary = {
    "file_name": "c15.p014.csv",
    "row_count": len(df),
    "column_count": len(df.columns),
    "activity_label_missing_percent": round(df["activity_label"].isna().mean() * 100, 2)
        if "activity_label" in df.columns else None,
    "zero_duration_window_count": int((df["window_duration_seconds"] == 0).sum())
        if "window_duration_seconds" in df.columns else None,
    "columns_with_missing_values": int((df.isna().sum() > 0).sum())
}

summary_df = pd.DataFrame([summary])
summary_df.to_csv("file_level_curation_summary.csv", index=False)
```

### Curation value

This type of summary can support repository-level documentation, preservation planning, and future reuse decisions.

---

## Overall Recommendation

A future curation workflow could combine these snippets into a lightweight validation script that generates consistent file-level metadata summaries across all participant CSV files (as shown in `sample script outputs/validation_findings.json`). The goal should not be to automatically modify the dataset, but to augment repository documentation and help users make informed reuse decisions.
