# ADR-007 — Learner Profile is a dynamic evidence projection

Status: **proposed in C1; semantic self-review complete**

## Context

The v1 ability map stores personal fields such as mastery, automation, recent training and needs-improvement beside the semantic ability definition. That prevents clean multi-learner support and makes canonical graph meaning change with one learner's history.

ChineseTutor also needs to distinguish several realities that a single score cannot represent:

- can perform but does not self-trigger;
- apparently strong but supported by little evidence;
- previously strong but now review-due;
- weak but narrow/low-impact;
- moderately weak but a high-impact graph bottleneck;
- completely untested rather than actually weak.

## Decision

ChineseTutor uses one `LearnerNodeState` current record per `(learner_id, node_id)`.

The record is a dynamic projection of learning evidence over the canonical graph. It never changes the meaning of `LearningNode`.

Core observed dimensions:

```text
Mastery M0-M3
Automation A0-A3
Verified Complexity C0-C5
per-axis Evidence Strength
per-axis Verification Recency
Evidence Provenance
Forgetting Risk
Stable Error Pattern
```

Operational labels are derived from these states plus graph/goal context.

## Unknown is explicit

`null/unknown` is not M0/A0/C0.

A newly added or never-tested graph node must remain unknown until evidence exists. This prevents graph expansion toward Gaokao coverage from falsely creating hundreds of learner deficits.

## Mastery and automation are independent

Mastery answers:

> Can the learner perform/know this successfully and independently?

Automation answers:

> Does the learner reliably access/trigger/execute it without prompting or excessive friction?

Therefore `M2/A1`, `M1/A2`, and other mismatches are valid and pedagogically important.

## Profile state is not monotonic

Current estimates may move upward or downward as evidence changes. Forgetting, transfer failure, over-scaffolded historical success, and renewed fluency are all real.

The system must not encode “mastery only increases”.

## Evidence confidence and verification are per axis

A single evidence-strength field was rejected during C1 review because Mastery, Automation and Complexity can be supported by different observations.

Example:

```text
mastery: M2
mastery_evidence_strength: strong

automation: A1
automation_evidence_strength: weak
```

Likewise, verification timestamps are separate:

```text
mastery_verified_at
automation_verified_at
complexity_verified_at
```

An untimed independent answer may verify mastery while providing little evidence about automation under pressure.

C1 uses ordinal evidence strength rather than pseudo-precise numbers. C2/F1 may later compute richer confidence internally.

## Historical counts distinguish unknown from zero

Attempt/success counters are derived caches once C2 exists. During legacy migration, `null` means historical count is unknown, while `0` means the event source positively contains zero events.

This prevents migration gaps from masquerading as lack of training.

## Operational statuses are orthogonal

The product terms:

```text
Stable
Developing
Bottleneck
Review Due
Unknown
```

mix different dimensions. C1 separates:

```text
readiness_status: unknown | developing | stable
bottleneck_status: unknown | none | candidate | confirmed
review_status: unknown | current | due | overdue
```

and optionally derives a convenience `attention_status` for dashboards.

This preserves cases such as “developing + bottleneck + review due”.

## Stable is requirement-relative

A node is not universally “finished”. `stable` means current evidence meets an active target requirement.

The same observed M2/C2 state might be stable for today's training objective but below a later C4/C5 Gaokao requirement.

Goal comparison belongs outside the canonical node definition.

## Bottleneck is graph-aware

A bottleneck is not merely the weakest node. It combines learner deficit/uncertainty with downstream graph impact and causal plausibility.

Exact scoring is deferred to recommendation work, but C1 reserves the semantic distinction now.

## Current Profile is not the evidence ledger

The current state is a projection/materialized view. TrainingAttempt and artifact history remain the source evidence once C2 exists.

Profile values can therefore be recomputed, audited and revised without losing learning history.

## Legacy backfill is conservative

One-to-one v1 rows can seed weak-confidence state.

Mixed/split rows do not copy the same mastery number to every new child node.

Task-Type-only rows do not create fake profile nodes.

Cross-domain merges do not fabricate M3 transfer.

`最近训练` maps to `last_trained_at`, not to any verification timestamp.

`待加强` becomes an attention note, not an automatic bottleneck.

Historical counts may remain null rather than being fabricated as zero.

## Consequences

### Positive

- supports multiple learners cleanly;
- preserves understanding-vs-automation differences;
- distinguishes unknown from weak;
- preserves confidence differences across dimensions;
- supports forgetting and review;
- supports graph-aware bottleneck detection later;
- makes recommendation and tutoring explainable;
- enables state evolution in both directions.

### Costs

- profile becomes multi-dimensional rather than one simple score;
- operational dashboard labels must be derived carefully;
- C2 must provide trustworthy event evidence before automated updates are strong;
- migration cannot blindly copy all current Notion fields.

## Rejected alternatives

### Keep mastery on LearningNode
Rejected because it makes canonical graph meaning learner-specific and prevents multi-learner state.

### Default every new node to M0
Rejected because absence of evidence is not demonstrated weakness.

### Use one percentage per ability
Rejected because it collapses mastery, automation, complexity, confidence, recency and error pattern into an opaque number.

### Use one evidence confidence/timestamp for the whole state
Rejected during self-review because different observations verify different axes.

### Make Stable/Developing/Bottleneck/ReviewDue one persisted exclusive truth
Rejected because these concepts are not semantically exclusive. Orthogonal dimensions are preserved and a UI label may be derived.

### Never downgrade mastery
Rejected because learner state is an estimate, not an achievement badge.

### Copy v1 split-node mastery to every child
Rejected because mixed v1 rows intentionally hide different underlying constructs.
