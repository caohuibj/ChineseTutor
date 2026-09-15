# LearnerNodeState canonical profile schema

Status: **C1 normative draft — semantic self-review complete**

This document defines the current personal state of one learner on one canonical `LearningNode`.

`LearningNode` answers **what the construct means**. `LearnerNodeState` answers **what current evidence suggests about this learner on that construct**.

The profile is a dynamic overlay. It must never redefine the canonical node.

---

## 1. Identity

One current state record exists per:

```text
(learner_id, node_id)
```

Suggested machine identity:

```text
LNS-<LEARNER_ID>-<NODE_ID>
```

`node_id` always references the stable B1 canonical ID. If a Notion page is recreated, the profile identity does not change.

Multiple learners can have independent states for the same node without any schema change.

---

## 2. Unknown is not M0

Absence of evidence must never be converted into weakness.

Therefore these fields are nullable:

```yaml
mastery: null | M0 | M1 | M2 | M3
automation: null | A0 | A1 | A2 | A3
verified_complexity_band: null | C0 | C1 | C2 | C3 | C4 | C5
```

`null` means **insufficient evidence / unknown**.

`M0` / `A0` mean there is positive evidence of non-establishment / non-automatic access.

This distinction is mandatory. Otherwise graph expansion toward full Gaokao coverage would create artificial learner deficits before nodes were ever tested.

---

## 3. Persisted current-state fields

### 3.1 Primary estimate fields

```yaml
learner_state_id: string
learner_id: string
node_id: string

mastery: null | M0 | M1 | M2 | M3
automation: null | A0 | A1 | A2 | A3
verified_complexity_band: null | C0 | C1 | C2 | C3 | C4 | C5

mastery_evidence_strength: none | weak | moderate | strong
automation_evidence_strength: none | weak | moderate | strong
complexity_evidence_strength: none | weak | moderate | strong
state_source: legacy_backfill | attempt_derived | mixed | manual_override

last_trained_at: datetime | null
mastery_verified_at: datetime | null
automation_verified_at: datetime | null
complexity_verified_at: datetime | null

manual_override_reason: string | null
state_updated_at: datetime
```

Evidence confidence and verification timestamps are per axis because mastery, automation and complexity can be supported by different observations.

A single `last_verified_at` or `evidence_strength` would falsely imply that evidence for one dimension verifies all dimensions.

### 3.2 Derived/cached evidence summaries

The following may be stored as materialized/cache fields for operational convenience but are **not source-of-truth evidence**:

```yaml
attempt_count: int | null
independent_success_count: int | null
transfer_success_count: int | null

forgetting_risk: unknown | low | medium | high
primary_error_pattern: null | K | R | I | E | Q | M | C
error_pattern_evidence_strength: none | weak | moderate | strong
error_pattern_note: string | null

attention_reason: string[]
```

`null` counts mean historical quantity is unknown (common during v1 backfill). `0` means the system positively knows there are zero counted events in the C2 event source.

### 3.3 Derived operational view fields

These may be materialized/cache fields in Notion or code, but they are derived from Profile + B2 graph + active goal/review context:

```yaml
readiness_status: unknown | developing | stable
bottleneck_status: unknown | none | candidate | confirmed
review_status: unknown | current | due | overdue
attention_status: unknown | stable | developing | bottleneck | review_due
```

The dimensions remain separate because one node can simultaneously be developing, high-impact, and overdue for review.

---

## 4. Mastery M0–M3

Mastery measures **quality and independence of successful performance**, not speed.

### Shared semantics

```text
M0  not established; evidence shows the learner cannot yet perform/recall the construct reliably
M1  succeeds with meaningful support or in tightly familiar/constrained conditions
M2  independently succeeds on representative routine/authentic tasks
M3  demonstrates stable generalization/transfer across sufficiently varied unfamiliar contexts
```

### Type-aware interpretation

#### Knowledge

- `M0`: cannot reliably recognize/recall/explain the knowledge even with ordinary cueing.
- `M1`: recognizes or recalls with prompts / familiar phrasing.
- `M2`: independently retrieves and correctly applies/distinguishes the knowledge in representative contexts.
- `M3`: knowledge remains accurate across delayed, varied or unfamiliar applications and does not collapse under nearby confusions.

