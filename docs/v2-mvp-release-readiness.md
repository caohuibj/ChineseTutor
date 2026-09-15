# ChineseTutor v2 MVP — Release Readiness and Merge Plan

Status: **release contract draft**  
Issue: #38

This document defines the first point at which ChineseTutor v2 is considered **usable as a real learner system**, rather than merely well-specified.

The purpose is to stop architecture work from expanding indefinitely and establish a falsifiable MVP finish line.

---

## 1. What `v2 MVP usable` means

The v2 MVP is ready when the current learner can repeatedly use this production loop:

```text
Learner Profile
      ↓
TrainingMove selection
      ↓
Question selection
      ↓
first TrainingAttempt
      ↓
F2 minimal effective intervention
      ↓
second/subsequent TrainingAttempt
      ↓
C3 Profile update
      ↓
F3 review / next TrainingMove
      ↺
```

with:

- canonical LearningNodes and dependencies;
- traceable Material / TaskType / Question provenance;
- learner-specific Profile separated from canonical graph meaning;
- structured Attempt evidence;
- explainable recommendation and intervention decisions;
- non-destructive v1 coexistence / rollback;
- enough authentic content for the learner's **current active requirements**.

### MVP explicitly does **not** mean

- every Gaokao Knowledge node has already been populated;
- every high-school TaskType has a full question ladder;
- every #29–#36 gap is exhausted;
- recommendation weights are population-calibrated;
- a standalone app/UI exists;
- all tutoring or Notion writes are fully automated;
- the learner is asked to work through Gaokao papers now.

Gaokao remains the **coverage boundary**, not the prerequisite for first v2 use.

---

# 2. Current semantic stack to converge

Snapshot at creation of this release plan:

| Order | PR | Layer | Release role |
|---:|---:|---|---|
| 1 | #10 | Architecture v2 | root semantic contract |
| 2 | #11 | B1 LearningNode | canonical node model |
| 3 | #12 | B2 Dependency | graph relation model |
| 4 | #13 | C1 Learner Profile | personal state |
| 5 | #14 | C2 TrainingAttempt | evidence event |
| 6 | #15 | D1 Material / TaskType / Question | content entities |
| 7 | #16 | D2 Complexity | intrinsic task demand |
| 8 | #18 | C3 Evidence → Profile | profile projection |
| 9 | #19 | F1 TrainingMove | next-action recommendation |
| 10 | #22 | F2 Intervention | within-question tutoring |
| 11 | #27 | F3 Retention / Review | time / re-verification |
| 12 | #28 | G1 Mother Strategies | method consolidation |
| 13 | #37 | I1 Gaokao audit | terminal coverage stress-test |

B3's required **42-row migration disposition audit** is already contained in B1. Production row creation/backfill is not.

---

# 3. Merge train

Do **not** merge children out of order and do not treat current stacked `mergeable=false` flags as proof of semantic conflict before retargeting.

Use this convergence procedure:

```text
#10 → main
#11 → main
#12 → main
#13 → main
#14 → main
#15 → main
#16 → main
#18 → main
#19 → main
#22 → main
#27 → main
#28 → main
#37 → main
```

For each step:

1. finish review of the current PR;
2. ensure parent semantic assumptions still match the merged main branch;
3. retarget **only the next PR** from its stacked parent branch to `main`;
4. inspect the new diff for accidental parent duplication or missing files;
5. resolve any real conflict;
6. rerun the PR's semantic QA gates / representative fixtures;
7. merge;
8. close the corresponding implementation issue when its PR says `Closes #...` and acceptance criteria remain true;
9. repeat.

### Why sequential retargeting

Retargeting the full chain at once makes temporary diffs noisy and makes it harder to tell whether a conflict is semantic or only caused by the stack. Sequential convergence keeps every PR reviewable.

### #10 special rule

#10 is still the architecture root. Before merge:

- mark it ready only after confirming later PRs did not invalidate its entity boundaries;
- update only contradictions that would make `main` misleading;
- do **not** fold all later implementation details back into #10.

Later PRs remain the normative detail.

---

# 4. I1 gap classification for MVP

I1 correctly discovered gaps. Release readiness requires deciding which ones block first real use.

## 4.1 Pre-launch MUST

### #31 — Evidence sufficiency / claim validity Ability

**Classification:** `PRE-LAUNCH MUST`

Reason:

- high graph centrality across modern/information reading, classical interpretation, open evaluation and argumentation;
- current architecture already relies on evidence quality, but Profile cannot yet distinguish `found evidence` from `evidence is sufficient / appropriately bounded`;
- without this node, a central future recommendation/diagnosis can collapse into generic reasoning.

Minimum pre-launch scope:

