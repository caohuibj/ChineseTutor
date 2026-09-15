# TrainingAttempt evidence-event schema

Status: **C2 normative draft — semantic self-review complete**

`TrainingAttempt` is the lightweight historical event representing one submitted learner response to one concrete Question/task instance under known support conditions.

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

`attempt_series_id` groups successive responses to the same concrete question/task instance during one learning cycle. A different Question creates a new series even in the same tutoring session.

At least one of `question_id` or `legacy_question_ref` is required. Native events should use canonical `question_id` once the Question entity exists; legacy reconstruction may use a traceable historical reference.

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

Consistency rules:

- every `primary_target` / `secondary_target` in `node_evidence` appears in `target_node_ids`;
- prerequisite/strategy/incidental observations may reference additional canonical nodes;
- at most one entry per `(node_id, role)`;
- `not_observed` means insufficient valid evidence, not failure.

Question-level correctness must never be copied blindly to every node.

---

## 3. Attempt context snapshot

Evidence meaning depends on conditions. The event snapshots relevant context instead of relying only on mutable Question metadata.

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

`material_id_snapshot` and `task_type_id_snapshot` preserve historical diversity/context even if canonical metadata is retagged later.

`material_familiarity` and `transfer_probe` are required to interpret generalization evidence. A different exam/grade is not automatically an unfamiliar transfer.

`question_variant_group_id` lets later aggregation discount superficial variants.

`response_mode` prevents modality from being mistaken for ability failure. Oral reasoning does not automatically imply `written_expression=0`. Long-form writing may reference a versioned artifact through `artifact_ref` rather than being duplicated into the event.

---

## 4. Hint / intervention model

```yaml
max_hint_level_before_response: null | H0 | H1 | H2 | H3 | H4 | H5 | H6 | H7
hint_count_before_response: int | null
intervention_type: null | none | task_reminder | strategy_reminder | guiding_question | text_location | evidence_supply | partial_reasoning | near_answer
intervention_summary: string | null
```

Native C2 events record H0-H7. `null` is allowed only for legacy reconstructed evidence where support is unknown.

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

The hint fields describe support available **before this response**, not maximum help anywhere in the session. `hint_count_before_response` is optional but distinguishes repeated prompts at the same ordinal level when useful.

---

## 5. Diagnostic scoring dimensions

Each dimension uses:

```text
null = not assessed / not applicable
0    = failed / absent
1    = partial / unstable
2    = successful for this attempt condition
```

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

`null` is mandatory for N/A/not-observed dimensions. For example, an oral response normally leaves `written_expression` null unless written output was also observed.

These are local observations under the recorded conditions, not global M0-M3 Profile levels.

---

## 6. Error coding

```yaml
error_codes: [K, R, I, E, Q, M, C]
primary_error_code: null | K | R | I | E | Q | M | C
error_note: string | null
```

```text
K knowledge
R reading/location
I inference/reasoning
E expression
Q question understanding
M method/strategy invocation
C carelessness/execution
```

Multiple codes may coexist. `primary_error_code` identifies the earliest/most causal failure when that is supportable. One Attempt error never automatically becomes the C1 stable error pattern.

---

## 7. Assessment provenance

The learner response is historical evidence; the diagnostic labels are an assessment of that evidence. Assessment provenance must therefore be auditable.

```yaml
assessed_by: tutor_ai | teacher | learner_self | mixed | legacy_unknown
assessment_confidence: unknown | low | medium | high
assessment_policy_version: string | null
```

### `assessed_by`
Who produced the diagnostic scores/node observations for this event.

### `assessment_confidence`
Confidence in the **assessment/tagging**, not the learner's mastery. A low-confidence assessment should not be treated like a high-confidence one in later Profile aggregation.

### `assessment_policy_version`
Optional identifier for the rubric/scoring policy used. This future-proofs recalibration without pretending old judgments were generated under today's policy.

For native AI-tutored events, `assessed_by=tutor_ai` is normal. Teacher review can later create an auditable correction/reassessment rather than silently erasing the original response history.

---

## 8. Attempt-to-attempt delta

Do not store future `next_attempt_delta` on an earlier event.

Each later response stores:

```yaml
delta_from_previous: null | worse | unchanged | partial_improvement | major_improvement | resolved
delta_summary: string | null
```

This preserves append-oriented history.

---

## 9. Learner response / tutor feedback payload

Optional lightweight fields:

```yaml
raw_answer: string | null
feedback_summary: string | null
```

Operational systems may retain raw answers where appropriate. GitHub fixtures should avoid committing private learner content unnecessarily. For long writing, prefer `artifact_ref` to copying the whole draft.

---

## 10. Evidence interpretation principles

C2 defines evidence-event semantics, not the final C1 Profile scoring formula.

1. `H5-H7 + correctness=2` can show learning/progress but not independent M2 by itself.
2. Stable quality while support drops (`H5→H3→H1/H0`) is strong automation evidence.
3. A repaired second attempt has different evidentiary meaning from an H0 first attempt.
4. M3-like transfer needs unfamiliar/diverse evidence; familiar textbook success is insufficient.
5. Near-identical variant groups do not create independent diversity merely by count.
6. Modality matters: oral thinking and written production provide different evidence.
7. Assessment confidence/provenance matters; uncertain legacy/AI labels should remain distinguishable.
8. Overall correctness may disagree with reasoning/evidence dimensions; Profile aggregation uses the relevant local evidence rather than correctness alone.

---

## 11. Low-overhead logging profile

For routine short-answer work, most context should be auto-filled from Question/session state. Minimum human/AI capture is approximately:

```yaml
question/legacy ref
attempt series + number
target nodes
response mode
hint level
a few applicable 0/1/2 diagnostics
primary error if any
one or two node-evidence observations
completed_at
assessment provenance
```

Long prose is optional. The system should not require a recap essay per question.

---

## 12. Relationship to durable Session Summary

```text
TrainingAttempt = raw structured evidence event
Session Summary / 学习记录 = promoted durable synthesis
```

Promote only meaningful breakthroughs, stable patterns, transfer results, method formation, writing revisions or topic closure.

---

## 13. Event-history integrity

Native Attempt history should be append-oriented. If a later reviewer changes the diagnostic interpretation, implementation should preserve an audit trail (e.g. assessment revision/correction record) rather than silently rewriting the learner response history.

Exact storage mechanics are implementation scope; semantic history must remain reconstructable.

---

## 14. C2 invariants

1. One submitted response = one TrainingAttempt event.
2. Successive responses to one Question share series ID and increment attempt number.
3. `null` diagnostic score means not observed/applicable, never failure.
4. Native events record H0-H7; legacy reconstruction may have unknown hint level.
5. Strong hints reduce independence evidence even when final answer is correct.
6. Attempt-level scores do not automatically apply equally to every involved node.
7. Per-node evidence is explicit and target-role mapping is internally consistent.
8. Second-attempt change is stored on the later event.
9. Material/task/familiarity/complexity/variant context is snapshotted.
10. Response modality prevents oral/artifact work from being mis-scored as written failure.
11. Assessment provenance/confidence remains distinct from learner-state confidence.
12. Long-form production can reference artifacts instead of duplicating them.
13. TrainingAttempt stays lightweight and does not replace durable recap artifacts.
14. Historical evidence and later assessment corrections remain auditable.