#### Ability

- `M0`: cannot yet carry out the observable operation reliably.
- `M1`: completes the operation with meaningful scaffold.
- `M2`: independently performs it on representative authentic tasks.
- `M3`: transfers it stably across unfamiliar materials/task variants.

#### Strategy

- `M0`: cannot execute the procedure meaningfully even when named.
- `M1`: executes it with step reminders or guided prompting.
- `M2`: executes it correctly and can use it on representative tasks.
- `M3`: flexibly adapts the strategy across varied tasks without rigid template behavior.

A single success must not produce `M3`.

---

## 5. Automation A0–A3

Automation measures **trigger/access/execution fluency**, not conceptual mastery.

```text
A0  does not reliably access or invoke the construct when needed
A1  accesses/invokes after an explicit reminder or cue
A2  independently recognizes when to access/invoke it in normal conditions
A3  accesses/invokes reliably with low friction under time/attention pressure
```

Type-aware reading:

- Knowledge: retrieval/access fluency.
- Ability: recognizing the needed operation and executing it without process prompting.
- Strategy: self-triggering and running the procedure without scaffold.

Mastery and automation are intentionally independent.

Valid examples:

```text
M2 / A1  can perform once activated, but still needs a reminder to invoke it
M1 / A2  quickly invokes a familiar procedure, but execution quality remains weak
M3 / A2  transfers well, but is not yet fully automatic under pressure
```

A profile system that forces `M == A` fails C1.

---

## 6. Verified complexity band

`verified_complexity_band` is the highest complexity band currently supported by sufficiently credible learner evidence.

It is **not**:

- the hardest question ever attempted or answered once;
- an intrinsic property of the node;
- a grade label;
- guaranteed to increase monotonically.

Bands follow the architecture summary:

```text
C0 recognition
C1 single-step application
C2 multi-step explanation
C3 integrated multi-node task
C4 unfamiliar transfer
C5 open evaluation / complex construction
```

The detailed task complexity vector remains Question/Attempt data. C1 stores only the current verified summary band.

If evidence is old, contradictory, or heavily scaffolded, the verified band may remain lower than the maximum attempted band.

---

## 7. Evidence strength is per axis

C1 deliberately uses ordinal evidence strength instead of false numerical precision:

```text
none      no usable evidence
weak      sparse, old, heavily scaffolded, near-identical, or legacy-only evidence
moderate  multiple reasonably independent/representative observations
strong    diverse, recent, independent evidence including relevant transfer/retention checks
```

C2 will define how TrainingAttempt events contribute to the three axis-specific fields:

```text
mastery_evidence_strength
automation_evidence_strength
complexity_evidence_strength
```

Important states are valid:

```text
mastery M2 + mastery evidence strong
automation A1 + automation evidence weak
```

This means ability quality is well supported while self-trigger behavior still needs verification.

C1 does not define a scoring formula.

---

## 8. Counts are derived caches, not source-of-truth evidence

```text
attempt_count
independent_success_count
transfer_success_count
```

exist for fast profile inspection and migration compatibility.

After C2 exists, they must be computed from TrainingAttempt evidence rather than manually incremented as authoritative facts.

During legacy migration they may be `null` because historical count is unknown.

A count alone never proves mastery:

```text
20 near-identical scaffolded attempts != M3
```

Evidence diversity and hint dependence matter.

---

## 9. Recency and forgetting

### `last_trained_at`
Most recent meaningful interaction involving the node. Training may include scaffolded practice and does not necessarily verify mastery.

### Axis-specific verification timestamps

```text
mastery_verified_at
automation_verified_at
complexity_verified_at
```

These may differ. For example, an untimed answer can verify mastery without verifying timed automation.

### `forgetting_risk`
Dynamic derived estimate:

```text
unknown | low | medium | high
```

C1 defines semantics but not decay formula.

Different node families will decay differently:

- lexical facts, memorization and cultural knowledge: strong recency sensitivity;
- reasoning Abilities: more transfer/usage sensitivity;
- Strategies: automation/hint-use sensitivity;
- writing production: artifact/version evidence sensitivity.

