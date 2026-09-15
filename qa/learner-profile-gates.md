# QA gates — Learner Profile / LearnerNodeState (C1)

These gates are normative acceptance checks for Issue #3.

## Gate 1 — Canonical meaning and personal state are separate

Given one canonical node, multiple learners can have independent states:

```text
CN-A-evidence-to-character-judgment
  learner A -> M3/A2/C4
  learner B -> M1/A1/C1
  learner C -> unknown
```

Fail if node definition/schema changes by learner.

---

## Gate 2 — Unknown is not M0/A0/C0

No evidence must produce:

```text
mastery: null
automation: null
verified_complexity_band: null
mastery_evidence_strength: none
automation_evidence_strength: none
complexity_evidence_strength: none
```

Fail if newly added nodes default to M0/A0/C0.

---

## Gate 3 — Mastery and automation can disagree

Pass:

```text
M2 / A1
```

for a learner who can perform after activation but does not self-trigger the method.

Pass:

```text
M1 / A2
```

for a learner who automatically invokes a familiar procedure but still executes it poorly.

Fail if schema or update rules force automation to equal mastery.

---

## Gate 4 — M3 requires diversity/transfer, not one success

Reject M3 based on:

- one correct answer;
- several near-identical questions;
- only familiar textbook material;
- success after heavy hinting.

C2 will define exact evidence aggregation, but C1 must preserve this invariant.

---

## Gate 5 — Complexity band is verified, not attempted maximum

Fail:

```text
learner attempted one C4 item -> verified_complexity_band = C4
```

Pass only when evidence sufficiently supports successful performance at the band.

Complexity may later downgrade when evidence becomes stale/contradictory.

---

## Gate 6 — Evidence strength is explicit and axis-specific

The system must distinguish:

```text
mastery M2 + mastery evidence strong
automation A1 + automation evidence weak
```

Fail if one global evidence-strength field makes confidence on Mastery, Automation and Complexity indistinguishable.

---

## Gate 7 — Training recency and axis verification recency are separate

Training exposure is not verification.

Required timestamps:

```text
last_trained_at
mastery_verified_at
automation_verified_at
complexity_verified_at
```

Fail if an untimed practice answer refreshes automation verification automatically, or every practice session refreshes all dimensions.

---

## Gate 8 — Current state is allowed to downgrade

Pass transitions when evidence justifies them:

```text
M2 -> M1
A2 -> A1
C3 -> C2
```

Fail if profile is monotonic upward by schema design.

---

## Gate 9 — Status dimensions are not collapsed semantically

A node can be:

```text
developing + bottleneck candidate + review due
```

Therefore the system must preserve orthogonal fields:

```text
readiness_status
bottleneck_status
review_status
```

A single `attention_status` may summarize for UI only.

Fail if one enum destroys the other dimensions.

---

## Gate 10 — Bottleneck is not “lowest score”

A confirmed bottleneck needs:

- actual learner deficit/uncertainty;
- meaningful downstream graph impact;
- plausible causal relation to downstream failures/unlock value.

Fail if `M0/M1 => bottleneck` automatically.

---

## Gate 11 — Stable is relative to an active requirement

Fail if `stable` means “permanently complete”.

Pass if the system can represent:

```text
observed M2/C2
stable for current target
but eventual Gaokao target may require C4
```

Observed state remains distinct from goal comparison.

---

## Gate 12 — Graph edges never auto-propagate mastery

Reject:

```text
prerequisite M3 -> dependent gets mastery
transfer source M3 -> target gets mastery
parent mastery -> child mastery
child mastery -> parent mastery
```

Edges guide testing/recommendation, not evidence substitution.

---

## Gate 13 — Counts are nullable caches, not mastery formulas

During legacy migration:

```text
attempt_count: null
```

means “historical count unknown”.

It must not be normalized to zero.

Also fail:

```text
attempt_count >= 5 => M2
```

