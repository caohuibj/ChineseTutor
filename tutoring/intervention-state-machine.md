# F2 adaptive tutoring intervention state machine

Status: **normative draft**

F2 governs the interaction **inside one Question / attempt series** after F1 has already chosen a TrainingMove and a concrete Question has been selected.

The design objective is:

> use the smallest intervention that repairs or exposes the earliest causal gap, then return cognitive work to the learner as quickly as possible.

---

## 1. State model

```text
S0 PREPARE
  ↓
S1 PRESENT / ORIENT
  ↓
S2 AWAIT_RESPONSE
  ↓
S3 ASSESS_RESPONSE
  ├─ sufficient → S7 STANDARDIZE_OR_CLOSE
  ├─ uncertain cause → S4 DIAGNOSE
  └─ clear gap → S5 INTERVENE

S4 DIAGNOSE
  ↓
S5 INTERVENE
  ↓
S6 AWAIT_RETRY
  ↓
S3 ASSESS_RESPONSE

S7 STANDARDIZE_OR_CLOSE
  ├─ expression goal remains → S8 NORMALIZE_EXPRESSION
  ├─ move requires transfer/new probe → S9 SWITCH_PROBE
  └─ done → S10 CLOSE

S8 NORMALIZE_EXPRESSION
  ↓
S6 AWAIT_RETRY or S10 CLOSE

S9 SWITCH_PROBE
  ↓
new Question / new attempt series under same or successor TrainingMove
```

No state transition directly changes Learner Profile. C2 records Attempts; C3 later projects evidence into Profile.

---

## 2. S0 PREPARE

Inputs:

- active TrainingMove;
- target LearningNodes;
- intended evidence purpose;
- Question / TaskType / complexity;
- learner Profile snapshot;
- F1 max intended hint level;
- recent related Attempts.

Before presenting the item, derive:

```yaml
expected_operation_chain: [node/dimension steps]
likely_prerequisite_risks: []
move_success_signal: string
planned_hint_ceiling: H0-H7
```

This prevents tutoring from improvising a completely different lesson after the first imperfect answer.

---

## 3. S1 PRESENT / ORIENT

Default: present the task with no extra method help beyond the F1 plan.

Do not pre-teach the solution because the learner has a weak historical Profile. The purpose may be to observe whether prior learning now transfers independently.

For `diagnose` moves, presentation should be minimal enough to preserve diagnostic value.

For `scaffolded_practice`, planned low-level support may be present from the start if F1 explicitly intended it.

---

## 4. S2 AWAIT_RESPONSE

Accept multiple response modes:

- oral / conversational language;
- short fragments;
- arrow logic;
- full written answer;
- artifact / revision.

Do not mistake informal expression for poor understanding before the intended construct is assessed.

If the learner gives only a conclusion, assess whether the task demanded the hidden reasoning or whether a follow-up is needed to observe it.

---

## 5. S3 ASSESS_RESPONSE

Assess in two layers.

### Layer A — what is already correct?

Identify:

- facts correctly located;
- relevant evidence already selected;
- valid conclusion;
- valid partial causal/structural relation;
- correct strategy trigger;
- expression that can be preserved.

### Layer B — what is the earliest actionable gap?

Use the expected operation chain + B2 prerequisites + C2 diagnostic dimensions.

Possible dispositions:

```text
A. sufficient for current move
B. correct reasoning, expression incomplete
C. one clear missing causal step
D. prerequisite uncertainty/failure
E. diagnosis uncertain
F. careless/execution slip
G. learner is blocked despite valid lower support
```

Do **not** simply choose the lowest 0/1 diagnostic score. Later failures may be downstream symptoms.

---

## 6. S4 DIAGNOSE

Use a diagnostic intervention when the causal layer is unclear.

Examples:

- final translation wrong: ask learner to explain one key word before deciding whether K or I failed;
- character conclusion unsupported: ask which sentence/behavior led to the judgment before deciding R vs I;
- polished but irrelevant answer: re-check question demand before teaching content;
- writing paragraph weak: ask for intended central point before deciding structure vs language.

Diagnostic prompts should be **cheaper than full reteaching**.

A diagnosis turn may itself generate a valid new Attempt if the learner submits a meaningful response.

---

## 7. S5 INTERVENE — minimal effective intervention

Default shape of one tutor turn:

```text
1. preserve one correct element
2. name or isolate one missing link
3. ask one question / one operation
4. return response responsibility to learner
```

Example structure:

