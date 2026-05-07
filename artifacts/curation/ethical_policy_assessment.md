# Ethical, Legal, and Policy Assessment

## Human Data Considerations

ArWISE contains wearable activity data. Even if direct identifiers are removed, wearable traces may reveal behavioral routines, sleep patterns, travel behavior, home related location-derived features, and activity habits.

## Privacy Considerations

The dataset includes features derived from location and distance from home. Although the data are feature-level rather than raw GPS traces, these variables still require careful reuse guidance.

## Reuse Risks

- There is some risk of behavioral reidentification if combined with external data.
- Missing activity labels can be misinterpreted as negative examples.
- Overgeneralization across cohorts without understanding cohort differences.
- Secondary use beyond the original consent scope if consent constraints are not explicit.

## Recommended Documentation Improvements

- Adding a short privacy and de-identification statement.
- Clarify whether participant consent allows broad secondary reuse.
- Explain whether location-derived features were transformed to reduce identifiability.
- Provide recommended citation and responsible reuse language.
- Clarify whether there are restrictions on commercial or clinical use.

## Curator Conclusion

The dataset is appropriate for public research reuse through a repository, but the ethical documentation should more directly address privacy risk, de-identification decisions, and downstream reuse boundaries.
