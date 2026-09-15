# D2 review summary

Issue: #6 — Define task complexity vector independent of source grade

## Implemented

- eight-dimensional intrinsic task-demand vector;
- reviewed C0-C5 semantic summary bands;
- explicit separation of intrinsic complexity from learner novelty/transfer;
- deterministic highest-to-lowest band review precedence;
- migration policy for legacy `基础/常规/提升/迁移` labels;
- cross-domain calibration fixtures covering modern reading, classical Chinese, poetry, language use, writing, and high-integration information tasks;
- compatibility amendment for the earlier C1 `C4=unfamiliar transfer` shorthand;
- QA gates and ADR-010.

## Semantic self-review findings

### 1. `C4 = unfamiliar transfer` was conceptually wrong — fixed

Unfamiliarity is learner-relative. The same Question may be familiar to one learner and unseen to another.

D2 now defines:

```text
C4 = high integration / remote application / constrained evaluation
```

while transfer evidence stays in C2 Attempt context.

### 2. One scalar difficulty score would hide useful training levers — rejected

Two tasks can have similar overall difficulty but demand different interventions:

- high reasoning + low expression;
- low reasoning + high expression;
- high text load + direct extraction;
- low text load + deep implicit inference.

The vector remains primary; C-band is routing/UI summary only.

### 3. Band assignment needed more consistency — fixed

The first schema anchors were interpretable but could still produce reviewer drift. Added `complexity/band-assignment.md` with C5→C0 review precedence and calibration rules.

### 4. Time pressure should not promote semantic complexity — fixed

Time pressure is retained because it strongly affects Automation evidence and recommendation, but it cannot independently increase C-band.

### 5. Full writing exposed an important edge case — clarified

A full narrative composition may have `reasoning_depth=2` rather than 3 yet still be C5 because `response_openness=3` and `expression_load=3` require sustained complex construction.

Thus C5 cannot simply mean “reasoning depth 3”.

### 6. Remote knowledge alone should not create C4 — clarified

A fixed recall item may have high `knowledge_retrieval_distance` but low reasoning/openness. Preserve the vector rather than inflating semantic band.

### 7. Legacy `迁移` cannot map to C4 — fixed

Legacy `迁移` may indicate novelty, cross-domain application, extension, or simply teacher-perceived difficulty. It now migrates as a provisional curation/transfer-probe hint, never as a direct band assignment.

## Calibration result

Representative bands currently behave as intended:

```text
sentence minimal revision                  C1
character evidence -> trait explanation    C2
classical translation                      C2
poetry emotion explanation                 C2
poetry comparison                          C3
multi-text evidence sufficiency evaluation C4
full narrative writing                     C5
material-based argument writing            C5
```

These are calibration fixtures, not claims that every item of the named TaskType has that band. Complexity belongs to each concrete Question.

## Acceptance judgment

**PASS for D2 semantic architecture.**

The model now supports controlled training moves such as:

```text
hold reasoning depth steady
raise expression load
```

or:

```text
hold intrinsic vector roughly steady
reduce hints and switch to unfamiliar material
```

This is more useful for adaptive tutoring than generic “harder/easier” progression.

## Deliberately deferred

- empirical calibration against large question sets;
- automatic vector scoring by model;
- exact inter-rater agreement procedure;
- recommendation weighting using Profile + vector;
- live Notion migration of all current questions.