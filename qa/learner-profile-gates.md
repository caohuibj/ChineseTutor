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
evidence_strength: none
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

## Gate 5 — Complexity ceiling is verified, not attempted maximum

Fail:

```text
learner attempted one C4 item -> complexity_ceiling = C4
```

Pass only when evidence sufficiently supports successful performance at the band.

Complexity may later downgrade when evidence becomes stale/contradictory.

---

## Gate 6 — Evidence strength is explicit

The system must distinguish:

```text
M2 + weak evidence
M2 + strong evidence
```

Fail if both appear identical in Profile or recommendation logic.

---

## Gate 7 — `last_trained_at` and `last_verified_at` are separate

Training exposure is not verification.

Fail if every practice session automatically updates `last_verified_at`.

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

## Gate 13 — Counts are not mastery formulas

Fail:

```text
attempt_count >= 5 => M2
```

unless C2 later defines a richer evidence rule incorporating independence, diversity, complexity, recency and hints.

Counts are cached summaries only.

---

## Gate 14 — Error pattern requires repetition or explicit evidence

One error may be recorded at Attempt level but must not automatically become the profile's `primary_error_pattern`.

Pass only when pattern is stable enough to guide intervention.

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

Legacy-only merge backfill should normally cap at M2 and expose weak/moderate evidence.

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

## Scenario A — current student character reasoning

Historical evidence shows correct direction after prompting, but written inferential link and automatic invocation remain weak.

Plausible C1 state:

```text
M1/A1/C2
weak evidence
primary error: E
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
last_verified old
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
- [x] evidence strength/provenance defined without false precision;
- [x] recency/forgetting fields defined;
- [x] stable/developing/bottleneck/review-due semantics normalized into orthogonal dimensions;
- [x] error-pattern summary rules defined;
- [x] v1 backfill rules cover one-to-one, split, task-only and merge/generalize cases;
- [x] grade/stage fields excluded from ability state;
- [x] profile downgrade and uncertainty are supported;
- [ ] semantic self-review completed;
- [ ] stacked PR opened.
