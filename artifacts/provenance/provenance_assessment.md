# Provenance Assessment

## Scope

This assessment evaluates whether the dataset provides enough provenance information for an independent researcher to understand how the shared CSV files were produced and how they should be interpreted.

## Provenance Strengths

- The dataset is publicly hosted in Dryad with a DOI.
- The dataset is associated with a peer-reviewed publication.
- File organization by cohort and participant provides a basic structural lineage.
- Feature names suggest derived variables from smartwatch sensor streams.

## Provenance Gaps

### 1. Raw-to-derived transformation lineage

The shared files contain derived features rather than raw sensor streams. However, the documentation available in the feature description does not fully explain the preprocessing pipeline from raw smartwatch data to the final feature table.

Missing details include:
- window-generation procedure
- sensor synchronization approach
- handling of irregular sampling
- filtering steps
- feature computation logic
- cohort harmonization steps

### 2. Version history

The repository description does not provide enough detail about whether cohort files or preprocessing scripts changed across releases.

### 3. Local timestamp provenance
Timestamps are documented as local user time without timezone. This is understandable for privacy and usability, but it limits comparability across users unless timezone handling is explained.

### 4. Annotation provenance

Missing activity labels are documented as no ground truth, but the annotation workflow needs more explanation. Users need to know how labels were collected, whether they were self-reported, how label timing was aligned to windows, and whether some cohorts have systematically lower label coverage.

## Recommended Provenance Improvements

1. Add a preprocessing workflow diagram.
2. Publish or describe (in detail) feature-engineering scripts.
3. Add a release/version history.
4. Add file-level label coverage statistics.
5. Add cohort-level notes describing device/watch differences.
6. Clarify how local timestamps should be used or compared.

## Curator Conclusion

The dataset has adequate publication-level provenance but incomplete computational provenance. This is a major reuse barrier for researchers who want to reproduce or adapt the feature-generation process.
