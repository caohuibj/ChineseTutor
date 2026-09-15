# R1.8 — 20–40 native Attempt pilot

Status: **waiting for first real learner response**

Issue: #51

## 1. Purpose

R1.8 is an integration pilot, not a synthetic benchmark and not a mastery-certification exam.

It must show that real tutoring turns can continuously exercise:

```text
TrainingMove
→ Question presentation
→ native TrainingAttempt
→ per-node evidence
→ diagnosis
→ InterventionDecision
→ retry
→ C3 Profile projection
→ recommendation recompute
```

with enough diversity to expose release blockers before v2 MVP is declared operational.

## 2. Hard evidence floor

The pilot is not complete before all of the following are true:

```yaml
native_training_attempts: 20..40
observed_task_or_domain_families: ">=4"
genuine_intervention_retry_sequences: ">=5"
```

Only actual learner submissions count.

Legacy reconstructed Attempts from R1.5 do not count. Curated Questions do not count. Tutor-written mock answers do not count.

## 3. Starting series

```yaml
session_id: SESSION-r18-pilot-01
attempt_series_id: SERIES-r18-01-zhouyafu-46
next_attempt_id: ATT-r18-01-zhouyafu-46-01
training_move: TM-r16-01-comparison-significance-h0-baseline
question: Q-zhouyafu-46-comparison-function
prompt: 霸上、棘门军与细柳营的对比有何作用？
support_before_response: H0
hint_count_before_response: 0
material_familiarity: familiar
transfer_probe: false
```

The first learner response is a baseline for independence/automation under a familiar surface. It cannot establish transfer.

## 4. Adaptive coverage plan

The order below is a coverage envelope, not a fixed worksheet.

### Family A — comparison / character evidence

Primary goals:
- resolve the known comparison-significance gap;
- verify comparison-dimension self-trigger;
- replace legacy uncertainty for character judgment;
- obtain a changed-surface probe.

Available Questions include:
- Zhou Yafu 4.6 comparison baseline;
- memoir-pair comparison/significance variant;
- Han Wendi character;
- Tengye / Huiyi Luxun character tasks;
- biography quotation→character task.

Expected contribution: roughly 5–10 Attempts including legitimate retries.

### Family B — detail / technique / structure / language effect

Primary goals:
- obtain first native evidence for detail-effect Ability;
- distinguish concrete effect reasoning from empty terminology;
- exercise structural-function and revision-effect paths.

Available Questions include:
- Huiyi Luxun 2.2;
- Tengye 1.2;
- Tengye 1.3;
- Tengye 1.4(2);
- biography 3.3.

Expected contribution: roughly 4–8 Attempts.

### Family C — classical lexical prerequisite

Primary goals:
- determine whether lexical Knowledge or contextual inference blocks classical work;
- verify that upstream failure produces downstream `not_observed`, not fake negative evidence.

Available Question:
- Zhou Yafu 4.2 selected contextual-word diagnostic.

Follow-up classical tasks should be chosen only if diagnostic evidence warrants them.

Expected contribution: roughly 2–5 Attempts.

### Family D — genre / title / bounded evaluation

Primary goals:
- separate genre Knowledge from evidence-based genre judgment;
- test title meaning on a more unfamiliar surface;
- exercise evidence sufficiency / bounded evaluation only after lower-cost diagnostics.

Available Questions include:
- Zhang Zhongxing genre judgment;
- biography title comparison;
- Lin Zexu title paradox;
- four-text 3.5 evaluation (late pilot only).

Expected contribution: roughly 4–8 Attempts.

### Family E — reading → writing transfer

Primary goal:
- test whether “special context / key choice / typical event” can guide authentic material selection rather than remain a reading-only strategy.

Available Question:
- adapted “让我难忘的他/她” material-selection diagnostic.

Use after sufficient reading evidence; do not jump to a full 800-word essay solely to raise Attempt count.

Expected contribution: 1–4 Attempts.

## 5. Intervention coverage rule

The pilot needs at least five genuine repair sequences because F2/F3 cannot be release-tested through all-correct H0 responses alone.

But runtime must **not** manufacture intervention opportunities.

If the learner's first response is sufficient:
- log the independent success;
- close or switch probe as appropriate;
- do not give a needless hint just to create a retry.

If fewer than five genuine repair sequences have occurred at Attempt 20:
- continue with appropriate diagnostic/complexity tasks up to Attempt 40;
- choose tasks because they expose uncertain/high-value constructs, not because they are expected to cause failure.

If five sequences still cannot be exercised honestly by Attempt 40, record a pilot-coverage blocker for R1.9 rather than fabricating tutoring data.

## 6. Per-Attempt runtime checklist

For every learner submission:

1. ensure one stable Attempt ID;
2. snapshot Question / TaskType / Material / complexity / familiarity / transfer status;
3. record actual support before the response;
4. store raw answer in private production evidence when appropriate;
5. assess only applicable 0/1/2 dimensions;
6. create node-specific observations;
7. assign independence per node;
8. identify earliest causal error if one is supportable;
9. if repair is needed, create InterventionDecision before giving the next prompt;
10. if retry arrives, create a new Attempt in the same series;
11. run C3 projection only after underlying evidence is durable;
12. recompute/transition TrainingMove when its evidence purpose is satisfied.

## 7. Profile-update sampling requirements

The pilot should exercise all of these outcomes if real evidence permits:

- `hold + confidence increase`;
- guided repair supporting M1 but not A2;
- independent H0/H1 evidence supporting M2/A2 eligibility;
- complexity verification without mastery promotion;
- at least one `not_observed` caused by prerequisite blocking if such a case genuinely appears;
- at least one contradiction/verification decision if real data produces it;
- transfer evidence only on genuinely unfamiliar/diverse contexts.

Do not force a semantic outcome that the learner evidence does not produce.

## 8. Stop conditions inside a series

Stop same-item work when:
- local success criterion is met;
- answer exposure saturates independence;
- the evidence purpose is exhausted;
- a prerequisite needs a cleaner probe;
- continued same-item work has lower value than switching variant.

Do not turn every Question into a long tutoring dialogue.

## 9. Session recap / dual-run

At meaningful session boundaries:
- preserve raw evidence in v2;
- create/update durable human-readable `学习记录` only for real breakthroughs/patterns;
- do not mirror every Attempt into v1 ability rows;
- keep v1 rollback-safe through R1.8.

## 10. Pilot tracker semantics

GitHub Issue #51 is the release-level tracker.

Notion TrainingAttempts are the authoritative native Attempt count.

Issue counters are summaries, not the evidence ledger. Update the issue after coherent batches/sessions rather than every low-level write if that reduces operational noise.

## 11. Human boundary

R1.8 cannot advance from zero native Attempts until the real learner submits the first answer.

The runtime is already ready. The correct next action is to present:

> 霸上、棘门军与细柳营的对比有何作用？

and wait without giving a method hint.
