# F3 review and spaced-verification policy

Status: **normative draft**

F3 turns temporal uncertainty into explicit review work without pretending that time itself is learner failure.

---

## 1. Core distinction

ChineseTutor must separate:

```text
stale evidence
≠
demonstrated forgetting
```

A node can remain:

```text
mastery = M2
review_status = overdue
forgetting_risk = high
```

This means:

> We still carry the best supported estimate M2, but confidence in its current freshness is no longer adequate. Re-verify cheaply.

Only new learner evidence can support an actual M/A/C downgrade through C3.

---

## 2. Review pipeline

```text
LearnerNodeState
+ axis verification timestamps
+ evidence strength
+ retention family
+ qualifying recent Attempts
+ ActiveRequirements
        ↓
compute axis clocks
        ↓
current / due / overdue
        ↓
ReviewDecision
        ↓
F1 creates/selects TrainingMove(review or diagnose)
        ↓
choose cheapest valid Question/probe
        ↓
F2 tutors if needed
        ↓
C2 Attempt evidence
        ↓
C3 Profile projection
        ↓
refresh clock / contradiction / diagnosis
```

F3 never bypasses the existing evidence loop.

---

## 3. Axis-specific review debt

For each learner-node state, calculate separately:

```text
mastery_review_status
automation_review_status
complexity_review_status
```

One node may validly be:

```text
mastery: current
automation: due
complexity: current
```

Example: a student can still reason correctly but has stopped self-triggering the method under time pressure.

---

## 4. Review debt semantics

Derived ordinal view:

```text
unknown   not enough temporal evidence
none      current and no special reason to verify
light     due but low consequence / strong history
material  due/overdue with useful learning consequence
urgent    active requirement or downstream dependence makes freshness important now
```

`urgent` is not automatically `P0`. F1 still considers causal importance and executable alternatives.

---

## 5. Cheapest-valid-verification-first

When a node is stale, do not immediately reteach it.

Use this order:

1. **Can a recent ordinary Attempt already count as valid implicit review?** If yes, credit it and recompute.
2. **Can one short probe re-observe the due axis?** Use it.
3. **If the short probe is ambiguous, run a discriminator/diagnostic.**
4. **Only if fresh evidence shows weakness, enter teaching/scaffolded practice.**

Examples:

### 文言实词
Do not reopen the whole lesson. Ask one or several contextual retrieval items.

### 人物证据推理
Do not assign a full reading set if one compact unfamiliar evidence→warrant item can verify the construct.

### 写作细节
Do not require a full composition if one scene rewrite is sufficient.

---

## 6. What counts as successful review

A review is successful when the new Attempt produces evidence sufficient for the axis being checked.

### Mastery refresh
Typical requirements:

- target node actually observed;
- positive performance at representative demand;
- independence compatible with current mastery claim;
- no direct supply of the target operation/content.

### Automation refresh
Typical requirements:

- learner self-triggers the construct;
- no H2 strategy naming/recall cue if the goal is independent trigger;
- execution is low-friction enough for the claimed A level;
- A3 verification includes meaningful time/attention pressure when relevant.

### Complexity refresh
Typical requirements:

- node independently observed at comparable target complexity;
- relevant D2 dimensions are represented;
- a low-complexity item cannot refresh a higher-band claim merely because it is correct.

---

## 7. Review failure

A review failure is not processed by a special hidden rule.

Correct path:

```text
review probe
→ C2 negative/mixed evidence
→ C3 contradiction handling
```

Possible C3 outcomes:

- hold state, lower confidence;
- request another comparable verification;
- downgrade one axis after sufficiently strong recent contradiction;
- expose prerequisite/error pattern and redirect F1.

This prevents one bad retrieval attempt from erasing stable history while still allowing genuine regression to become visible.

---

## 8. Uncertain review result

If the learner misses an item but cause is unclear, use F2 discriminator logic.

Example:

```text
古诗默写错误
```

Possible causes include:

- exact memory loss;
- character/orthography slip;
- question misunderstanding;
- carelessness.

