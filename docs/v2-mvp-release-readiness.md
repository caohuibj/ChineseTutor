# ChineseTutor v2 MVP — Release Readiness / Merge Plan

Issue: #38  
Purpose: define the first **usable v2** finish line and stop architecture-first expansion.

## 1. MVP definition

`v2 MVP usable` means the current learner can repeatedly run:

```text
Profile
→ TrainingMove
→ Question
→ first Attempt
→ minimum-effective intervention
→ retry
→ Profile update
→ review / next move
↺
```

with canonical graph semantics, traceable Material/Question provenance, structured Attempt evidence, explainable Profile updates, and non-destructive v1 coexistence.

MVP does **not** require:

- exhaustive Gaokao Knowledge population;
- every future TaskType or question ladder;
- a standalone UI/app;
- population-calibrated recommendation weights;
- full automation of Notion writes.

Gaokao is the **coverage boundary**, not the prerequisite for first v2 use.

---

## 2. Merge train: #10 → #37

Current semantic stack:

```text
#10 Architecture v2
#11 B1 LearningNode
#12 B2 Dependency
#13 C1 Learner Profile
#14 C2 TrainingAttempt
#15 D1 Material / TaskType / Question
#16 D2 Complexity
#18 C3 Evidence → Profile
#19 F1 TrainingMove
#22 F2 Intervention
#27 F3 Retention / Review
#28 G1 Mother Strategies
#37 I1 Gaokao coverage audit
```

B3's 42-row **migration disposition audit** is already inside B1; production backfill is not.

### Merge procedure

Merge in exactly that order.

For each PR:

1. review current semantics;
2. merge its parent;
3. retarget **only this next PR** to `main`;
4. inspect the retargeted diff for duplicated/dropped parent changes;
5. rerun its QA/fixtures;
6. resolve real conflicts and merge;
7. continue to the next child.

Do not interpret a current stacked `mergeable=false` as a semantic conflict before retargeting.

For #10, only correct contradictions exposed by later work; do not fold all later schemas back into the architecture PR.

---

## 3. I1 gaps: what blocks MVP?

| Issue | MVP classification | Minimum pre-launch scope |
|---|---|---|
| #31 Evidence sufficiency / claim validity | **MUST** | canonical Ability, observable partial/failure states, B2 relations, Strategy mappings, cross-domain fixtures |
| #30 Rule induction + rule application | **MINIMUM SLICE** | separate the two Ability outcomes + basic relations/fixtures |
| #34 Higher-order TaskTypes | **MINIMUM SLICE** | only stable TaskTypes actually used by pilot Questions |
| #35 Knowledge Graph completion | **MINIMUM SLICE** | current school/pilot Knowledge + direct prerequisites + enough exact/conceptual Knowledge to exercise F3 |
| #36 Authentic complexity ladders | **MINIMUM SLICE** | ladders for ~5–7 active high-value node families |
| #29 Representation/discourse transformation | **POST-LAUNCH** unless pilot directly targets it | none by default |
| #32 Purpose-driven question formulation | **POST-LAUNCH** | none |
| #33 Multi-constraint solution synthesis | **POST-LAUNCH** unless active task requires it | none by default |

### Minimum #36 pilot ladders

Use current authentic material to cover roughly 5–7 high-value families, for example:

- evidence → explanation → conclusion;
- character / typical-event reasoning;
- information integration / comparison;
- classical contextual word sense → translation;
- poetry evidence → emotion;
- writing constraint parsing / material selection;
- evidence sufficiency after #31.

Each selected family should provide, where semantically appropriate:

```text
routine/bounded practice
+ independent verification
+ a meaningfully different transfer/integrated condition
```

This is enough to start. Full graph ladders are post-launch work.

---

## 4. MVP GO / NO-GO gates

### Gate A — Semantic baseline

GO only if:

- #10–#37 merge train is on `main`;
- #31 is merged;
- minimum #30 semantics are merged;
- no unresolved entity-boundary contradiction remains.

### Gate B — Production v2 data exists beside v1

Required operational entities or auditable equivalents:

