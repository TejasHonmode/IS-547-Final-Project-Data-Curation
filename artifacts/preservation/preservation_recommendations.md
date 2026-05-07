# Preservation Recommendations

## Current Preservation Strengths

- Dryad hosting provides repository infrastructure.
- DOI assignment supports citation and discovery.
- CSV format is open, widely supported, and preservation-friendly.
- The associated publications improves interpretive context.

## Preservation Risks

1. Fragmented Packaging  
Cohort zip files and split archives may be difficult for future users to navigate without a formal inventory.

2. Missing Version History  
Long-term users need to know whether files or certain features were updated or regenerated.

3. Limited Machine-Readable Metadata  
Feature descriptions are useful but should also be provided in machine-readable form (json, preferrably).

4. Incomplete Provenance  
Derived features cannot be fully interpreted without workflow-level provenance.

## Recommended Preservation Actions

- Add a repository-level manifest with checksums.
- Add machine-readable data dictionary in CSV or JSON.
- Add version history and release notes.
- Add clear README with file naming conventions.
- Add file-level label coverage.
- Add provenance diagram or preprocessing workflow description.
- Preserve scripts or pseudocode for feature extraction.
- Document known anomalies and recommended user decisions.

## Recommended Archival Package Structure

```text
ArWISE/
  README.md
  LICENSE.txt
  CITATION.cff
  metadata/
    data_dictionary.csv
    file_inventory.csv
    label_coverage.csv
    provenance_record.md
  data/
    c01.zip
    c02_part1.zip
    ...
  scripts/
    feature_extraction_reference.py
  docs/
    methodology_notes.pdf
    cohort_notes.md
```
