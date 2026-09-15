# TrainingAttempt evidence-event schema

Status: **C2 normative draft — semantic self-review complete**

`TrainingAttempt` is the lightweight event representing one submitted learner response to one concrete Question/task instance under known support conditions.

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

A different Question creates a new series even in the same tutoring session.

### `question_id`
References the canonical Question entity once the Material/Question PR exists.

During migration/legacy reconstruction, `question_id` may be null only if `legacy_question_ref` is present.

At least one of `question_id` or `legacy_question_ref` is required.

---

## 2. Target/evidence mapping to the Learning Graph

A Question may involve multiple canonical nodes. One attempt must distinguish **intended target nodes** from **actual evidence observed for each node**.

```yaml
target_node_ids: string[]
node_evidence:
  - node_id: string
    role: primary_target | secondary_target | prerequisite | strategy | incidental
    observation: not_observed | negative | mixed | positive
    independence: unknown | guided | partial | independent
    note: string | null
```

### Consistency rules

- every `primary_target` / `secondary_target` in `node_evidence` must appear in `target_node_ids`;
- prerequisite/strategy/incidental evidence may reference additional canonical nodes;
- one attempt may contain at most one `node_evidence` entry per `(node_id, role)`;
- a target node with insufficient valid evidence may still appear as `not_observed`.

### Why per-node evidence exists

A multi-node question might show:

- correct text location;
- weak reasoning;
- strong written expression;
- successful Strategy invocation;
- missing prerequisite knowledge.

The system must not copy one attempt-level correctness score to every target node.

For low-overhead routine use, a single-target attempt usually needs only one or two entries.

`not_observed` is not negative evidence.

---

## 3. Attempt context snapshot

Evidence quality depends on task conditions, so the event records a lightweight snapshot rather than relying only on mutable Question metadata.

```yaml
completed_at: datetime
started_at: datetime | null
elapsed_seconds: int | null
time_limit_seconds: int | null

material_id_snapshot: string | null
task_type_id_snapshot: string | null
material_familiarity: familiar | partially_familiar | unfamiliar | unknown
transfer_probe: bool
legacy_reconstructed: bool

complexity_band_snapshot: null | C0 | C1 | C2 | C3 | C4 | C5
complexity_vector_snapshot: object | null
question_variant_group_id: string | null

response_mode: written | typed_chat | oral | artifact | mixed | unknown
artifact_ref: string | null
```

### `material_id_snapshot` / `task_type_id_snapshot`
Preserve historical material/task diversity even if Question metadata is later retagged.

For legacy records these may be null if canonical entities do not yet exist.

### `material_familiarity`
Critical for transfer evidence. Familiar textbook success cannot be silently treated as unfamiliar transfer.

### `transfer_probe`
True only when the attempt was intentionally suitable for testing transfer/generalization of one or more target nodes.

This does not itself prove transfer success; `node_evidence` and performance still matter.

### `legacy_reconstructed`
Marks events reconstructed from old learning records rather than captured live. These can seed evidence but should not be treated as equally precise to native C2 telemetry.

### `question_variant_group_id`
Optional family identifier for near-identical variants. Later aggregation can avoid counting superficial rewrites as independent demonstrations.

### `response_mode`
Prevents modality from being mistaken for ability failure.

Examples:

- an oral reasoning attempt should not automatically receive `written_expression=0`;
- a full composition may be stored as an artifact/version and referenced through `artifact_ref`;
- typed chat may legitimately expose organization/expression but differs from timed handwritten production.

### `artifact_ref`
Optional reference to a writing draft/version or other production artifact. Long-form evidence should not require duplicating the whole artifact into every attempt event.

---

## 4. Hint / intervention model

### 4.1 Support before this response

```yaml
max_hint_level_before_response: null | H0 | H1 | H2 | H3 | H4 | H5 | H6 | H7
hint_count_before_response: int | null
```

Native C2 events should always record H0-H7. `null` hint level is allowed only for legacy reconstructed evidence where historical support is unknown.

`hint_count_before_response` is optional but useful when multiple hints at the same ordinal level were needed.

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

`max_hint_level_before_response` records the strongest support available **before the response represented by this event**.

