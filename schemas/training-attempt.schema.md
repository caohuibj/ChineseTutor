# TrainingAttempt evidence-event schema

Status: **C2 normative draft**

`TrainingAttempt` is the lightweight event representing one submitted learner response to one concrete Question/task instance under a known amount of support.

It is the primary event source for learner evidence. It is intentionally much smaller than a durable learning recap.

---

## 1. Event identity and grouping

Each submitted response is one event.

```yaml
attempt_id: ATT-...
attempt_series_id: SERIES-...
learner_id: string
question_id: string | null
legacy_question_ref: string | null
attempt_number: int
previous_attempt_id: string | null
session_id: string | null
```

### `attempt_series_id`
Groups successive responses to the **same concrete question/task instance** during one learning cycle.

Example:

```text
SERIES-001
  ATT-001 attempt_number=1  first answer
  ATT-002 attempt_number=2  after one guiding question
  ATT-003 attempt_number=3  after a strategy reminder
```

A new different Question creates a new series even in the same tutoring session.

### `question_id`
References the canonical Question entity once the Material/Question PR exists.

During migration/legacy reconstruction, `question_id` may be null only if `legacy_question_ref` is present.

At least one of `question_id` or `legacy_question_ref` is required.

---

## 2. Target/evidence mapping to the Learning Graph

A Question may involve multiple canonical nodes. One attempt must therefore distinguish **intended target nodes** from **actual evidence observed for each node**.

```yaml
target_node_ids: string[]
node_evidence:
  - node_id: string
    role: primary_target | secondary_target | prerequisite | strategy | incidental
    observation: not_observed | negative | mixed | positive
    independence: unknown | guided | partial | independent
    note: string | null
```

### Why per-node evidence exists

A multi-node question might show:

- correct text location;
- weak reasoning;
- strong written expression;
- successful Strategy invocation;
- missing prerequisite knowledge.

The system must not copy one attempt-level correctness score to every target node.

For low-overhead routine use, a single-target attempt usually needs only one or two `node_evidence` entries.

`not_observed` means the node was relevant/targeted but the attempt did not provide enough evidence about it. It is not negative evidence.

---

## 3. Attempt context snapshot

Evidence quality depends on task conditions, so the event records a lightweight snapshot rather than relying only on mutable Question metadata.

```yaml
completed_at: datetime
started_at: datetime | null
elapsed_seconds: int | null
time_limit_seconds: int | null

material_familiarity: familiar | partially_familiar | unfamiliar | unknown
transfer_probe: bool
legacy_reconstructed: bool

complexity_band_snapshot: null | C0 | C1 | C2 | C3 | C4 | C5
complexity_vector_snapshot: object | null
question_variant_group_id: string | null
```

### `material_familiarity`
Critical for transfer evidence. Familiar textbook success cannot be silently treated as unfamiliar transfer.

### `transfer_probe`
True only when the attempt was intentionally suitable for testing transfer/generalization of one or more target nodes.

This does not itself prove transfer success; `node_evidence` and performance still matter.

### `legacy_reconstructed`
Marks events reconstructed from old learning records rather than captured live. These events can seed evidence but should not be treated as equally precise to native C2 telemetry.

### `question_variant_group_id`
Optional family identifier for near-identical variants. Later evidence aggregation can avoid counting five trivial rewrites as five independent demonstrations.

---

## 4. Hint / intervention model

### 4.1 Maximum hint level before this response

```yaml
max_hint_level_before_response: null | H0 | H1 | H2 | H3 | H4 | H5 | H6 | H7
```

Native C2 events should always record H0-H7. `null` is allowed only for legacy reconstructed evidence where the historical hint level is unknown.

```text
H0  no hint
H1  task-type reminder
H2  strategy/model reminder
H3  one key guiding question
H4  relevant text range/location supplied
H5  critical evidence supplied
H6  partial reasoning supplied
H7  near-answer / model answer exposure
```

This field records the strongest support available **before the learner response represented by this event**.

It is not “how much help was given in the whole session”.

### 4.2 Intervention immediately before the attempt

```yaml
intervention_type: null | none | task_reminder | strategy_reminder | guiding_question | text_location | evidence_supply | partial_reasoning | near_answer
intervention_summary: string | null
```

Keep `intervention_summary` short. Example:

> 只追问“为什么在天子劳军这一特殊情境下仍按军令办事更能说明品质？”

Do not paste the whole tutor conversation into every event.

---

## 5. Diagnostic scoring dimensions

Each dimension uses:

```text
null = not assessed / not applicable
0    = failed / absent
1    = partial / unstable
2    = successful for this attempt condition
```

Required schema fields:

```yaml
question_understanding: null | 0 | 1 | 2
task_type_recognition: null | 0 | 1 | 2
text_location: null | 0 | 1 | 2
evidence_selection: null | 0 | 1 | 2
reasoning: null | 0 | 1 | 2
terminology: null | 0 | 1 | 2
written_expression: null | 0 | 1 | 2
answer_correctness: null | 0 | 1 | 2
```

### Why `null` is mandatory

A narrative-writing task may not have `text_location`.