Do not automatically rewrite the Knowledge mastery state until the construct is actually diagnosed.

---

## 9. Implicit review from normal training

A learner should not receive redundant dedicated review when normal work already supplies strong current evidence.

Examples:

- a new文言文 translation independently retrieves a target实词 → may refresh that lexical node;
- a modern narrative question independently executes evidence→character reasoning → may refresh that reasoning node;
- an essay revision where the student independently selects a typical detail → may refresh the writing-material Ability.

But the attempt must explicitly contain valid node evidence. Mere co-occurrence in the text is insufficient.

---

## 10. Review scheduling and ActiveRequirements

ActiveRequirements can pull a verification earlier.

Example:

```text
normal due date: 30 days away
school assessment: 5 days away
```

For a core exact-retrieval node, F3 may schedule a short freshness check now.

This is not grade-based progression. It is a time-bounded requirement changing the value of fresh evidence.

Long-term Gaokao requirements may also request periodic transfer-retention checks without making every node permanently urgent.

---

## 11. Review priority handoff to F1

Default mapping:

```text
current                     → no review candidate
review due, stable history  → P3_maintenance
review overdue, core node   → P2_normal or P3 depending consequence
active requirement near     → may rise to P2/P1
recent contradiction        → P1 diagnostic/review
confirmed regressed hard prerequisite blocking active targets → P0/P1 via F1 causal rules
```

Time alone never creates P0.

---

## 12. Preventing review flood

As the graph grows toward Gaokao coverage, naive review scheduling could overwhelm new learning.

F3 therefore requires queue controls.

### 12.1 Batch cheap retrieval
Compatible exact-retrieval/recitation nodes can be reviewed in one short block while preserving per-node evidence.

### 12.2 Suppress redundant dedicated review
If a scheduled authentic task will validly observe the same node soon, prefer embedded review.

### 12.3 Use a maintenance budget
Default operational policy:

- normal session: review should usually consume no more than ~25% of intended training time;
- exceptions: imminent assessment, confirmed regression, or explicit review session.

This percentage is configurable and not a learning law.

### 12.4 Prioritize review by consequence
Core prerequisites and high-value exact retrieval outrank stale enrichment nodes.

### 12.5 Do not review all axes separately when one task can validly observe several
A well-chosen Question may refresh mastery and complexity together, or mastery and automation if self-trigger is also visible.

---

## 13. Avoiding variant inflation

Review should not use the same memorized surface form repeatedly when the construct requires flexible access.

- exact recitation may legitimately use the same canonical text;
- lexical/conceptual/reasoning review should vary contexts when needed;
- M3 retention should use genuinely different contexts rather than cosmetic variants;
- repeated near-identical variants are discounted exactly as in C3.

---

## 14. Review after repair

When a node was recently repaired with strong scaffold, schedule a **post-repair verification** sooner than the normal retention horizon.

Suggested default:

```text
post_repair_check = min(7 days, 0.35 × normal horizon)
```

for constructs where a short independent re-probe is practical.

Purpose:

> verify that supported learning became independently retrievable after delay.

This is especially useful after H5-H7 or explicit micro-teaching.

Again, the value is configurable.

---

## 15. Stable vs current

A node may be semantically stable for a requirement but temporally stale.

```text
readiness_status = stable
review_status = due
```

The dashboard should preserve both dimensions rather than replacing `stable` with `review_due` as though they were the same kind of state.

---

## 16. F3 invariants

1. Time creates uncertainty, not failure.
2. Review debt is per axis.
3. Successful normal work may serve as review only when it re-observes the construct.
4. Cheapest valid verification precedes reteaching.
5. Review failure enters C2/C3 like any other evidence.
6. Review timing is configurable and family-sensitive.
7. Review priority is integrated through F1, not a competing scheduler.
8. F2 remains responsible for within-question help during review probes.
9. Review queues must avoid flooding and redundant practice.
10. Delayed independent post-repair checks are valuable when scaffold was heavy.