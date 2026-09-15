# D2 band-assignment review policy

Status: normative supplement to `schemas/task-complexity.schema.md`.

The eight-dimensional vector is primary. `C0-C5` is a compact reviewed summary and must not be produced by a naive weighted average.

## Review order

Assign the vector first, then evaluate bands from highest to lowest. The first band whose semantic condition is clearly satisfied becomes the summary band.

### C5 — open complex construction

Use when:

- `response_openness = 3`; and
- either `expression_load = 3` or `reasoning_depth = 3`; and
- the response requires sustained organization/choice rather than a short bounded answer.

Typical: full writing, open design/evaluation with substantial constructed response.

### C4 — high integration / constrained evaluation

Use when C5 is not satisfied and either:

- `reasoning_depth = 3` plus at least one of `information_hiddenness`, `material_heterogeneity`, `knowledge_retrieval_distance` >= 2; or
- at least three semantic-demand dimensions among hiddenness/reasoning/heterogeneity/knowledge-distance/response-openness/expression-load are >=2, with a clear high-integration requirement.

C4 remains constrained enough that the task is not an open long-form construction.

### C3 — integrated multi-node / multi-source synthesis

Use when C4/C5 are not satisfied and:

- two or more semantic-demand dimensions are >=2; and
- the task requires coordinated integration across nodes, sections, materials, or comparison dimensions.

Typical: multi-text comparison/integration, complex structural explanation, two-poem comparison.

### C2 — multi-step explanation

Use when C3-C5 are not satisfied and:

- `reasoning_depth = 2`; or
- `information_hiddenness = 2` with required explicit explanation; or
- the answer requires an evidence→warrant→conclusion chain or several coordinated local operations.

### C1 — single-step application

Use when:

- reasoning is <=1;
- the response is tightly bounded;
- no meaningful multi-source/high-integration requirement exists;
- at least one nontrivial rule/relation/transformation is required.

### C0 — recognition/direct retrieval

Use when the task is essentially recognition, recall, direct copy/extraction, or minimal reproduction with no meaningful inferential transformation.

## Guardrails

- `time_pressure` never raises a semantic band by itself.
- `text_load` never raises a semantic band by itself.
- learner unfamiliarity/transfer status never changes the canonical band.
- a single remote fact (`knowledge_retrieval_distance=3`) does not automatically make a fixed recall item C4.
- a high band does not imply the learner should train it now; recommendation considers Profile and prerequisites.
- reviewers may override the mechanical-looking threshold examples when the semantic rationale is clearer, but must record `complexity_rationale`.

## Calibration discipline

When reviewers disagree by two or more bands, do not average. Revisit the vector anchors first.

When reviewers disagree by one band, inspect the dominant operation:

- local multi-step explanation -> C2;
- coordinated multi-node/source integration -> C3;
- evaluation/modeling/remote application -> C4;
- open sustained construction -> C5.

The goal is stable routing, not pseudo-precision.