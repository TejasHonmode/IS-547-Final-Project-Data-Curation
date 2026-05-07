# Hypothetical Correspondence to Authors

Subject: CURATE(D) Review Questions and Metadata Clarifications for ArWISE Dataset

To ArWISE Dataset Authors,

I am reviewing the ArWISE dataset as part of a CURATE(D)-based curation assessment. The goal of this review is to evaluate whether an independent researcher can understand, interpret, and reuse the dataset without needing direct clarification from the original research team.

I would like to thank you for making this dataset publicly available. The dataset has pretty strong reuse value because it combines wearable sensor features, activity labels, and multi-cohort structure. During review of the repository documentation and the `c15` cohort subset, I identified several clarification questions that may improve metadata quality and long-term reuse.

## 1. Missing Value Representation

The feature description states that unavailable GPS speed/course values may use `-1`. The general missing-data note also says empty strings are used for missing data. In the inspected subset, `speed_mean` and `course_mode` often use `-1`, while `speed_std` and `course_std` contain empty values.

Could you clarify the intended missing-value policy for each GPS-related field?

## 2. `time_of_day_cos` Definition

The feature description states that `time_of_day_cos` equals `sin(time_of_day_radians)`. This is wrong and needs to be fixed. I inspected the files, and the values match `cos(time_of_day_radians)` for the  rows so the data is good for that.

## 3. Activity Label Coverage

In the cohort susbet that I was working on, activity label is missing. The subset of data that I was working with specifically had in its description that activity label are not present but it seems like this parameter is rather important in analysis. 

Do you have any documents that might show us the relative importance of different features?

## 4. Zero-Duration Windows

Many files contain records where `stamp_start` equals `stamp_end`. 

Could you clarify whether these zero-duration windows are expected and whether users should retain or exclude them?

## 5. Acceleration Feature Equality

Many records also have `user_acceleration_x_mean` and `user_acceleration_y_mean` identical. Their standard deviation fields also match.

Could you confirm whether this equality is expected because of preprocessing or whether it may indicate an export or feature generation issue?

## 6. Preprocessing Provenance

For long-term reuse, it would be helpful to include additional documentation describing:
- window generation
- sensor synchronization
- feature extraction
- cohort harmonization
- label alignment

Thank you again for making the dataset available.

Sincerely,  
Tejas Honmode  
Institutional Data Curator Role, IS547 CURATE(D) Project
