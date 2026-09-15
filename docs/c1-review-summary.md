# C1 review summary

Issue: #3 — Separate personal Learner Profile state from canonical graph

## Implemented

- `LearnerNodeState` identity keyed by `(learner_id, node_id)`;
- explicit unknown state distinct from M0/A0/C0;
- Mastery M0–M3 with Knowledge/Ability/Strategy interpretation;
- Automation A0–A3 independent from mastery;
- verified complexity band C0–C5 as evidence-backed summary, not attempted maximum;
- per-axis evidence confidence for Mastery / Automation / Complexity;
- per-axis verification timestamps;
- state provenance (`legacy_backfill / attempt_derived / mixed / manual_override`);
- nullable cached counters to distinguish unknown history from zero events;
- recency / forgetting semantics;
- stable error-pattern summary with confidence;
- orthogonal readiness / bottleneck / review statuses plus derived UI attention label;
- conservative non-destructive v1 backfill rules;
- synthetic profile fixtures;
- QA gates and ADR-007.

## Semantic self-review findings

The first C1 draft exposed four issues that were corrected before opening the PR.

### 1. A single evidence-strength field was too coarse — fixed

Mastery can be strongly evidenced while Automation remains weakly evidenced. C1 now stores:

```text
mastery_evidence_strength
automation_evidence_strength
complexity_evidence_strength
```

instead of one global confidence value.

### 2. A single `last_verified_at` was misleading — fixed

An untimed independent answer may verify mastery but not timed automation. C1 now uses:

```text
mastery_verified_at
automation_verified_at
complexity_verified_at
```

plus `last_trained_at` for general activity recency.

### 3. Legacy counters need unknown distinct from zero — fixed

The first draft used integer counters only. During v1 migration we usually do not know exact historical attempt counts. C1 now allows:

```text
attempt_count: null
```

for unknown historical count, while `0` means a known zero in the structured event source.

### 4. Stable / Developing / Bottleneck / Review Due are not one semantic axis — normalized

These labels mix readiness, graph impact and recency. C1 keeps:

```text
readiness_status
bottleneck_status
review_status
```

separate and treats `attention_status` only as a derived dashboard convenience.

## Important profile decisions

### Unknown-first

A newly added high-value Gaokao node is `unknown`, not M0. The graph can expand without falsely making the learner look weaker.

### State may downgrade

M/A/C are estimates from evidence and can move downward when forgetting, transfer failure or contradictory evidence appears.

### No graph mastery propagation

B2 edges guide diagnosis and recommendation but never grant learner evidence automatically.

### Stable is requirement-relative

A node may be stable for the current active target while still below future Gaokao complexity. Observed state and goal comparison stay separate.

### Bottleneck is graph-aware

A bottleneck is not simply the lowest score. It needs learner deficit/uncertainty plus meaningful downstream impact and causal plausibility.

### Legacy backfill is conservative

- clean one-to-one v1 rows may seed weak-confidence states;
- split rows do not copy one mastery value to all children;
- Task-Type-only labels do not create fake profile nodes;
- merged cross-domain rows do not fabricate M3 transfer;
- `最近训练` does not become verification recency;
- `待加强` does not become Bottleneck.

## Acceptance judgment

**PASS for C1 semantic architecture.**

The major unresolved questions now require C2 TrainingAttempt evidence:

- exact update rules for M/A/C;
- how hint level changes evidence weight;
- how repeated similar tasks are de-duplicated for confidence;
- how first/second attempts contribute differently;
- how artifact/version evidence works for writing;
- when contradictory evidence downgrades a state.

Those are intentionally C2 scope rather than adding formulas prematurely in C1.