- canonical Ability semantics;
- observable success / partial / failure conditions;
- B2 relations to evidence identification / reasoning abilities;
- recommended Strategy mappings;
- at least two representative authentic task mappings from different domains;
- C1/C2/C3 compatibility fixtures.

Full high-school material ladder may wait until post-launch.

---

## 4.2 Pre-launch MINIMUM SLICE, then continue post-launch

### #30 — Rule induction + novel-case application

**Classification:** `PRE-LAUNCH MINIMUM SLICE`

Before MVP pilot:

- canonicalize `rule/principle induction` and `rule/principle application` as separable Ability outcomes;
- add enough relations/fixtures to prevent one generic `信息概括` node from absorbing both.

Post-launch:

- expand transfer contexts and complexity ladders.

Reason: these are high-value cross-domain operations, but the MVP does not need exhaustive Gaokao examples before use.

### #34 — Authentic higher-order TaskTypes

**Classification:** `PRE-LAUNCH MINIMUM SLICE`

Before MVP pilot:

- ensure every TaskType actually used by the pilot inventory has a canonical stable identity and output contract;
- do not require every interview/Q&A/literary-micro-commentary form to be populated unless it appears in the pilot pool.

Post-launch:

- complete higher-order authentic communication / commentary families.

### #35 — Gaokao-supporting Knowledge Graph

**Classification:** `PRE-LAUNCH MINIMUM SLICE`

Before MVP pilot, populate only:

1. Knowledge directly required by the learner's current school materials;
2. Knowledge required by pilot Questions;
3. direct hard prerequisites for the pilot's target Abilities;
4. enough Knowledge families to validate retention behavior for both exact retrieval and conceptual knowledge.

Post-launch:

- systematically complete Gaokao Knowledge families across language, classical, poetry/literature, argument, writing and culture.

### #36 — Authentic complexity ladders

**Classification:** `PRE-LAUNCH MINIMUM SLICE`

Before MVP pilot, build a minimum useful ladder for the highest-value active nodes, not for the whole graph.

Minimum pilot target set should include approximately 5–7 high-value nodes/families such as:

- evidence → explanation → conclusion;
- evidence-to-character judgment / typical-event reasoning;
- information integration/comparison;
- classical contextual word sense → translation;
- poetry evidence → emotion;
- writing constraint parsing / material selection;
- evidence sufficiency once #31 is canonicalized.

For each selected family, inventory should support at least:

```text
routine / bounded practice
+ independent verification
+ one meaningfully different transfer/integrated condition where appropriate
```

Post-launch expands this to broader graph coverage.

---

## 4.3 Post-launch expansion — not MVP blockers

### #29 — Representation / discourse transformation

**Classification:** `POST-LAUNCH`, unless the current pilot explicitly uses text→table / prose→Q&A transformation as a target.

Current architecture can still represent the task using existing information/constraint abilities during MVP. The canonical Ability should be added soon, but first learner use should not wait on it.

### #32 — Purpose-driven question formulation

**Classification:** `POST-LAUNCH`

Important for authentic communication and later Gaokao coverage, but not required for the current learner's core reading/writing loop.

### #33 — Multi-constraint solution synthesis

**Classification:** `POST-LAUNCH`

Important high-order transfer Ability. Existing lower-level constraint parsing and information integration are sufficient for first MVP use; canonical synthesis should follow after pilot unless current tasks require it.

---

# 5. MVP release gates

A release is **GO** only when all blocker gates below pass.

## Gate A — Semantic baseline merged

Required:

- #10, #11, #12, #13, #14, #15, #16, #18, #19, #22, #27, #28, #37 merged to `main` in dependency order;
- no unresolved semantic contradiction among canonical entities;
- release-blocking additions from #31 and minimum #30 slice merged;
- all relevant schema versions identifiable.

Failure → **NO-GO**.

## Gate B — Production v2 data model exists alongside v1

Required production entities / equivalent data sources:

- LearningNode;
- LearningEdge / dependency representation;
- LearnerNodeState;
- Material;
- TaskType;
- Question;
- TrainingAttempt;
- ProfileUpdateDecision or equivalent audit trail;
- TrainingMove / queue representation;
- review/verification state or auditable derived equivalent.

Required safeguards:

- v1 data retained;
- migration mapping preserved;
- no destructive field deletion;
- rollback path documented;
- private learner data stays out of the public GitHub repository.

Failure → **NO-GO**.

## Gate C — Conservative backfill is complete enough to start

Required:

- all current 42 v1 capability rows have their reviewed B1 migration disposition applied or explicitly queued;
- split/merge rows do not copy one learner mastery state blindly to all new nodes;
- `unknown` remains distinct from weak;
- at least the current learner's active materials/questions are represented using D1 entities;
- historical sessions are marked reconstructed / lower-confidence when native Attempt telemetry did not exist.

Failure → **NO-GO**.

