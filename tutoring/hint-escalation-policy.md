# F2 hint escalation and fade policy

Status: **normative draft — semantic self-review corrections applied**

F2 uses C2's H0-H7 support scale but interprets it **causally per node**. The scale records the amount/form of task-specific help available before the next response; it is not a universal score deduction and it does not replace explicit `node_effects`.

---

## 1. Canonical levels

```text
H0  no hint
H1  task-demand / task-type orientation
H2  strategy, rule-family or knowledge-recall cue that does not supply the task-specific answer
H3  one key guiding question exposing one missing relation
H4  relevant text range / source location supplied
H5  critical evidence or task-specific prerequisite information supplied
H6  partial reasoning / missing logical bridge supplied
H7  near-answer / model-answer exposure
```

F2 does not treat these labels as a perfect universal ordering across every node. Two interventions at the same H level can affect different constructs very differently. `InterventionDecision.node_effects` is authoritative for what remains independently observable.

Examples:

- “这类翻译先圈哪些关键词？” → H2 strategy cue.
- “这个词你学过哪些常见义项？” → H2 knowledge-recall cue.
- “这里‘虽’就是‘即使’。” → H5-like direct task-specific prerequisite supply for the word-meaning node; independent retrieval of that Knowledge node is invalidated, while downstream sentence reasoning may still remain observable.

Therefore **node effects outrank the ordinal label** when interpreting evidence.

---

## 2. Selection rule: lowest direct support, not lowest number

Choose the least revealing intervention that directly targets the diagnosed blocker.

Do not start at H1 if H1 does not address the actual problem.

Examples:

- clear I gap with correct evidence → H3 key question may be the first appropriate intervention;
- clear R gap → H3 “where would you look?” or H4 location narrowing depending on how blocked the learner is;
- M gap → H2 strategy activation;
- E gap with correct reasoning → expression conversion can be H0-H2 relative to comprehension because no new content hint is needed.

The policy is not a ritual ladder.

---

## 3. First-intervention heuristics

### Q
Prefer H1 reorientation/reframe.

### K
Prefer H2 recall cue or a minimal contrast. If knowledge is genuinely absent, provide a narrow micro-explanation/direct prerequisite fact and treat the supplied node as guided/not independently observed (often H5-like support for that node).

### R
Prefer H3 search question; use H4 when navigation itself is blocking progress; H5 only when critical evidence must be supplied.

### I
Prefer H3 one-key-question. Use H6 only after the learner has tried and the missing relation remains blocked.

### M
Prefer H2 strategy trigger/name/first step.

### E
No new comprehension hint by default. Ask for oral/arrow reformulation or one complete sentence; provide a sentence skeleton only if needed.

### C
Prefer H0 targeted self-check.

---

## 4. Escalation conditions

Escalate support only when one of these is true:

1. learner meaningfully attempted the requested operation and remains blocked;
2. diagnosis is high-confidence and more explicit support is needed;
3. learner cannot access an upstream prerequisite needed to continue;
4. task must be closed efficiently after sufficient productive struggle;
5. accessibility / fatigue requires reducing unnecessary struggle.

Before escalation, ask:

> Did the previous intervention fail because it was too weak, or because the diagnosis was wrong?

If diagnosis may be wrong, change diagnosis rather than simply increasing H.

---

## 5. Escalation examples

### Reasoning gap

```text
H0 response: evidence + conclusion, no warrant
H3: “为什么这个行为在这里特别能说明刚正不阿？”
```

If learner still cannot answer but evidence/context is understood:

```text
H6: supply part of bridge, e.g. “关键在于普通将领面对天子可能会放松军令……”
```

Then ask learner to finish the consequence/quality link.

Do not jump to H7 unless closure/teaching requires it.

### Location gap

```text
H3: “这题的依据更可能在劳军前、入营过程还是文帝评价里？”
H4: “先看天子到细柳营门后的这一段。”
H5: point out the exact sentence if still blocked.
```

### Knowledge gap

```text
H2: ask for known meanings / relevant rule family
H5-like direct supply: teach the exact missing task-specific prerequisite only if genuinely absent
```

Then return immediately to the original operation. Do not expand into a broad lecture.

---

## 6. F1 hint ceiling

F1 `max_intended_hint_level` defines the desired evidence condition for the TrainingMove.

F2 rules:

- try to remain within the ceiling when the learner can progress;
- never withhold necessary help solely to protect the move;
- exceeding the ceiling marks the move's intended evidence criterion as unmet for that attempt;
- record `ceiling_exceeded` and actual support;
- after repair, schedule a lower-support re-probe rather than pretending the ceiling was met.

Example:

```text
TrainingMove: fade scaffold, max H1
learner unexpectedly cannot locate evidence
F2 uses H4
```

Correct consequence:

```text
learning continues
Attempt logs H4
move success not yet met
next comparable probe should remove location help
```

---

## 7. Fade rules

After a successful supported response, choose the next evidence condition based on what support compromised.

### H2 strategy/recall cue success
Next task: same operation, H0/H1 to test self-trigger/retrieval.

### H3 guiding question success
Next task: remove the key question; preserve comparable reasoning demand.

### H4 location success + downstream independent reasoning
Next task: remove location cue while keeping reasoning complexity stable.

### H5 evidence/prerequisite supply
Next task: learner must independently retrieve/select the supplied element before the downstream operation is fully re-verified.

### H6 partial reasoning
Next task: new/variant item must require the learner to construct the missing bridge independently.

### H7 model exposure
Do not claim same-item independence later. Use a new sufficiently distinct probe after teaching.

---

## 8. Productive struggle limit

Minimal intervention does **not** mean withholding help indefinitely.

Stop repeating low-level prompts when:

- learner has made two genuine attempts without progress;
- repeated prompting is producing guessing rather than reasoning;
- the prerequisite is clearly absent;
- frustration/fatigue cost is rising faster than information value;
- the tutor already has enough evidence to know what must be taught.

At that point, teach the missing piece explicitly, mark evidence appropriately, and plan a later independent probe.

---

## 9. Answer exposure boundaries

### Structure-only support
May preserve content independence.

Example:

> “按‘结论—证据—解释’三部分整理。”

### Partial content support
Compromises the supplied node/step, but may preserve other operations.

### Near-complete/complete answer exposure
Same-item independent mastery evidence for the supplied solution is no longer recoverable.

The learner may still:

- explain why the model works;
- compare it with their attempt;
- repair one local phrase;
- practice reproduction.

But independent verification requires a new item.

---

## 10. Hint-policy invariants

1. H-level is support context, not a global learner score penalty.
2. Node-specific effect determines what independence remains observable.
3. Escalate only the blocked component.
4. Re-check diagnosis before brute-force escalation.
5. F1 ceiling is planned evidence condition, not a help prohibition.
6. Successful support creates a fade obligation when independence/automation is the goal.
7. H6/H7 are valid teaching tools but weak/invalid independent evidence for supplied reasoning.
8. Directly supplying task-specific prerequisite knowledge invalidates independent retrieval for that Knowledge node while potentially preserving downstream observation.
9. Productive struggle has an efficiency boundary.