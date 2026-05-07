# Reproducibility and Transparency Workflow

## Purpose

This artifact documents how the curation findings can be inspected and reproduced.

A Python script can be generated to address some of the mentioned issues.

## Files Used

- Raw subset: `data/c15.zip`
- Sample Outputs:
  - `outputs/schema_profile.csv`
  - `outputs/window_duration_profile.csv`
  - `outputs/validation_findings.json`
  - `outputs/evidence_summary.csv`


## Required Software

- Python
- pandas
- numpy

## What the Script Should Check

The script reproduces:
- row and column count
- missing values by column
- timestamp span
- activity-label missingness
- GPS missing-value patterns
- zero-duration windows
- standard deviation missingness
- `time_of_day_cos` validation
- x/y acceleration equality checks

## Sample of Important Findings That Can be Produced

- `activity_label` is missing for 16067 of 16067 rows.
- `time_of_day_cos` matches cosine for 16067 rows.
- 3258 rows have zero-duration windows.
- x/y acceleration mean values match in 16067 rows.

## Transparency Limitation

This workflow reproduces the curation assessment for the uploaded subset only. It does not claim full-dataset validation across all cohorts.
