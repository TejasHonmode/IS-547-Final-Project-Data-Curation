# CURATE(D) Assessment of the ArWISE Dataset for Reuse-Ready Wearable Health Research

 **By - Tejas Honmode (honmode2)**

## Introduction

This final project applies the CURATE(D) pathway to assess the reuse readiness of the ArWISE (Activity Recognition from In-the-Wild Smartwatches) dataset. I am framing the work from the perspective of an institutional research data curator working in a university. As a data curator, I have to review the published dataset before recommending it for long-term reuse by researchers. The purpose is to evaluate whether the dataset is sufficiently documented, transparent, and preservation ready for independent secondary reuse.

The use case is a research team that wants to reuse ArWISE as a benchmark dataset for wearable activity-recognition models. For that use case, the dataset must support independent understanding of variables, activity labels, preprocessing decisions, file organization, provenance, and ethical reuse constraints. Wearable datasets are challenging because sensor traces and location-derived variables may have sensitive behavioral information even after direct identifiers are removed. This makes documentation, provenance, and reuse guidance very important curation concerns.


## Dataset and Artifact Characterization

The primary research object is the ArWISE dataset deposited in Dryad. The dataset consists of participant-level CSV files grouped into cohort zip files. The feature description states that files are organized by cohorts c01-c20, with some cohorts split into multiple parts because of repository file-size constraints. Some participants in cohorts c03 and c05 alternated between two watches, represented with w1 and w2 designations.

The dataset includes smartwatch-derived motion features, GPS-derived mobility features, time-derived features, and an ‘activity_label’ field. The uploaded subset used for evidence in this final package is ‘c15’. It contains 21 participants.

Related artifacts created for this project include a schema profile, data dictionary, curation log, FAIR assessment, provenance assessment, improved README recommendations, preservation recommendations, hypothetical author correspondence, and a guide to create a simple Python profiling script.


## Data Lifecycle Model and Curation Goals

This project uses CURATE(D) pathway because the project evaluates an already-published dataset rather than creating a new dataset from raw collection. The project uses a simplified research data lifecycle model: acquisition, organization, inspection, metadata assessment, quality assessment, provenance review, documentation improvement, and preservation planning.

The prioritized curation goals are: (1) support independent understanding and reuse, (2) identify metadata and documentation gaps, (3) assess provenance and transparency, (4) evaluate ethical and policy concerns, (5) document reproducible curation evidence, and (6) recommend preservation improvements.


## Curation Workflow

The workflow began with review of the official project instructions, project plan, and progress report. I then used the provided feature description as the repository-level metadata source and inspected the uploaded ‘c15’ cohort files as the concrete evidence subset. Because the full dataset is large and multi-cohort, I limited detailed profiling to the uploaded c15 subset and avoided claiming full-dataset validation.

The workflow steps were: (1) inspect file structure and feature descriptions, (2) profile the uploaded CSV files, (3) compare observed fields against documented metadata, (4) identify reuse barriers, (5) produce metadata and provenance recommendations, and (6) package all findings as CURATE(D) artifacts.
A simple guide to reproduce a Python script is included. The script can regenerates the schema profile, window-duration profile, and validation findings. This makes the curation process inspectable without turning the project into a complex technical implementation.


## Metadata and Data Documentation Findings

The dataset has a useful foundation of metadata. The feature description gives variable names, units, timestamp meaning, GPS conventions, activity labels, and file-structure notes. However, several issues reduce independent reuse readiness.