An open composition does not have simple answer correctness comparable to a vocabulary item.

`null` prevents “not applicable” from being misread as failure.

### `question_understanding`
Added beyond the original minimum fields because Q-type failure must be distinguishable from downstream reading/reasoning failure.

### `task_type_recognition`
Whether the learner independently identified what operation/task family was required when this is meaningfully observable.

### `text_location`
Whether the learner located/selectively returned to the relevant text region/material.

### `evidence_selection`
Whether relevant and sufficient evidence was chosen.

### `reasoning`
Whether explicit inferential/relational steps validly connect evidence to conclusion.

### `terminology`
Whether useful subject terminology/concepts were accurate where required.

### `written_expression`
Whether the reasoning could be converted into complete, concise, scoreable written language.

### `answer_correctness`
Overall result quality for tasks where such a judgment is meaningful. It never replaces the diagnostic dimensions.

---

## 6. Error coding

```yaml
error_codes: [K, R, I, E, Q, M, C]
primary_error_code: null | K | R | I | E | Q | M | C
error_note: string | null
```

Codes:

```text
K knowledge
R reading/location
I inference/reasoning
E expression
Q question understanding
M method/strategy invocation
C carelessness/execution
```

Multiple codes may coexist.

`primary_error_code` should identify the earliest or most causally important failure for this attempt when that can be judged.

One attempt-level error does not automatically become C1 `primary_error_pattern`.

---

## 7. Attempt-to-attempt delta

Do **not** store “next attempt delta” on an earlier event because it requires future knowledge.

Each later response stores its change from the previous response:

```yaml
delta_from_previous: null | worse | unchanged | partial_improvement | major_improvement | resolved
delta_summary: string | null
```

Example:

```text
ATT-001 reasoning=1, expression=0
ATT-002 reasoning=2, expression=2
        max_hint=H3
        delta_from_previous=resolved
```

This makes second-attempt learning visible without turning events into long narratives.

---

## 8. Learner response / tutor feedback payload

Optional lightweight fields:

```yaml
raw_answer: string | null
feedback_summary: string | null
```

Operational systems may retain raw answers where appropriate. GitHub fixtures should avoid committing private learner content unnecessarily.

`feedback_summary` should record the key diagnosis/intervention outcome, not duplicate the whole recap.

Example:

> 方向正确；缺“特殊情境为什么让这一选择更有辨识度”的显性解释。

---

## 9. Evidence interpretation principles

C2 defines event semantics but does not yet implement a full Profile scoring formula.

However, downstream update logic must respect these rules.

### 9.1 Final correctness under heavy support is not independent mastery

```text
H5-H7 + correctness 2
```

can show learning/progress but cannot by itself justify M2 independent performance.

### 9.2 Hint reduction is positive automation evidence

Comparable tasks showing:

```text
H5 -> H3 -> H1/H0
```

with stable quality are strong evidence of decreasing scaffold dependence.

### 9.3 First and second attempts have different evidence meanings

A second attempt after a useful H3 prompt can demonstrate successful repair/learning, but its independence classification differs from an H0 first attempt.

### 9.4 Familiarity/transfer must be explicit

M3-like transfer claims need unfamiliar/diverse evidence; textbook familiarity cannot be ignored.

### 9.5 Similarity matters

Many attempts from the same variant group should not be treated as independent diversity evidence.

### 9.6 Diagnostic dimensions can contradict overall correctness

Example:

```text
correctness 2
reasoning 0
```

may indicate guessing, copied wording, or an answer with no visible reasoning where reasoning is required.

Profile updates should use the relevant dimensions/node evidence, not correctness alone.

---

## 10. Low-overhead logging profile

For a routine short-answer attempt, the minimum operational capture can be:

```yaml
question/legacy ref
attempt_series_id + attempt_number
target_node_ids
max_hint_level_before_response
4-8 applicable diagnostic scores
error_codes/primary error if any
one or two node_evidence entries
completed_at
```

All long prose fields are optional.

The goal is approximately a few structured selections plus a short note when needed, not a new essay about every question.

---

## 11. Relationship to durable Session Summary

```text
TrainingAttempt = raw structured event
Session Summary / 学习记录 = promoted durable synthesis
```

Promote to durable summary only when there is something worth preserving:

- new reusable strategy;
- stable error pattern;
- major breakthrough;
- transfer success/failure;
- unit/topic closure;
- significant writing revision.

Do not create a long Learning Record for every Attempt.

---

## 12. C2 invariants

1. One submitted response = one TrainingAttempt event.
2. Successive responses to one question share `attempt_series_id` and increment `attempt_number`.
3. `null` diagnostic score means not observed/applicable, never failure.
4. Native events record H0-H7; legacy reconstructions may have unknown hint level.
5. Strong hints reduce independence evidence even when the final answer is correct.
6. Attempt-level scores do not automatically apply equally to every target node.
7. Per-node evidence is explicit when Profile updates need node-specific interpretation.
8. Second-attempt improvement is stored on the later attempt as delta-from-previous.
9. Transfer/familiarity/complexity context is snapshotted with evidence.
10. TrainingAttempt remains lightweight and does not replace durable recap artifacts.
