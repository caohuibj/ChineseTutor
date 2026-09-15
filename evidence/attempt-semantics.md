# TrainingAttempt evidence semantics

Status: **C2 design draft**

C2 turns the tutoring conversation into lightweight evidence that can update the C1 Learner Profile without converting every question into a long note.

The central unit is not “a finished problem”. It is **one learner response under known support conditions**.

---

## 1. Why first and second answers are separate events

ChineseTutor's teaching loop is:

```text
first answer
-> diagnose earliest failing layer
-> minimal intervention
-> second answer
```

If only the final answer is stored, the system loses the learning signal.

Example:

```text
Attempt 1
reasoning=1
expression=0
hint=H0

Intervention
one guiding question (H3)

Attempt 2
reasoning=2
expression=2
hint=H3
delta=resolved
```

The final result alone would say “correct”. The event pair says much more:

- initial understanding was partly present;
- one inferential bridge was missing;
- a small prompt repaired it;
- independent performance is not yet verified;
- future comparable tasks should reduce scaffold.

That is exactly the evidence ChineseTutor needs.

---

## 2. Earliest causal failure principle

Attempt scoring should follow the tutoring diagnostic order:

```text
Q question understood?
R relevant text/material located?
K required knowledge available?
I reasoning relation valid?
M suitable strategy triggered/executed?
E reasoning converted into written output?
C execution/carelessness?
```

The event can contain several error codes, but `primary_error_code` should capture the earliest/most causal failure where possible.

Example:

```text
question_understanding=2
text_location=2
evidence_selection=2
reasoning=2
written_expression=0
primary_error=E
```

This must not update reading comprehension downward. It is an expression bottleneck.

---

## 3. Diagnostic score meaning is local to the attempt

`0/1/2` does not mean global learner mastery.

It means only:

```text
0 absent/failed in this response condition
1 partial/unstable
2 successful in this response condition
```

Profile updates later aggregate across attempts, task diversity, complexity and hints.

A C2 event should therefore stay factual and local rather than trying to decide M0-M3 itself.

---

## 4. Per-node evidence prevents false attribution

A real question can exercise many things at once.

Example: a classical character question may involve:

```text
classical lexical decoding
sentence comprehension
evidence selection
character inference
written expression
strategy invocation
```

If the final answer is wrong because one key word was mistranslated, it would be false to mark every involved node as failed.

C2 therefore records:

```text
target_node_ids
+
node_evidence[]
```

Example:

```yaml
- node_id: CN-A-infer-classical-contextual-word-sense
  role: prerequisite
  observation: negative
  independence: independent

- node_id: CN-A-evidence-to-character-judgment
  role: primary_target
  observation: not_observed
  independence: unknown
```

Interpretation:

> The attempt failed before character reasoning could be validly assessed.

This is essential for causal diagnosis.

---

## 5. Hint level is evidence context, not a penalty

A hint is not a negative event. It changes what the response can prove.

Example:

```text
H0 correct
```

is strong evidence of independent access.

```text
H3 correct after one key question
```

is useful evidence that a small scaffold unlocks the missing layer.

```text
H6 correct
```

shows the learner can follow supplied reasoning but does not verify independent reasoning.

The tutoring goal is not “never give hints”. It is:

> give the smallest hint that produces the next correct cognitive step, then watch required hint level fall over time.

---

## 6. Hint reduction is a first-class learning signal

On comparable tasks:

```text
H5 -> H3 -> H2 -> H0
```

with stable output quality is often stronger evidence of strategy/automation growth than raw accuracy alone.

C2 therefore makes `max_hint_level_before_response` mandatory for native events.

Later Profile logic can distinguish:

```text
correct because tutor supplied evidence
```

from:

```text
correct because learner independently located and reasoned
```

---

## 7. Second-attempt delta belongs to the later event

The later event knows what changed.

Correct model:

```text
ATT-002.previous_attempt_id = ATT-001
ATT-002.delta_from_previous = major_improvement
```

Do not store `next_attempt_delta` on ATT-001, because an event should not contain future information.

This makes event streams append-friendly and auditable.

---

## 8. `null` is different from 0

This rule is critical across C1 and C2.

Example: writing prompt.

```text
text_location = null
```

means the dimension is not applicable.

```text
text_location = 0
```

means locating relevant source material was required and failed.

The same applies to correctness on open writing tasks.

---

## 9. Evidence for Mastery, Automation and Complexity differs

One event can contribute differently to each C1 axis.

### Mastery evidence
Primarily influenced by:

- relevant node outcome;
- reasoning/knowledge/production success;
- independence;
- diversity across attempts;
- unfamiliarity/transfer where required.

### Automation evidence
Primarily influenced by:

- hint level;
- task-type/strategy self-recognition;
- elapsed/time-pressure condition;
- stable quality while support is withdrawn.

### Complexity evidence
Primarily influenced by:

- question complexity snapshot;
- successful node observation;
- material familiarity;
- degree of support.

C2 records facts needed for later aggregation but does not hard-code the final M/A/C formula yet.

---

## 10. Transfer evidence is stricter than ordinary success

A transfer attempt should normally have:

```text
material_familiarity = unfamiliar
transfer_probe = true
```

and meaningful target-node positive evidence.

If the material is only a renamed near-copy, `question_variant_group_id` should reveal that similarity and prevent inflated evidence diversity.

Transfer is not declared simply because the source came from a higher grade or different exam.

---

## 11. Legacy reconstructed evidence

Existing project learning records predate C2. They can still be represented, but uncertainty must be explicit.

Legacy reconstruction rules:

- `legacy_reconstructed=true`;
- unknown historical hint level may be null;
- do not invent elapsed time or diagnostic scores not supported by the record;
- preserve only evidence actually recoverable from the source;
- use conservative node observations;
- C1 evidence confidence should normally remain weak/moderate until native attempts accumulate.

This allows current history to seed the new system without falsifying precision.

---

## 12. What belongs in a Session Summary instead

Attempt event fields should stay terse.

Use durable summary for:

- “what new method was formed?”
- “what stable error pattern emerged?”
- “what changed over several attempts?”
- “what should be transferred next?”

Use TrainingAttempt for:

- response condition;
- scores;
- errors;
- node evidence;
- hint/intervention;
- attempt delta.

This separation is the main scalability improvement over v1 `学习记录`.

---

## 13. Example diagnostic interpretation

### Case A: understanding good, expression weak

```yaml
question_understanding: 2
text_location: 2
evidence_selection: 2
reasoning: 2
written_expression: 0
answer_correctness: 1
error_codes: [E]
```

Tutor response:

> Do not reteach the reading. Ask the learner to make the missing inferential sentence explicit.

### Case B: strategy reminder fixes task

Attempt 1:

```text
hint H0
task_type_recognition 0
reasoning 1
```

Attempt 2:

```text
hint H2
strategy reminder
reasoning 2
expression 2
```

Profile signal:

> Mastery may be developing; Automation/strategy self-trigger remains weak.

### Case C: downstream target not observed because prerequisite failed

```text
classical word sense: negative
sentence comprehension: negative
character reasoning: not_observed
```

Tutor response:

> Repair lexical decoding, not character-analysis method.

---

## 14. Event immutability

Native TrainingAttempt events should be append-oriented and treated as historical evidence.

If a scoring/tagging error is discovered later, prefer:

- corrected event version / audit note; or
- explicit correction metadata in implementation;

rather than silently rewriting history without trace.

Exact event-versioning mechanics are implementation scope, but semantic history must remain auditable.
