# ADR-014 — Retention, review and spaced verification are evidence-freshness policies

Status: **proposed / F3**

## Context

ChineseTutor now has:

- canonical LearningNodes and dependency graph;
- personal LearnerNodeState;
- TrainingAttempt evidence;
- evidence-driven Profile projection;
- task complexity;
- TrainingMove recommendation;
- adaptive within-question intervention.

The remaining temporal problem is that a state such as `M2` can become old. We need to know when to seek fresh evidence without pretending that elapsed time itself proves forgetting.

## Decision

### 1. Time changes freshness, not mastery directly

Elapsed time may change:

- review status;
- freshness confidence;
- forgetting-risk estimate;
- priority of a verification move.

Elapsed time alone must not change:

- Mastery M0–M3;
- Automation A0–A3;
- verified Complexity C0–C5.

Only new learner evidence interpreted through C3 can change those state estimates.

### 2. Review debt is axis-specific

Mastery, automation and complexity have separate verification clocks because the evidence that supports them can differ.

### 3. Historical evidence strength and freshness confidence are distinct

A strong evidence set remains historically strong even when old. F3 derives freshness confidence instead of rewriting old evidence as weak.

### 4. Retention profiles are semantic/configurable policy

Review cadence varies by evidence shape:

- exact retrieval;
- recitation;
- conceptual knowledge;
- reasoning ability;
- strategy;
- writing production.

Default horizons are operational configuration and must not be presented as universal cognitive laws.

### 5. Cheapest valid verification precedes reteaching

A stale established node receives the smallest task that can validly re-observe the due axis. Full reteaching requires evidence of current need.

### 6. Ordinary training can satisfy review

If a normal TrainingAttempt genuinely re-observes the node/axis, it can count as implicit review. Passive exposure cannot.

### 7. Failed review remains ordinary evidence

A failed review probe becomes C2 evidence and enters C3 contradiction handling. F3 has no hidden decay/downgrade path.

### 8. F1 remains the global prioritizer

F3 emits temporal review needs. F1 compares them against bottlenecks, new development, ActiveRequirements, dependency leverage and Question supply.

### 9. F2 remains the within-question controller

Review questions receive the same minimal-effective-intervention rules. Needing help during review is recorded honestly rather than suppressed to protect the review result.

## Consequences

### Positive

- Profile does not become falsely monotonic/permanent;
- old mastery is not erased merely by a clock;
- review is individualized by construct semantics;
- maintenance can be embedded in authentic work;
- review remains auditable and explainable;
- the system can later calibrate horizons from real learner history without schema replacement.

### Costs

- more derived temporal fields;
- policy versioning is required;
- review scheduling needs queue controls to avoid maintenance flood;
- the system must distinguish valid observation from passive co-occurrence.

## Rejected alternatives

### A. Automatic numeric mastery decay
Rejected because time is not direct evidence of current failure and a universal decay constant would create false precision.

### B. One spaced-repetition interval for every node
Rejected because exact recitation, lexical retrieval, reasoning and writing require different evidence shapes.

### C. Separate review engine that bypasses F1
Rejected because maintenance must compete transparently with higher-value bottleneck/development work.

### D. Treat every normal exposure as review
Rejected because seeing a construct is not the same as independently performing/retrieving it.

## Validation

F3 QA must demonstrate:

- M2 can be overdue without being downgraded;
- mastery can be current while automation is due;
- a normal authentic task can satisfy review when it re-observes the node;
- passive exposure cannot;
- failed review routes through C2/C3;
- regressed prerequisites can escalate only after evidence/causal consequence;
- review defaults are configurable, not grade-based.