# F2 semantic self-review — adaptive tutoring intervention engine

Status: **complete**

Scope reviewed:

- `InterventionDecision` schema;
- causal diagnosis policy;
- within-question state machine;
- H0-H7 escalation/fade policy;
- oral/arrow→written normalization;
- C2 linkage amendment;
- cross-domain fixtures;
- F1/C2/C3 boundary consistency.

---

## 1. Risk: turning H0-H7 into one global “amount of help” score

### Problem
The same hint can invalidate one node while leaving another fully observable. A scalar interpretation would wrongly discount all downstream evidence.

### Correction
F2 makes `node_effects` explicit. H-level remains a historical support snapshot; causal node effects determine which observations stay independent.

Example:

```text
H4 location supplied
→ location independence lost
→ reasoning may still be independently observed
```

---

## 2. Risk: knowledge gaps did not fit the original hint enum cleanly

### Problem
C2 was oriented around task/strategy/location/evidence/reasoning hints. A missing lexical/grammar fact sometimes needs a recall cue or explicit micro-teach.

### Correction
F2 separates:

- H2 knowledge-recall cue: does not supply task-specific answer;
- direct task-specific prerequisite supply: H5-like support for the supplied Knowledge node and independent retrieval is invalidated.

Pedagogical function lives in `InterventionDecision`; Attempt keeps the support snapshot.

---

## 3. Risk: “one key question” becoming three hidden questions

### Problem
An early Zhou Yafu fixture used a sequence of comparison subquestions inside one H3 prompt. That violated the project principle of advancing one layer at a time.

### Correction
Fixture now asks one missing-warrant question:

> 为什么“天子亲自劳军”这个特殊情境，会让周亚夫仍按军令办事更能说明他的品格？

If that fails, F2 escalates in a later turn rather than front-loading the whole chain.

---

## 4. Risk: premature diagnosis from absence of evidence

### Problem
A poetry answer “作者很悲伤” was initially tagged R because no evidence was given. But this could be either:

- R: cannot find evidence;
- I: has evidence but cannot explain it.

### Correction
Use `primary_causal_layer=uncertain`, store competing hypotheses and ask a cheap discriminator: “指出支持这个判断的词/意象”.

`InterventionDecision` now has `alternative_hypotheses` separate from coexisting `secondary_layers`.

---

## 5. Risk: F1 hint ceiling becoming a teaching prohibition

### Problem
A fade-scaffold move may target H1, but a real learner can unexpectedly expose a prerequisite/location block.

### Correction
F2 may exceed the F1 ceiling when needed, records override reason, and preserves honest evidence:

```text
learning continues
actual Attempt logs H4
F1 success criterion remains unmet
later probe fades support again
```

This keeps pedagogy and measurement aligned rather than sacrificing either.

---

## 6. Risk: answer/model exposure contaminating later evidence

### Problem
After H6/H7, the learner may produce the correct same-item answer. Without an explicit boundary the system could incorrectly classify that as M2 evidence.

### Correction
`answer_content_exposure` + node effects + `same_item_independence_recoverable` make the boundary explicit. New sufficiently distinct material is required to re-establish independent evidence for supplied content.

Prior independent reasoning observed **before** later standardization remains valid.

---

## 7. Risk: expression weakness being mistaken for comprehension weakness

### Problem
The learner may have correct oral/arrow reasoning but fragmented written Chinese. Generic tutoring engines often restart the reading analysis.

### Correction
F2 introduces a distinct `expression_conversion` path:

```text
correct meaning
→ explicit logical links
→ complete scoring sentence
→ accurate terminology
```

No comprehension reteach unless new evidence shows comprehension was wrong.

---

## 8. Risk: oral-first support becoming a permanent bypass

### Problem
Allowing spoken/arrow reasoning is diagnostically efficient, but the project goal includes independent written output.

### Correction
Oral/arrow mode is a temporary representation. Once thinking is correct, F2 deliberately transitions to written normalization and records whether that conversion was independent or scaffolded.

---

## 9. Risk: tutor decisions not being reconstructable from Attempt history

### Problem
C2 records the learner response and support conditions but did not have a canonical reference to the pedagogical decision that produced the support.

### Correction
F2 adds a stacked C2 amendment:

```yaml
preceding_intervention_decision_id
```

This creates the audit chain:

```text
Attempt n
→ InterventionDecision
→ Attempt n+1
```

---

## 10. Risk: “粗心” becoming a catch-all diagnosis

### Problem
Repeated errors can be mislabeled C when the real issue is knowledge, method activation or automation.

### Correction
C is permitted only with slip/self-correction evidence. Repetition across contexts triggers re-diagnosis rather than repeated “careless” feedback.

---

## 11. Risk: local prerequisite discovery accidentally replacing F1

### Problem
Within a Question, F2 may discover a prerequisite gap and switch to a micro-probe. This could grow into a second global recommendation engine.

### Correction
F2 owns only local causal control inside the current learning cycle. When new evidence materially changes what should be trained globally, close/suspend the series and let F1 recompute the next TrainingMove.

---

## 12. Risk: standardization silently improving the learner's ideas

### Problem
A polished tutor answer can introduce better evidence/interpretation than the learner actually constructed, obscuring what was learned.

### Correction
Normalization is constrained to learner-supported meaning and text-supported relations. If new reasoning/content is supplied, it is model exposure and must be recorded as such.

---

# Review conclusion

F2 is semantically ready for a stacked PR.

The resulting interaction loop is now explicit:

```text
F1 TrainingMove
↓
Question
↓
C2 Attempt 1
↓
F2 causal diagnosis
↓
minimum effective InterventionDecision
↓
C2 Attempt 2
↓
F2 fade / normalize / escalate / close
↓
C3 Profile update
↓
F1 recompute when evidence purpose changes
```

The architecture preserves the project's core tutoring rule:

> 先确认学生已经想对了什么，只修当前最关键的一层，再让学生自己完成下一版；理解正确后，再把它训练成规范书面表达。