It is not “how much help was given anywhere in the session”.

### 4.2 Intervention immediately before the attempt

```yaml
intervention_type: null | none | task_reminder | strategy_reminder | guiding_question | text_location | evidence_supply | partial_reasoning | near_answer
intervention_summary: string | null
```

Keep `intervention_summary` short. Do not paste the whole tutor conversation into every event.

---

## 5. Diagnostic scoring dimensions

Each dimension uses:

```text
null = not assessed / not applicable
0    = failed / absent
1    = partial / unstable
2    = successful for this attempt condition
```

Fields:

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

An oral attempt should normally leave `written_expression` null unless written output was actually required/observed.

An open composition does not have simple answer correctness comparable to a vocabulary item.

`null` prevents “not applicable” from being misread as failure.

### `question_understanding`
Separates Q-type failure from downstream reading/reasoning failure.

### `task_type_recognition`
Whether the learner independently identified what operation/task family was required when meaningfully observable.

### `text_location`
Whether the learner located/selectively returned to the relevant text region/material.

### `evidence_selection`
Whether relevant and sufficient evidence was chosen.

### `reasoning`
Whether explicit inferential/relational steps validly connect evidence to conclusion.

### `terminology`
Whether useful subject terminology/concepts were accurate where required.

### `written_expression`
Whether observed written output converts reasoning into complete, concise, scoreable language.

### `answer_correctness`
Overall result quality where meaningful. It never replaces diagnostic dimensions.

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

`primary_error_code` identifies the earliest or most causally important failure when that can be judged.

One attempt-level error does not automatically become C1 `primary_error_pattern`.

---

## 7. Attempt-to-attempt delta

Do **not** store “next attempt delta” on an earlier event because it requires future knowledge.

Each later response stores change from the previous response:

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

For long-form writing, prefer `artifact_ref` to duplicating the full draft.

`feedback_summary` records the key diagnosis/intervention outcome, not the whole recap.

---

## 9. Evidence interpretation principles

C2 defines event semantics but does not yet implement a full C1 Profile scoring formula.

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

`hint_count_before_response` can refine this when several same-level hints were needed.

### 9.3 First and second attempts have different evidence meanings

A second attempt after an H3 prompt can demonstrate successful repair/learning, but its independence differs from an H0 first attempt.

### 9.4 Familiarity/transfer must be explicit

M3-like claims need unfamiliar/diverse evidence; textbook familiarity cannot be ignored.

### 9.5 Similarity matters

Many attempts from the same variant group should not be treated as independent diversity evidence.

### 9.6 Modality matters

Oral insight is valid evidence of thinking but not automatically evidence of written organization. Timed writing is stronger automation/expression evidence than untimed chat.

### 9.7 Diagnostic dimensions can contradict overall correctness

Example:

```text
correctness 2
reasoning 0
```

may indicate guessing, copied wording, or an answer with no visible required reasoning.

Profile updates should use relevant dimensions/node evidence, not correctness alone.

---

## 10. Low-overhead logging profile

For a routine short-answer attempt, minimum operational capture can be:

```yaml
question/legacy ref
attempt_series_id + attempt_number
target_node_ids
response_mode
max_hint_level_before_response
4-8 applicable diagnostic scores
error_codes/primary error if any
one or two node_evidence entries
completed_at
```

Material/task/complexity metadata should normally be auto-filled from Question and snapshotted by the system.

All long prose fields are optional.

The goal is a few structured selections plus a short note when needed, not a new essay about every question.

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
5. Strong hints reduce independence evidence even when final answer is correct.
6. Attempt-level scores do not automatically apply equally to every target node.
7. Per-node evidence is explicit when Profile updates need node-specific interpretation.
8. Target-role node evidence is consistent with `target_node_ids`.
9. Second-attempt improvement is stored on the later event as delta-from-previous.
10. Material/task/familiarity/complexity context is snapshotted with evidence.
11. Response modality prevents oral/artifact work from being mis-scored as written failure.
12. Long-form production may reference an artifact/version instead of duplicating content.
13. TrainingAttempt remains lightweight and does not replace durable recap artifacts.
