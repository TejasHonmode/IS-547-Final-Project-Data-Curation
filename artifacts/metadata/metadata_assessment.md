# Metadata Assessment

## Scope

This assessment reviews the ArWISE feature description, cohort-level documentation, and the uploaded `c15` subset. The goal is not to judge the scientific value of the dataset, but to assess whether repository metadata supports independent understanding, interpretation, and reuse.

---

## Metadata Strengths

- The dataset has a structured file organization model: participant-level CSV files are grouped into cohort archives.
- The feature description provides variable names, broad meanings, and measurement units for many sensor-derived variables.
- Activity labels are defined as a controlled list of twelve possible values.
- Missing activity labels are documented as representing absence of ground truth.
- The dataset has a DOI and is connected to a peer-reviewed publication.
- Cohort-level participant characteristics are documented separately, including distinctions between:
  - younger adults
  - HOA/SCD/MCI participants
  - cohorts with and without activity labels
  - expert-annotated vs self-reported activities

---

## Metadata Issues Identified

### 1. `time_of_day_cos` documentation error

The feature description states that:

`time_of_day_cos = sin(time_of_day_radians)`

However, inspection of the uploaded subset showed that `time_of_day_cos` values correctly correspond to:

`cos(time_of_day_radians)`

This appears to be a metadata/documentation error rather than a data-generation error. This error might prompt a user to further check if the data in this column is correct or not.

### Recommendation

Correct the feature description to:

`time_of_day_cos = cos(time_of_day_radians)`

---

### 2. Missing-value policy needs more granularity

The feature description notes that unavailable GPS speed/course values may be encoded as `-1`, while the missing-data section says empty strings are used for missing values.

In the inspected subset:
- `speed_mean` and `course_mode` frequently use `-1`
- related standard deviation fields such as `speed_std` and `course_std` contain empty values instead

This creates ambiguity for downstream preprocessing workflows because different missing-value conventions are used simultaneously across related features.

### Recommendation

Add a detailed missing-value policy table documenting:
- which fields use `-1`
- which fields use empty strings
- whether different missing-value conventions have different semantic meanings

---

### 3. Cohort-level label coverage is not surfaced clearly enough

In the inspected `c15` subset, `activity_label` is missing for all rows.

After reviewing the cohort-level documentation, this appears to be expected behavior because Cohort 15 is explicitly described as:

`HOA/SCD/MCI, no activity labels`

This means the absence of labels is not necessarily a dataset anomaly.

However, this information is not surfaced prominently enough at the file or cohort level for independent users.

A researcher inspecting only the CSV files could incorrectly assume:
- labels were lost during preprocessing
- the dataset export is incomplete
- or the file is corrupted

This is therefore a metadata discoverability and reuse-readiness issue rather than a simple data-quality issue.

Activity label also seems like an important metric and it being missing, seems like a problem for the overall analysis of the data. It would also be beneficial if the authors could provide relative importances of each feature of the dataset.

### Recommendation

Add:
- cohort-level label coverage summaries with more details
- file-level label availability indicators
- a reusable matrix showing which cohorts support supervised activity-recognition tasks

This would significantly improve reuse efficiency and reduce user confusion.

---

### 4. Derived feature anomalies require explanation

In the inspected subset:
- `user_acceleration_x_mean`
- `user_acceleration_y_mean`

contain identical values across many records.


This may reflect:
- preprocessing behavior
- sensor orientation assumptions
- export artifacts
- or feature-engineering choices

It is highly unlikely that users accelerate equally in the same direction on the x-y plane so more information about how this field was derived would be useful.

### Recommendation

Add provenance or preprocessing notes explaining whether this equality is expected.

---

### 5. Window-duration behavior requires documentation

The inspected subset contains rows where:

`stamp_start == stamp_end`

producing apparent zero-duration windows.


The repository documentation does not explain whether these represent single-sample windows, timestamp truncation errors or invalid windows.

This limits interpretability for downstream users.

### Recommendation

Document:
- how windows are generated
- how overlapping or short windows are handled
- whether zero-duration windows should be retained or filtered during reuse

---

## Overall Metadata Assessment

The dataset has a strong foundational metadata structure and substantial research value. However, several metadata and documentation issues reduce independent reuse readiness.

The most important metadata improvements are:
- correcting the `time_of_day_cos` definition
- clarifying missing-value conventions
- surfacing cohort-level label coverage more prominently
- improving preprocessing provenance
- documenting edge-case window behavior

Many of the observed reuse barriers are not caused by absence of metadata, but rather by metadata being fragmented across multiple locations and not sufficiently connected to file level reuse decisions.