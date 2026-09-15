# R1.6 current-learner pilot content gates

Status: **pre-runtime production QA**

These gates apply to the minimum current-learner slice only. They do not claim exhaustive Gaokao graph/content coverage.

## A. Canonical / learner separation

- [x] Only one new Knowledge node was added because the current school task actually requires biography-vs-memoir distinction.
- [x] No mastery/automation/review fields were written into canonical LearningNodes.
- [x] Unknown learner nodes were not backfilled as M0/A0.
- [x] Existing legacy-backed states were not upgraded merely because new Questions were curated.

## B. TaskType compactness

- [x] TaskTypes represent recurring learner operations, not one exam stem each.
- [x] Character analysis / typical event / comparison remain reusable across texts.
- [x] New detail, structure, genre, title, evaluation, classical word-sense, writing-selection and revision-comparison environments have distinct output contracts.
- [x] Source grade/year is absent from TaskType identity.
- [x] Broader Issue #34 remains open.

## C. Material provenance

- [x] Every content-backed Question references exactly one Material occurrence/bundle.
- [x] The writing diagnostic references `MAT-unforgettable-person-writing-prompt-school`; it is not a material-less Question.
- [x] Multi-text Questions reference bundle Materials rather than duplicating component identity.
- [x] Source file and school provenance are preserved.
- [x] Copyrighted source text is not copied into the registry/docs.

## D. Question authenticity

- [x] School prompts copied from the learning design are `school_provided + verbatim`.
- [x] Diagnostic transformations are explicitly `adapted`.
- [x] No adapted item is described as verbatim authentic school/exam content.
- [x] D2 complexity is based on task demand, not source grade.
- [x] Transfer candidates are separate from complexity band.
- [x] Variant-group metadata is used where near-duplicate evidence could otherwise fake diversity.

## E. Current learner requirements

- [x] Known legacy-backed gaps use current-training requirements.
- [x] Unknown nodes use diagnostic/school-sync requirements rather than fake mastery gaps.
- [x] Character, comparison significance and comparison dimension require moderate evidence before target satisfaction.
- [x] Transfer is explicitly required only where a transfer claim is intended.

## F. TrainingMove explainability

- [x] Every initial TrainingMove has a human reason and success criterion.
- [x] The first selected move targets the only observed mixed reasoning gap: comparison significance.
- [x] The first move repeats familiar 4.6 only to establish a native H0 baseline.
- [x] The familiar repeat is explicitly not transfer evidence.
- [x] A new-surface comparison variant is queued immediately after the baseline.
- [x] Unknown detail/classical/genre/writing abilities are queued as diagnostics, not failures.

## G. R1.8 evidence integrity

- [x] Curating a Question does not create an Attempt.
- [x] Selecting a TrainingMove does not create an Attempt.
- [x] No synthetic answer is permitted to count toward the 20–40 native Attempt pilot.
- [x] H0–H7 must reflect interventions actually delivered in Chat runtime.
- [x] Retry responses must be separate TrainingAttempts in the same series.
- [x] Per-node observations must be written separately from overall correctness.

## H. Deployment safety

- [x] v1 remains available and source-of-truth switch is not started.
- [x] No v1 row is deleted, archived or overwritten by R1.6.
- [x] R1.5 deferred full-table Notion reconciliation remains a cutover blocker, not silently treated as passed.
- [x] Broad I1 Issues #34/#35/#36 remain open.

## Exit decision

R1.6 is GO for R1.7 Chat-runtime integration when:

```text
selected TrainingMove exists
+ its Question/Material are resolvable
+ Attempt is not created until learner submits
+ runtime can record actual hint/intervention telemetry
```

Those conditions are now represented in production. R1.7 must operationalize the write sequence before the first native pilot response is accepted as v2 evidence.
