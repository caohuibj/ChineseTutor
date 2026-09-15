# C2 review summary

Issue: #4 — Introduce structured TrainingAttempt evidence events

## Implemented

- one submitted learner response = one `TrainingAttempt` event;
- attempt-series grouping for first/second/subsequent answers to the same concrete Question;
- canonical Question reference with legacy reconstruction fallback;
- intended target nodes separated from actual per-node observed evidence;
- explicit `not_observed` vs negative evidence;
- H0-H7 support context plus optional same-level hint count;
- 0/1/2/null diagnostic dimensions for question understanding, task recognition, location, evidence, reasoning, terminology, written expression and overall correctness;
- multi-code K/R/I/E/Q/M/C errors plus primary causal error;
- material/task/familiarity/complexity/variant snapshots;
- response modality and optional long-form artifact reference;
- append-friendly attempt-to-attempt delta on the later event;
- assessment provenance/confidence so diagnostic labeling is auditable;
- conservative legacy reconstruction rules;
- current 《周亚夫军细柳》 session represented as C2 fixtures;
- explicit separation between raw Attempt events and durable `学习记录` / Session Summary.

## Semantic self-review findings

The first C2 draft exposed seven important issues and corrected them.

### 1. Final-answer-only storage would erase the teaching signal

First and second answers are separate events. The second event points backward and records its delta, preserving what the minimal intervention actually changed.

### 2. Question-level correctness cannot be copied to every node

Multi-node tasks now carry per-node evidence with `positive / mixed / negative / not_observed`. Upstream failure can therefore leave a downstream ability unobserved rather than falsely failed.

### 3. N/A must not equal failure

Every diagnostic dimension supports `null`. This is essential for writing, oral work and open tasks.

### 4. Response modality was missing

C2 now records `written / typed_chat / oral / artifact / mixed`. Oral reasoning can prove thinking without falsely proving or disproving written expression.

### 5. Long-form production needed artifact linkage

`artifact_ref` lets a composition/version carry the full writing artifact while the Attempt stores structured evidence, avoiding duplicated drafts.

### 6. Historical context needed stronger snapshots

Material ID, Task Type, complexity, familiarity and variant-group context are snapshotted so later retagging cannot silently change what historical evidence meant.

### 7. Diagnostic judgments themselves need provenance

C2 now records `assessed_by`, `assessment_confidence`, and optional `assessment_policy_version`. Learner evidence and confidence in the evaluator's labeling are distinct things. Later teacher/AI recalibration must be auditable rather than silently rewriting history.

## Important evidence decisions

### Hint level is context, not punishment

H3 success is valuable evidence that one guiding question repairs the missing step; it is simply different evidence from H0 independent success.

### Hint reduction is an automation signal

Comparable success while support falls from H5/H3 toward H1/H0 is more informative than accuracy count alone.

### Familiarity and variant similarity constrain transfer claims

A different year/grade/exam label does not automatically make a task an unfamiliar transfer. Material familiarity and near-duplicate families must be explicit.

### Attempt score is local, not Profile mastery

`reasoning=2` means reasoning succeeded in this response under these conditions. It does not mean `Mastery=M2` until C1 aggregation has sufficient diverse evidence.

### Legacy reconstruction is intentionally incomplete

Old learning records may have unknown hint level, timing or exact task metadata. C2 leaves those fields null instead of fabricating H0 independence or false precision.

## Acceptance judgment

**PASS for C2 semantic architecture.**

The next unresolved problem is no longer event shape. It is how reusable learning content is represented:

```text
Material
vs
Question
vs
Task Type
```

That is Issue #5 / the next PR. Full numerical Profile update rules should wait until Material/Question and complexity semantics are stable enough to interpret evidence diversity correctly.