The eventual risk model belongs to review/recommendation work.

---

## 10. Error pattern

Existing diagnostic codes are preserved:

```text
K knowledge
R reading/location
I inference/reasoning
E expression
Q question reading
M method/strategy
C carelessness/execution
```

`primary_error_pattern` is a derived current summary, not a replacement for attempt-level errors.

`error_pattern_evidence_strength` prevents one-off mistakes from masquerading as stable patterns.

`error_pattern_note` describes the concrete stable pattern, e.g.:

> 结论和证据都有，但经常省略“为什么该证据支持结论”的解释句。

---

## 11. Operational statuses are derived, not evidence primitives

The original C1 issue named:

```text
Stable
Developing
Bottleneck
Review Due
Unknown
```

These labels mix different dimensions:

- Stable / Developing = readiness/development state;
- Bottleneck = graph impact + causal weakness;
- Review Due = recency/forgetting;
- Unknown = evidence absence.

C1 therefore keeps orthogonal derived dimensions and exposes one convenience `attention_status` for UI/queues.

### `readiness_status`

```text
unknown
developing
stable
```

`stable` is relative to an external active requirement/target, not a permanent intrinsic property. A node can be stable for the current training target while still below eventual Gaokao complexity.

### `bottleneck_status`

```text
unknown
none
candidate
confirmed
```

A weak node is not automatically a bottleneck. Bottleneck requires:

1. learner evidence of a meaningful deficit or uncertainty;
2. graph evidence that the node constrains important downstream nodes;
3. plausible causal connection to observed downstream failures or unlock value.

### `review_status`

```text
unknown
current
due
overdue
```

Depends on recency and node-family forgetting behavior.

### `attention_status`

A convenience derived label for a dashboard. Suggested precedence:

```text
unknown
> bottleneck
> review_due
> developing
> stable
```

Secondary dimensions remain visible so information is not lost.

---

## 12. State source and manual override

### `state_source`

```text
legacy_backfill
attempt_derived
mixed
manual_override
```

This makes profile provenance explicit.

### Manual override

Manual override is allowed only for exceptional operational correction and requires `manual_override_reason`.

It must not erase underlying Attempt history. Later recomputation can compare evidence-derived state with override state.

---

## 13. Profile update invariants

1. No evidence -> `mastery/automation/complexity = null`, not `M0/A0/C0`.
2. Mastery and automation are independent axes.
3. One correct item cannot establish `M3`.
4. Highest attempted complexity is not automatically verified complexity.
5. Transfer success updates only actually evidenced constructs; B2 `transfers_to` never auto-grants target mastery.
6. Hard prerequisite mastery never auto-grants downstream mastery.
7. Parent/child mastery is not automatically copied in either direction.
8. Counts are supporting summaries, not mastery formulas.
9. Current state may downgrade when recent contradictory evidence is sufficiently strong.
10. Verification/evidence confidence is tracked per M/A/C axis.
11. Profile history must remain reconstructable from evidence; current state is a projection, not the sole learning record.

---

## 14. Fields forbidden from LearnerNodeState

Canonical meaning belongs elsewhere:

```text
node definition
node aliases
node domain/subdomain definition
canonical prerequisites
canonical strategy relation
question prompt
reference answer
rubric
```

Final scheduling action belongs to TrainingMove rather than Profile:

```text
next_question_id
exact_training_move
scheduled_at
```

Profile may expose `attention_reason`, but recommendation choice belongs to the recommendation layer.

---

## 15. C1 invariants

1. `(learner_id, node_id)` uniquely identifies current state.
2. Unknown and M0/A0 are distinct.
3. M, A and C may move independently.
4. Evidence confidence and verification recency are per axis.
5. State provenance is explicit.
6. Dynamic operational statuses are derived from underlying evidence/profile + graph/goal context.
7. v1 state can be backfilled without copying ambiguous split-node mastery blindly.
8. Current state can change both upward and downward.
9. No grade/year is a mastery dimension.
10. No recommendation may treat low-confidence legacy backfill as equivalent to strong attempt-derived evidence.