unless later evidence logic also considers independence, diversity, complexity, recency and hints.

---

## Gate 14 — Error pattern requires repetition and confidence

One mistake may exist at Attempt level but must not automatically become profile `primary_error_pattern`.

A profile error pattern should carry `error_pattern_evidence_strength` and be stable enough to guide intervention.

---

## Gate 15 — Split-node backfill is conservative

Given v1:

```text
修辞与表达效果 mastery=2
```

and v2 split:

```text
Knowledge: 修辞知识
Ability: 解释修辞语境效果
```

Fail if both children automatically receive M2.

Expected: unknown/low-confidence child states unless historical evidence distinguishes them.

---

## Gate 16 — Task-Type-only v1 labels do not create fake profile nodes

If v1 label migrates primarily to Task Type (`标题含义与作用`, `炼字`, etc.), do not create `LearnerNodeState` for a non-canonical pseudo-node merely to preserve an old mastery number.

Historical task performance must later map through Question/Attempt evidence.

---

## Gate 17 — Cross-domain merge does not fabricate transfer

If modern and classical character reasoning merge into one shared Ability, success in one domain may support the shared estimate but cannot by itself prove cross-domain M3 transfer.

Legacy-only merge backfill should normally cap at M2 and expose weak/moderate confidence.

---

## Gate 18 — Legacy `待加强` is not Bottleneck

Backfill may create:

```text
attention_reason: legacy v1 待加强
```

but bottleneck stays unknown until graph/evidence criteria are met.

---

## Gate 19 — Stage/grade planning metadata does not become ability state

Reject mapping:

```text
阶段定位=高中展开 -> complexity C4
当前重点 -> bottleneck
初二达标标准 -> mastery target embedded in Profile
```

Grade/stage is not a state axis.

---

## Gate 20 — Recommendation action is not stored as Profile truth

Profile can explain attention reasons, but exact next Question/TrainingMove belongs to recommendation/scheduling layers.

Reject fields such as:

```text
next_question_id
must_do_tomorrow
current_recommended_move
```

as canonical LearnerNodeState evidence fields.

---

# Representative acceptance scenarios

## Scenario A — character reasoning

Evidence shows correct reasoning after activation, but automatic invocation and written inferential bridge remain weaker.

Valid state:

```text
mastery M2 / mastery evidence moderate
automation A1 / automation evidence weak
complexity C2 / complexity evidence moderate
primary error E / error-pattern evidence moderate
```

This must not be interpreted as “does not understand character analysis”.

## Scenario B — unseen graph node

A high-value Gaokao reasoning node is added but never tested.

Expected:

```text
unknown / evidence none
```

not M0.

## Scenario C — old memorization success

```text
mastery M2
mastery_verified_at old
forgetting high
review due
```

Prior mastery is not immediately deleted; system asks for efficient re-verification.

## Scenario D — strong strategy but weak prerequisite

```text
translation strategy A2
classical word-sense M1
translation ability M1
```

Tutor should diagnose lexical prerequisite rather than re-teach the five-step strategy.

---

# C1 definition of done

- [x] per-learner/per-node identity defined;
- [x] M0-M3 semantics defined with node-type interpretation;
- [x] A0-A3 semantics defined independently from mastery;
- [x] verified complexity band semantics defined;
- [x] evidence confidence defined per M/A/C axis;
- [x] verification recency defined per M/A/C axis;
- [x] nullable legacy counters distinguished from zero;
- [x] recency/forgetting semantics defined;
- [x] stable/developing/bottleneck/review-due normalized into orthogonal dimensions;
- [x] error-pattern summary confidence defined;
- [x] v1 backfill rules cover one-to-one, split, task-only and merge/generalize cases;
- [x] grade/stage fields excluded from ability state;
- [x] profile downgrade and uncertainty supported;
- [x] semantic self-review completed;
- [ ] stacked PR opened.
