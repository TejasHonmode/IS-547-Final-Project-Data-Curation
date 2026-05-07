# Curation Log

Project: CURATE(D) assessment of ArWISE dataset  
Curator role: Institutional research data curator  
Dataset subset used : `c15` cohort

| Step | Activity | Evidence/Outcome | Curation Decision |
|---|---|---|---|
| 1 | Reviewed official course project instructions | Confirmed CURATE(D) pathway requires curation report, curation log, metadata improvements, and correspondence. | Package organized as a curation review rather than a machine-learning analysis. |
| 2 | Reviewed project plan | Plan defined ArWISE as a multi-cohort wearable dataset requiring metadata, provenance, FAIR, and ethical assessment. | Maintained CURATE(D) scope and avoided expanding into full model reproduction. |
| 3 | Reviewed progress report | Progress report identified missing values, metadata mismatch, acceleration duplication, missing labels, and irregular windows. | Treated those as the core evidence trail for final assessment. |
| 4 | Inspected uploaded `c15` cohort subset | Folder has 21 samples and each file (sample) contains tens of thousands 0f records and 41 columns. | Used this cohort as evidence for sample-level curation findings. |
| 5 | Generated schema profile | `sample script outputs/schema_profile.csv` records dtype and missingness for every column. | Included as reproducibility artifact. |
| 6 | Assessed activity label completeness | `activity_label` is missing for a lot of files. | Recommended clearly marking files/cohorts with no ground-truth labels. |
| 7 | Assessed missing-value behavior | `speed_mean` uses -1 in some rows and blanks in other rows. Same with `speed_std`. | Recommended a missing-value policy that distinguishes unavailable GPS values from undefined standard deviations. |
| 8 | Validated `time_of_day_cos` | Checked if values matched to cos(`time_of_day_radians`) | Recommended correcting documentation from sine to cosine. |
| 9 | Assessed time-window consistency | Many records have zero-duration windows. Those rows may be invalid and possibly not fit for re-use in any kind of analysis. | Recommended documenting zero-duration windows or adding exclusion guidance. |
| 10 | Assessed acceleration feature anomaly | x/y acceleration mean and x/y acceleration standard deviation are identical across many of the inspected rows. | Recommended author clarification before secondary reuse. |
| 11 | Created data dictionary | `artifacts/metadata/data_dictionary.csv` consolidates feature descriptions and curation notes. | Metadata improvement artifact. |
| 12 | Drafted author correspondence | `hypothetical_author_correspondence.md` records curator questions for authors. | CURATE(D) correspondence artifact. |
| 13 | Prepared preservation recommendations | `preservation_recommendations.md` recommends versioning, centralized README, and machine-readable metadata. | Supports archiving and long-term access. |