> 你已经抓到“周亚夫拒绝天子按军规办事”这个证据。现在只差一层：为什么“天子亲自劳军”让这个行为特别能说明人物品质？先只回答这一点。

The tutor should not simultaneously add:

- the missing evidence;
- the full reasoning;
- the terminology;
- the final standardized sentence;

unless support escalation actually reaches those levels.

---

## 8. S6 AWAIT_RETRY — second attempt is default

After a repair intervention, the learner normally responds again.

Why:

- tutoring explanation alone is not evidence of learning;
- the second response shows whether the missing link was actually constructed;
- C2 can capture change from previous Attempt;
- support can then fade on the same or next item.

Exceptions where immediate second attempt may be skipped:

- H7/complete model answer exposure makes same-item independent reconstruction meaningless;
- task is a very long artifact and a targeted local revision is more appropriate;
- learner cannot continue and the pedagogical goal is explicit teaching/closure;
- accessibility/fatigue constraints require stopping.

Even then, schedule a later independent/new-item probe if mastery evidence is still needed.

---

## 9. Escalation policy inside a series

Do not mechanically run H1→H2→H3→H4→H5→H6→H7.

Escalation is **causal**.

If an H3 reasoning question fails because learner cannot find the passage, switch diagnosis to R and use location narrowing rather than giving a stronger reasoning hint.

If H4 location narrowing succeeds and learner then reasons independently, preserve that downstream reasoning evidence.

Default principles:

1. prefer the lowest support that directly addresses the diagnosed blocker;
2. if it fails, re-check diagnosis before escalating;
3. escalate only the component still blocked;
4. after repair, fade on the next opportunity;
5. H6/H7 are teaching/closure modes, not routine scaffolds.

---

## 10. Fade policy

After a successful scaffolded retry:

```text
same semantic operation
+ comparable complexity
+ less support
```

is usually the highest-value next evidence.

Examples:

- H3 success → new similar task at H1/H0;
- H4 location support + independent reasoning → next item keeps reasoning demand but removes location cue;
- H2 strategy reminder → next task tests spontaneous strategy invocation.

Do not simultaneously remove support and sharply raise several D2 dimensions unless F1 explicitly selected complexity extension.

---

## 11. S7 STANDARDIZE_OR_CLOSE

Once the reasoning/target construct is sufficiently established for this item, decide whether standardization is needed.

Possible outputs:

- concise confirmation and close;
- ask learner to produce final exam-style answer;
- tutor shows a normalized version **after** learner logic is complete;
- identify which logical layer the learner added on the retry;
- switch to a new probe for transfer/automation.

Standardization is not an excuse to replace the learner's reasoning with a richer interpretation unsupported by their response/material.

---

## 12. S8 NORMALIZE_EXPRESSION

Use when conceptual reasoning is sound but output is incomplete, fragmented or non-standard.

Sequence:

```text
meaning already correct
↓
make logical links explicit
↓
convert oral/arrow form into complete sentence(s)
↓
add necessary terminology only if it accurately names the relation
↓
remove redundancy
```

Do not reopen text comprehension unless new evidence shows comprehension was actually wrong.

---

## 13. S9 SWITCH_PROBE

Switch Question rather than continuing the same item when:

- answer/model exposure makes same-item independence unrecoverable;
- the current item has saturated its evidence purpose;
- transfer requires material novelty;
- a prerequisite needs a cleaner diagnostic item;
- the task cost is too high for the narrow evidence needed.

The next Question can remain under the same TrainingMove if the move's evidence purpose is not yet satisfied.

---

## 14. Stop rules

Close the attempt series when any is true:

- current item has met the local success criterion;
- further same-item work has low information value;
- answer exposure has saturated independence;
- a different prerequisite probe is required;
- F1 TrainingMove should be recomputed after meaningful new evidence;
- fatigue/time cost exceeds expected learning value.

Avoid “再做一道” as a default when the evidence purpose is already satisfied.

---

## 15. State-machine invariants

1. F1 chooses **what to train**; F2 chooses **how to intervene now**.
2. C2 records learner responses; F2 decisions occur between those events.
3. C3 updates Profile only from evidence, never from tutor intention.
4. A wrong final answer is not itself a diagnosis.
5. Correct partial reasoning is preserved and reused.
6. One intervention normally targets one causal layer.
7. Second attempt is the normal learning checkpoint.
8. Support escalation is causal, not ordinal ritual.
9. Support fades after successful repair when the move needs independence/automation.
10. Expression normalization follows correct reasoning rather than replacing it.