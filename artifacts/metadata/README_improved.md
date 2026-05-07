# Proposed Improved README for ArWISE Dataset

## Dataset Overview

ArWISE contains participant-level smartwatch-derived feature tables for activity recognition research. Files are stored as CSVs and organized into cohort-level zip archives.

## File Organization

- Each participant has one or more CSV files.
- CSV files are grouped by cohort (`c01` through `c20`).
- Some cohorts are split across multiple archives due to repository file-size limits.
- Some cohorts include `w1` and `w2` watch designations when participants alternated between day and night watches.
- Known missing file: `c03.p053.w2.csv`.

## Recommended File Inventory Table

A repository-level file inventory should include:

| Cohort | File/Archive | Participant Count | Watch Designation | Label Coverage | Notes |
|---|---|---|---|---|---|

## Missing Values

The documentation should distinguish among:

1. Empty string / NaN: no value exported or no ground truth label.
2. `-1`: GPS speed/course unavailable for specific mean/mode fields.
3. Undefined standard deviations: standard deviation values may be empty for zero-duration or insufficient windows.

## Activity Labels

Allowed labels:
Eat, Errands, Exercise, Hobby, Housework, Hygiene, Relax, Sleep, Socialize, Travel, Work, Other.

Recommended addition:
- Add file-level label coverage percentages.
- Indicate which files/cohorts are suitable for supervised learning.

## Feature Documentation Corrections

Correct:
`time_of_day_cos = cos(time_of_day_radians)`

Current description says sine, which conflicts with observed data values and feature naming.

## Recommended Reuse Notes

Users should:
- inspect label coverage before supervised modeling
- decide how to handle zero-duration windows
- distinguish between `-1` GPS unavailable values and empty missing values
- avoid assuming all cohorts have the same label completeness
- check whether x/y acceleration equality is expected before using those features
