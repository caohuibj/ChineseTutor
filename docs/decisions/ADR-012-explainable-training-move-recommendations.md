# ADR-012 — Recommend explainable TrainingMoves before concrete Questions

Status: **proposed in F1**

## Context

ChineseTutor now has:

- canonical LearningNodes and dependency edges;
- learner-specific Profile state;
- structured Attempt evidence;
- replayable evidence→Profile update policy;
- separate Material / TaskType / Question entities;
- intrinsic Question complexity independent of grade/novelty.

The missing layer is deciding what the learner should do next.

A naive recommender could select the lowest-scoring ability, the next school chapter, or the highest-ranked available Question. All three would violate the project's graph-first/evidence-first design.

## Decision

ChineseTutor recommends a learner-specific `TrainingMove` first, then selects a concrete Question that satisfies the move.

```text
Profile + graph + requirements + recent evidence
                 ↓
            TrainingMove
                 ↓
       Question eligibility/fit
                 ↓
             Attempt
                 ↓
         C3 Profile update
```

A TrainingMove contains:

- target node(s);
- move type;
- reason/priority class;
- TaskType/material/complexity constraints;
- planned hint ceiling;
- transfer/familiarity conditions;
- success criterion;
- provenance.

## Explainable priority instead of one opaque score

F1 uses:

```text
eligibility gates
→ priority classes
→ ordered semantic tie-breaks
```

Primary factors include learner gap, graph unlock value, Gaokao relevance, evidence uncertainty, forgetting risk, transfer need, current requirement, question supply, and cost.

This permits a high-leverage prerequisite to outrank a lower-scoring isolated weakness without relying on a hidden weighted score.

## ActiveRequirement is separate from Profile

Observed state cannot define its own target.

F1 therefore consumes an explicit requirement contract:

```text
current observed state
vs
current / long-term desired evidence state
```

A node can be stable for the current training target while still below its long-term Gaokao requirement.

This preserves progression without returning to grade-based staging.

## Prerequisite semantics

A demonstrated hard prerequisite deficit may block independent downstream training when the downstream construct cannot be validly observed.

Unknown prerequisite state is not treated as failure; cheap diagnosis is preferred when uncertainty matters.

`supports` edges never become hard blocks.

## Minimal Effective Intervention

TrainingMove controls the evidence conditions rather than simply “difficulty”:

- hint ceiling;
- complexity-vector dimensions;
- familiarity/transfer;
- response mode;
- TaskType/material family;
- timing.

This supports targeted changes such as:

```text
hold C2 reasoning constant
H3 -> H0
```

or:

```text
hold reasoning constant
raise expression load
```

rather than generic harder/easier sequencing.

## Transfer is distinct from complexity

F1 follows D2:

- complexity is intrinsic Question demand;
- novelty is learner-relative Attempt context.

A C2 unfamiliar H0 task may be the correct transfer probe. A familiar C4 task is not transfer evidence merely because it is hard.

## Question selection is a second stage

Question inventory affects executability but not learner need.

If no eligible Question exists:

- preserve the high-value need;
- emit a content-supply gap;
- optionally execute the next-best move.

Authentic traceable Questions are preferred for transfer/high-stakes validation; short labeled generated/editorial probes are valid for efficient narrow diagnosis.

## Recommendation stability

Recommendation is recomputed after meaningful evidence, not after every trivial event.

A move persists while its evidence purpose remains unresolved and no materially higher-value blocker appears.

This prevents recommendation thrashing.

## Maintenance escalation

Review/maintenance is normally lower-cost background work, but overdue/regressed core prerequisites can escalate when they threaten active downstream performance.

This prevents low-priority maintenance from being starved until it becomes a larger failure.

## Consequences

### Positive

- personalized next-step decisions are explainable;
- same TaskType can receive different scaffolding for different learners;
- high-centrality bottlenecks can outrank isolated weaknesses;
- school materials integrate without becoming the progression axis;
- hints, novelty, complexity and expression load can be manipulated independently;
- question-bank shortages become visible as supply gaps;
- the tutoring engine can pursue minimal effective intervention.

### Costs

- recommendation requires explicit active requirements or calibrated defaults;
- graph quality matters, especially `requires` edges;
- priority decisions remain rule-based and require semantic QA;
- question inventory needs sufficiently rich metadata for matching;
- later empirical calibration may refine class/tie-break behavior.

## Rejected alternatives

### Recommend the lowest mastery node
Rejected because weakness is not the same as leverage, uncertainty, urgency or readiness.

### Follow school/grade order
Rejected because grade is source/context metadata, not the learning graph.

### Rank Questions directly
Rejected because one Question ID cannot express why it is being used, desired support, complexity manipulation or success criteria.

### Use one weighted priority score as canonical truth
Rejected because weights would create false precision and weaken explainability before empirical calibration.

### Treat all unknown prerequisites as blockers
Rejected because absence of evidence is not evidence of weakness.

### Always choose authentic exam questions
Rejected because authentic tasks are ideal for validation/transfer, while narrow diagnosis is often more efficient with a controlled short probe.