## Gate D — Current-learner coverage is sufficient

Before pilot:

- current school/teacher materials can be linked to canonical nodes;
- current active Knowledge prerequisites exist;
- pilot TaskTypes have stable contracts;
- the minimum #36 complexity ladders exist for selected high-value nodes;
- lack of a future Gaokao node does not prevent current training unless it is an actual prerequisite/target.

Failure → **NO-GO for the affected domain**, not automatically for every other domain.

## Gate E — Runtime tutoring contract works end to end

A real session must be able to execute:

```text
select/confirm TrainingMove
→ choose Question
→ capture first Attempt
→ diagnose earliest causal failure
→ give one minimum-effective intervention
→ capture retry
→ create auditable Profile update
→ decide continue / fade / transfer / review / stop
```

Required:

- reasoning-correct/expression-weak route works;
- H6/H7 does not masquerade as independent evidence;
- unknown prerequisite can trigger diagnose rather than false failure;
- recommendation reason and success criterion are visible/auditable;
- data-write failure cannot silently create false Profile state.

Failure → **NO-GO**.

## Gate F — Pilot evidence passes

Minimum pilot before declaring v2 MVP operational:

- **20 native TrainingAttempts minimum**; 30 recommended before normal use, 40 for stronger confidence;
- at least **4 domains/task families**, including classical Chinese and modern reading plus at least two of poetry, language use, writing;
- at least **5 first-answer → intervention → retry sequences**;
- at least one `reasoning correct / written expression weak` case if naturally observed or deliberately diagnosed using an appropriate task;
- at least one scaffold-fade or automation move;
- at least one review/re-verification decision or an explicit finding that insufficient time has elapsed to test F3 naturally.

These counts are **engineering release floors for exposing integration defects**, not psychometric sample-size claims and not mastery thresholds. They must not be reused as automatic C3 promotion rules.

Every pilot Attempt used as Profile evidence must be traceable to:

```text
Question + target node
+ support condition
+ observation
+ ProfileUpdateDecision
```

Release blocker conditions:

- a Profile upgrade cannot be explained from evidence;
- repeated false prerequisite blocking occurs;
- tutor repeatedly reteaches comprehension when only expression is weak;
- same-variant practice fabricates transfer/M3;
- Attempt writes lose first/second-answer distinction;
- v1 data is overwritten/deleted without rollback;
- TrainingMove repeatedly selects unavailable/ineligible questions without surfacing a content-supply gap.

Any release blocker → **NO-GO until fixed and re-tested**.

## Gate G — Dual-run and rollback are proven

During pilot:

- keep v1 learning records/workflow readable;
- write v2 evidence in parallel;
- compare at least a sample of session summaries against raw v2 Attempt evidence;
- document how to return to v1-only operation if v2 data logic fails;
- do not hide/deprecate old production fields until pilot passes.

Failure → **NO-GO for cutover**, though v2 pilot may continue.

---

# 6. The v2 MVP finish line

The project crosses the MVP finish line when all of the following are true:

```text
1. semantic stack is on main
2. #31 + minimum #30 semantics exist
3. production v2 schema exists side-by-side with v1
4. current learner materials and core nodes are backfilled
5. minimum authentic ladders exist for active high-value nodes
6. chat/runtime executes the full evidence loop
7. >=20 native Attempts pass the pilot gates
8. no unresolved release-blocking semantic/data/tutoring defect remains
9. rollback to v1 is possible
```

At that point the project must change mode:

> **Stop architecture-first development. Start normal learner use and fill graph/content gaps from real evidence.**

---

# 7. Immediate execution sequence after this PR

```text
R1.1  converge/merge #10 → #37 into main
R1.2  close #31
R1.3  close minimum semantic slice of #30
R1.4  define production Notion v2 schema + migration manifest
R1.5  backfill current graph/profile/content non-destructively
R1.6  populate current-learner Knowledge + TaskTypes + minimum #36 ladders
R1.7  wire the chat/runtime write sequence
R1.8  run 20–40 native Attempt pilot
R1.9  fix release blockers
R1.10 declare v2 MVP operational and begin post-launch backlog
```

Post-launch priority begins with remaining #29–#36 work based on observed learner value, not issue number.

---

# 8. Release discipline

Until MVP pilot begins:

- new architecture entity types require evidence that B1–I1 genuinely cannot represent the need;
- new mother Strategies require G1 admission evidence;
- new TaskTypes must represent stable operation/output contracts, not one exam stem;
- new Knowledge/Ability nodes require observable learner-state meaning;
- grade/year remains provenance only;
- no exhaustive content expansion may delay a currently usable learner loop.

The release objective is therefore not “finish the whole graph”. It is:

> **make the graph/profile/evidence loop trustworthy enough to learn from real use.**
