# ADR-011 — Learner Profile updates are evidence-driven, axis-specific and replayable

Status: **proposed in C3**

## Context

B1/B2 define the canonical learning graph. C1 defines current per-learner node state. C2 defines structured TrainingAttempt evidence. D2 separates intrinsic task complexity from learner-relative novelty.

Without an explicit update policy, Profile values could still drift into arbitrary teacher impressions, naive attempt counting, or one opaque score. That would undermine explainability and personalized recommendation.

## Decision

ChineseTutor derives `LearnerNodeState` from node-specific evidence using semantic transition gates rather than a single weighted learner score.

Core principles:

1. **node observation, not Question correctness, is the update unit**;
2. **M/A/C are independent axes**;
3. **hint/support changes evidence meaning causally, not through a blanket penalty**;
4. **hold + confidence change is a valid outcome**;
5. **diversity, independence, familiarity, complexity, recency and assessment provenance all matter**;
6. **M3 requires stable unfamiliar generalization, not high C-band alone**;
7. **one high-complexity failure does not erase lower-band mastery**;
8. **repeated comparable recent contradiction can downgrade current state**;
9. **legacy evidence is retained conservatively but cannot dominate strong native telemetry**;
10. **every material update emits an auditable ProfileUpdateDecision**.

## State levels are claims, not badges

A node does not need to pass through every state in separate chronological steps.

If an imported or newly observed evidence set already satisfies M2 semantics, the current estimate may initialize directly to M2. The same applies to A and verified complexity.

This prevents artificial training merely to unlock a state transition.

## Replayability

The Profile is a projection, not an append-only truth ledger.

TrainingAttempt evidence remains authoritative. Implementations may update incrementally for efficiency, but the result must be reproducible from:

```text
canonical evidence ledger
+ policy version
+ explicit manual overrides
```

Periodic replay/recomputation must be possible. A later policy version may produce a different current projection without deleting historical evidence or prior decisions.

## Mastery

- M0 requires valid evidence of non-establishment, not lack of data.
- M1 is scaffolded/constrained successful performance.
- M2 requires representative independent performance with non-trivial diversity.
- M3 requires established M2 plus stable unfamiliar transfer/generalization across meaningfully distinct contexts.

Attempt count is descriptive, not sufficient.

## Automation

Automation tracks access/trigger/fluency separately from execution quality.

- A1 may coexist with M2 when a learner can execute once reminded.
- A2 needs self-trigger under normal conditions.
- A3 needs actual pressure/fluency evidence with maintained quality.

## Complexity

Verified complexity is based on intrinsic D2 Question demand successfully evidenced for the node. Novelty/transfer is learner-relative C2 context.

A learner can show M3 on unfamiliar C2 tasks; a familiar C4 task does not prove M3.

## Contradiction

A first meaningful contradiction usually lowers confidence or triggers verification before downgrading state.

Downgrade becomes warranted when recent valid observations at comparable or lower previously verified demand repeatedly contradict the old claim across meaningful contexts.

Staleness alone usually triggers review/confidence decay, not immediate mastery loss.

## Graph behavior

B2 edges are diagnostic/routing information only. They never create learner evidence or propagate mastery.

A prerequisite failure may make a downstream node `not_observed`; it does not automatically make the downstream node weak.

## Long-form artifacts

The policy is evidence-topology based rather than question-count based. For costly writing/long-form tasks, multiple independently assessable versions, sections, or artifact episodes can contribute distinct observations when they genuinely expose the same canonical node under varied conditions. The system must not require repeated full essays merely to satisfy a numeric item quota.

## Consequences

### Positive

- Profile changes become explainable and auditable;
- understanding vs automation vs complexity remain separable;
- minimal intervention gains become measurable;
- recommendation can distinguish weak state from weak confidence;
- over-drilling near-identical questions does not fabricate mastery;
- regression/forgetting can be represented without erasing history.

### Costs

- update logic is more complex than percentage scoring;
- automated assessment quality/provenance matters;
- confidence and diversity need explicit bookkeeping;
- empirical calibration may later refine thresholds.

## Rejected alternatives

### One weighted learner score
Rejected because it hides qualitatively different evidence and makes tutoring actions hard to explain.

### Correct answer count thresholds only
Rejected because five H6 near-identical successes do not equal two independent unfamiliar transfer successes.

### Never downgrade
Rejected because Profile represents current usable state, not permanent achievement badges.

### Automatically lower state when evidence gets old
Rejected because staleness indicates uncertainty/review need; actual performance evidence is needed before normal mastery downgrade.

### Propagate mastery through prerequisite/transfer edges
Rejected because graph relations are not learner evidence.

### Require every state transition sequentially
Rejected because state is an evidence claim, not a gamified unlock ladder.