```text
LearningNode
LearningEdge/dependencies
LearnerNodeState
Material
TaskType
Question
TrainingAttempt
ProfileUpdateDecision
TrainingMove/queue
review/verification state
```

Required safeguards:

- v1 stays intact;
- migration mappings are retained;
- no destructive deletion before validation;
- rollback is documented;
- private learner data stays out of public GitHub.

### Gate C — Conservative backfill

GO only if:

- all 42 v1 capability rows have their reviewed disposition applied or explicitly queued;
- split/merge rows do not blindly copy old mastery to every new node;
- `unknown` remains different from weak;
- current active Materials/Questions are represented in D1 entities;
- reconstructed historical evidence is labeled lower-confidence rather than inventing precise telemetry.

### Gate D — Current learner has enough graph/content coverage

Required:

- current school/teacher materials map to canonical nodes;
- required Knowledge prerequisites exist;
- pilot TaskTypes have stable contracts;
- minimum #36 ladders exist.

A missing future Gaokao node blocks only the affected task/domain, not the whole MVP.

### Gate E — Runtime loop works end to end

A real session must execute:

```text
TrainingMove
→ eligible Question
→ first Attempt
→ causal diagnosis
→ one minimum-effective intervention
→ retry
→ auditable Profile update
→ continue/fade/transfer/review/stop
```

Required behaviors:

- reasoning-correct/expression-weak routes to expression repair;
- unknown prerequisite triggers diagnose, not assumed failure;
- H6/H7 never masquerades as independent evidence;
- first and second answers remain distinct;
- write failure cannot silently create a Profile update.

### Gate F — Native pilot passes

Engineering release floor:

- **≥20 native TrainingAttempts**; 30 preferred before normal use;
- **≥4 task/domain families**, including modern reading and classical Chinese;
- **≥5 first-answer → intervention → retry sequences**;
- at least one scaffold-fade/automation move;
- review/re-verification is exercised when naturally due, or explicitly marked not-yet-testable because insufficient time elapsed.

These counts are integration-test floors, **not psychometric sample sizes or C3 mastery thresholds**.

Every Profile-relevant Attempt must trace to:

```text
Question + target node
+ support condition
+ node observation
+ ProfileUpdateDecision
```

NO-GO examples:

- unexplained mastery/automation/complexity upgrade;
- repeated false prerequisite blocking;
- comprehension reteaching when only expression is weak;
- same-variant repetition fabricating transfer/M3;
- first/second Attempt evidence being overwritten;
- destructive v1 migration;
- recommendation repeatedly selecting ineligible/unavailable Questions without surfacing a supply gap.

### Gate G — Dual-run / rollback

During pilot:

- keep v1 readable;
- write v2 in parallel;
- compare sample session recaps against raw v2 evidence;
- prove return to v1-only operation;
- do not deprecate/hide old production fields until pilot passes.

---

## 5. The v2 MVP finish line

Declare v2 operational only when:

```text
1. #10–#37 is merged to main
2. #31 + minimum #30 semantics exist
3. production v2 schema runs beside v1
4. current learner graph/content is backfilled
5. minimum authentic ladders exist
6. chat/runtime executes the full evidence loop
7. ≥20 native Attempts pass release gates
8. no unresolved release-blocking defect remains
9. rollback is proven
```

Then change development mode:

> **Stop architecture-first work. Use v2 normally and fill Knowledge/Ability/Task/material gaps from real learner evidence.**

---

## 6. Immediate execution after this PR

```text
R1.1  merge/retarget #10 → #37 into main
R1.2  implement #31
R1.3  implement minimum #30 slice
R1.4  create production Notion v2 schema + migration manifest
R1.5  non-destructive graph/profile/content backfill
R1.6  fill current-learner Knowledge + TaskTypes + minimum #36 ladders
R1.7  wire the chat/runtime write sequence
R1.8  run 20–40 native Attempt pilot
R1.9  fix release blockers
R1.10 declare v2 MVP operational
```

After R1.10, remaining #29–#36 work is prioritized by observed learner value rather than by issue number.
