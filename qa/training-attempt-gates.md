# QA gates — TrainingAttempt evidence events (C2)

These gates are normative acceptance checks for Issue #4.

## Gate 1 — One submitted response equals one event

First and second answers to the same Question must be separate `TrainingAttempt` records.

Fail if only the final corrected answer survives.

---

## Gate 2 — Attempts to the same Question are grouped

Successive responses share:

```text
attempt_series_id
```

and increment `attempt_number`.

A different concrete Question starts a new series even in the same tutoring session.

---

## Gate 3 — Question reference is traceable

At least one must exist:

```text
question_id
legacy_question_ref
```

Native events should use canonical `question_id` once that entity exists. Legacy reconstruction may use the legacy reference.

---

## Gate 4 — Hint level describes support before this response

Native events must record H0-H7.

Fail if hint level means “maximum help anywhere in the session” rather than support available before the current response.

Legacy reconstructed events may use null only when history does not support a precise value.

---

## Gate 5 — H5-H7 correctness is not independent mastery

A correct response after critical evidence, partial reasoning, or near-answer support can be positive learning evidence but cannot alone establish M2 independent performance.

---

## Gate 6 — Hint reduction is measurable

Comparable event sequences must preserve whether support drops, e.g.:

```text
H5 -> H3 -> H1 -> H0
```

Fail if final correctness overwrites prior hint-dependence history.

---

## Gate 7 — Null diagnostic score is not zero

Every 0/1/2 diagnostic dimension allows null for not-assessed/not-applicable.

Fail if an open writing task receives `text_location=0` merely because the dimension is irrelevant.

---

## Gate 8 — Question-understanding failure is distinguishable

C2 must support Q-layer failure separately from reading/reasoning/expression.

Fail if a misunderstood prompt can only be recorded as reasoning=0.

---

## Gate 9 — Understanding and expression can diverge

Pass:

```text
reasoning=2
written_expression=0
primary_error=E
```

Fail if low expression automatically downgrades reasoning in the event record.

---

## Gate 10 — Attempt correctness does not replace diagnostics

A single `answer_correctness` field must not be used as the only evidence.

Pass contradictory but meaningful observations such as:

```text
correctness=2
reasoning=0
```

when the answer is correct without visible required reasoning.

---

## Gate 11 — Multi-node questions require node-specific evidence

Do not apply one attempt-level result uniformly to every target/prerequisite/strategy node.

Pass:

```text
classical word sense = negative
character reasoning = not_observed
```

when decoding failure prevents valid observation of downstream reasoning.

---

## Gate 12 — `not_observed` is not negative

If evidence cannot validly reach a node because an upstream failure occurred, use:

```text
observation: not_observed
```

not `negative`.

---

## Gate 13 — Per-node evidence has a role

Allowed roles:

```text
primary_target
secondary_target
prerequisite
strategy
incidental
```

This lets later Profile updates distinguish intended training targets from incidental observations.

---

## Gate 14 — First/second delta is stored on the later event

Correct:

```text
ATT-002.previous_attempt_id = ATT-001
ATT-002.delta_from_previous = resolved
```

Reject future-looking `next_attempt_delta` stored on ATT-001.

---

## Gate 15 — Material familiarity is explicit for transfer claims

M3/transfer evidence cannot ignore whether material was familiar.

Transfer-probe events should normally include:

```text
material_familiarity=unfamiliar
transfer_probe=true
```

or a documented reason otherwise.

---

## Gate 16 — Similar variants do not masquerade as diversity

If multiple questions are near-identical variants, `question_variant_group_id` should allow later aggregation to discount false diversity.

Fail if five superficial rewrites are automatically counted as five independent transfer demonstrations.

---

## Gate 17 — Complexity is snapshotted

Attempt evidence records the relevant complexity band/vector snapshot.

This prevents later Question retagging from silently changing the historical evidence context.

---

## Gate 18 — Timing evidence is optional but explicit

Elapsed/time-limit fields may be null. When A3/time-pressure claims are made later, the relevant Attempt evidence must actually include timed conditions.

---

## Gate 19 — Error code can be multi-label but primary cause is available

Example:

```text
error_codes=[I,E]
primary_error=I
```

supports causal tutoring while preserving downstream symptoms.

One event error does not automatically become a C1 stable error pattern.

---

## Gate 20 — Legacy reconstruction does not fabricate telemetry

For pre-C2 records:

- set `legacy_reconstructed=true`;
- leave unknown hint/time/scores null if unsupported;
- do not invent H0 independence;
- use conservative node evidence.

---

## Gate 21 — TrainingAttempt remains lightweight

A routine attempt must be loggable through structured selections plus at most a short diagnostic note.

Fail if every event requires a long recap essay.

---

## Gate 22 — Durable summary remains separate

Do not force every Attempt to create/update a long `学习记录`.

Attempt = raw evidence event.

Session Summary = promoted durable synthesis for meaningful breakthroughs/patterns/closure.

---

## Gate 23 — Existing real session can be represented without losing key process

The reconstructed 《周亚夫军细柳》 fixture must preserve at least:

- special-context/choice reasoning in 4.5;
- comparison/foil recognition in 4.6;
- expression/reasoning gap distinction;
- uncertainty about historical hint level rather than inventing it.

---

# C2 definition of done

- [x] event identity/grouping defined;
- [x] Question/legacy reference defined;
- [x] target nodes and per-node evidence defined;
- [x] H0-H7 semantics defined per response;
- [x] 0/1/2 diagnostic fields support null N/A;
- [x] Q/R/I/E and other errors can be separated;
- [x] second-attempt delta is append-friendly;
- [x] complexity/familiarity/transfer context snapshotted;
- [x] near-duplicate variant grouping supported;
- [x] event-vs-summary boundary defined;
- [x] current real learning session represented conservatively;
- [ ] semantic self-review complete;
- [ ] stacked PR opened.
