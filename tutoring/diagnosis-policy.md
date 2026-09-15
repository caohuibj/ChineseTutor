# F2 causal diagnosis policy

Status: **normative draft**

The tutor must diagnose the **earliest actionable cause** of an incomplete/incorrect response before choosing an intervention.

The existing codes remain:

```text
Q  question understanding / task demand
K  knowledge prerequisite
R  reading / location / evidence access
I  inference / reasoning
M  method / strategy invocation
E  expression / organization / terminology
C  carelessness / execution
```

These are diagnostic labels, not mutually exclusive traits.

---

## 1. Earliest-cause rule

A visible downstream error should not be treated as causal when an upstream gap already explains it.

Examples:

```text
wrong translation
because key word meaning is wrong
=> primary K, not I/E
```

```text
character judgment is plausible
but no evidence chosen
=> inspect R before teaching I
```

```text
reasoning is complete orally
written answer omits the warrant
=> primary E for current response, not I
```

```text
learner knows strategy when reminded
but never triggers it independently
=> primary M/AUTOMATION issue, not necessarily mastery I
```

---

## 2. Use an expected operation chain, not one universal fixed order

Different TaskTypes have different causal chains.

Example character analysis:

```text
understand question
→ locate event/detail
→ select relevant evidence
→ identify special context/choice
→ explain significance
→ infer stable character quality
→ write complete answer
```

Classical translation:

```text
identify sentence boundaries/keywords
→ retrieve/infer word senses
→ parse syntax/special construction
→ reconstruct proposition
→ adjust modern word order
→ write fluent translation
```

Poetry emotion:

```text
identify scene/event/person
→ interpret keywords/images
→ infer emotional direction/object
→ integrate local evidence
→ express bounded emotional judgment
```

Writing:

```text
understand prompt constraints
→ choose controlling idea
→ select material/evidence
→ organize structure
→ produce scene/argument
→ revise expression
```

Diagnose against the relevant chain and B2 prerequisites.

---

## 3. Q — question/task understanding

Signals:

- answer addresses a neighboring but different demand;
- student explains content when asked for function/effect;
- comparison answer describes A only;
- writing ignores explicit audience/genre/task constraint.

First response:

- ask the learner to restate “这题到底要我回答什么”;
- isolate command word / comparison object / required output;
- do not teach content until the task demand is clear.

Do not label Q merely because the answer is wrong.

---

## 4. K — knowledge prerequisite

Signals:

- missing/incorrect word meaning, grammar rule, literary concept, cultural fact;
- learner's reasoning would be valid if the prerequisite fact/concept were correct;
- repeated inability to distinguish a canonical concept.

Intervention order:

1. check whether knowledge is actually absent vs retrieval temporarily blocked;
2. use a recall cue / contrast / micro-explanation;
3. return immediately to the original operation if feasible;
4. log that independent evidence for the supplied Knowledge node is compromised.

Do not turn every task into a lecture on all potentially relevant knowledge.

---

## 5. R — reading/location/evidence access

Signals:

- learner cannot find relevant passage/detail;
- evidence chosen is unrelated though task is understood;
- conclusion may be plausible but unsupported by text;
- multi-text answer misses the source containing the required condition.

Intervention ladder:

```text
ask where in text to look
→ narrow section
→ identify evidence candidates
→ supply critical evidence only if still blocked
```

Once location/evidence is supplied, downstream reasoning may still be validly observable if not supplied too.

---

## 6. I — inference/reasoning

Signals:

- evidence and conclusion are both present but relation is unexplained/invalid;
- causal link, comparison significance, structural effect or warrant is missing;
- learner jumps from fact to label;
- evidence does not logically support conclusion despite correct location.

Preferred first intervention:

> one key question that asks for the missing relation.

Examples:

- “为什么在这个特殊情境下，这个行为更能说明这个品质？”
- “这段放在这里，和前后文分别有什么关系？”
- “这条证据为什么足以支持你的判断？”

Avoid immediately supplying the missing warrant.

---

## 7. M — method/strategy invocation

Signals:

- learner can execute the process after being reminded but does not self-trigger it;
- response is disorganized because no strategy was selected;
- repeated forgetting to compare on a common dimension / run translation keyword check / use evidence-explanation chain.

Intervention:

- remind only the strategy name or first trigger when possible;
- avoid replaying every step if learner can continue independently;
- after success, next task should reduce/removes the reminder.

M often affects Automation more than Mastery.

---

## 8. E — expression

Signals:

- oral/arrow reasoning is sound but written answer is fragmented;
- evidence and reasoning are correct but pronouns/connectives/subject-object relations obscure meaning;
- necessary answer components exist but are not integrated into a complete scoring sentence;
- terminology is inaccurate even though conceptual relation is understood.

Intervention:

```text
preserve meaning
→ ask learner to state one complete relation
→ provide minimal sentence skeleton if needed
→ normalize terminology
```

Do not re-teach comprehension merely because the prose is weak.

---

## 9. C — carelessness/execution

Use cautiously.

Signals:

- learner immediately self-corrects when asked to check;
- known rule/word is omitted or copied incorrectly despite stable prior evidence;
- answer fails a simple self-check while reasoning path is otherwise sound.

Intervention:

- targeted self-check, not conceptual reteaching;
- if repeated across contexts, reconsider whether the issue is actually K/M/A rather than “粗心”.

`C` must never become a convenient label for unexplained failure.

---

## 10. Multiple errors

A response may contain several codes. Record them, but choose one `primary_causal_layer` for the next intervention when possible.

Example:

```text
Q correct
R correct
I incomplete
E fragmented
```

Treat I first because expression normalization before the reasoning is complete would only polish an incomplete thought.

Conversely:

```text
I complete orally
E fragmented in writing
```

Treat E directly.

---

## 11. Diagnostic uncertainty

If confidence is low, do not overcommit to a diagnosis.

Use a cheap discriminator:

- “你先指出原文依据。” separates R from I;
- “这个词在这里你怎么理解？” separates K from sentence-level I;
- “先口头说因果链，不管措辞。” separates I from E;
- “你准备调用哪套方法？” separates M from execution failure.

A diagnostic probe is often more efficient than a stronger hint based on a wrong diagnosis.

---

## 12. Diagnosis invariants

1. Wrong answer != known diagnosis.
2. Use task-specific operation chain + canonical prerequisites.
3. Upstream causal failure outranks downstream symptom.
4. Correct reasoning can coexist with poor expression.
5. Strategy invocation and reasoning quality are separable.
6. `C` requires evidence of a slip, not ignorance of the cause.
7. Diagnostic uncertainty should trigger a cheap discriminator before heavy teaching.
8. One intervention normally addresses the current primary causal layer only.