The most important documentation error is the definition of ‘time_of_day_cos’. The feature description says it equals ‘sin(time_of_day_radians)’. It should be corrected to ‘cos(time_of_day_radians)`.

A second issue is missing-value representation. The feature description says speed/course values may use -1 when GPS is unavailable, while the missing-data section says empty strings are used for missing values. In the inspected subset, ‘speed_mean’, ‘course_mode’, ‘speed_std’ and ‘course_std’ have a mixture of -1 and blanks for missing values. This suggests the repository needs a more detailed missing value policy by field type.

The `activity_label` field is also a major reuse concern. In the inspected subset, all rows have missing labels. This may be valid according to the documentation, but it means the file is not directly usable for supervised activity-recognition modeling. A label coverage table by file or cohort would help users select appropriate files.


## Data Quality and Reuse Barriers

The inspected subset revealed several curation concerns that should be documented for future users. First, there are several rows where ‘stamp_start’ equals ‘stamp_end’. This pattern may be explainable, but it should be explicitly documented because zero-duration windows seem like invalid entries to users.

Second, ‘user_acceleration_x_mean’ and ‘user_acceleration_y_mean’ are identical in many rows of the inspected files. This may be a preprocessing issue, but it is unusual enough that secondary users should not ignore it. It is highly unlikely for a human to move exactly with equal acceleration along the x-y plane in the real world. The correct curation response is not to declare the dataset wrong, but to document the anomaly and request clarification.

Third, local timestamps are provided without timezone. This is stated in the documentation and may be intentional, but users who compare participants across locations need explicit guidance. This is especially important for time-of-day and day-of-week features.


## Provenance and Data Lineage

The dataset has strong high-level provenance because it is hosted in Dryad, has a DOI, and is associated with a publication. However, computational provenance is less complete. The shared files contain derived features, but the available feature description does not fully document how raw watch data were transformed into these final variables.

The most important missing provenance details are window-generation logic, handling of overlapping windows, sensor synchronization, preprocessing filters, label alignment, and cohort harmonization. Without these details, a researcher can reuse the final table but cannot fully reproduce or evaluate the transformation pipeline. This distinction matters because computational reproducibility depends not only on file access, but also on understanding how the shared research object was created.


## Ethical, Legal, and Policy Assessment

The dataset contains human data. Even when direct identifiers are removed, wearable traces can reveal sensitive patterns related to activity routines, travel, sleep, and home-related behavior. The dataset includes distance-from-home and bearing-from-home features, which reduce direct exposure compared with raw GPS but still deserve careful privacy documentation.

The repository would be stronger if it included a clearer privacy and de-identification statement, consent-scope description, and downstream reuse guidance. For example, users should know whether commercial use, clinical decision-making, or participant-level re-identification attempts are prohibited. The curation recommendation is not to restrict legitimate research use, but to document responsible reuse boundaries more clearly.


## Reproducibility and Transparency

This project emphasizes transparency by documenting the exact subset inspected, and a guide to create a  profiling script in Python, and the generated (sample) outputs. Users can make a basic Python script on the included raw subset to reproduce the schema profile and validation findings. 

The main limitation is that this project does not reproduce the original machine-learning study or validate every cohort. The CURATE(D) contribution is a transparent curation review and a set of evidence-backed recommendations.

## Archiving and Preservation

Dryad provides a strong baseline for long-term access because it supports repository hosting and DOI-based citation. CSV is also an open and preservation-friendly format. However, preservation quality could be improved through better packaging.

Recommended preservation improvements include a repository-level manifest, checksums, machine-readable data dictionary, file inventory, label coverage table, version history, and preprocessing provenance record. These additions would improve future reuse without changing the underlying research data.


## Conclusion and Next Steps

The ArWISE dataset is valuable, publicly accessible, and relevant for wearable activity-recognition research. Its strengths include DOI-based access, cohort-level organization, clear feature naming, and publication context. However, the CURATE(D) review identified important reuse barriers and incomplete preprocessing provenance.

The most important next steps are to correct metadata errors, add file-level label coverage, clarify missing-value policies, publish preprocessing provenance, and improve repository-level preservation documentation. This project demonstrates that public availability is not the same as reuse readiness. High-quality curation requires evidence-based assessment, transparent documentation, and clear communication of limitations.


## AI Declaration

I used generative AI to assist with grammar and artifact formatting. I reviewed the content and revised the final deliverable for accuracy and fit with the assignment.

## References

-	Cook, Diane; Minor, Bryan; Holder, Lawrence et al. (2025). Activity recognition from in-the-wild smartwatches (ArWISE) [Dataset]. Dryad. https://doi.org/10.5061/dryad.jdfn2z3nm 
-	B. Minor, C. Greeley, R. Holder, B. Thomas, L. B. Holder and D. J. Cook, "A Feature-Augmented Transformer Model to Recognize Functional Activities From in-the-Wild Smartwatch Data," in IEEE Journal of Biomedical and Health Informatics, vol. 30, no. 1, pp. 256-265, Jan. 2026, doi: 10.1109/JBHI.2025.3586074.

