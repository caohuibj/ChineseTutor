# F1 semantic self-review summary

Status: **review complete**

F1 was reviewed against the project goals: graph-first progression, dynamic Learner Profile, authentic evidence, minimal effective intervention, and no grade-based learning ladder.

## What the first draft got right

- recommendation unit is a TrainingMove rather than a Question;
- Profile, graph, recent evidence and Question inventory are separate inputs;
- same TaskType can receive different support/novelty/complexity depending on the learner;
- `requires` can block independent downstream evidence while `supports` cannot;
- transfer is learner-relative and independent of D2 C-band;
- recommendation uses explainable priority classes rather than a single opaque score;
- success criteria are explicit and C3 remains responsible for Profile updates.

## Structural risks found and fixes

### 1. “Gap” was undefined without a target requirement

Risk:

```text
Profile says M2, but is M2 enough?
```

Without a desired state, the recommender cannot distinguish “stable for now” from “still below terminal target”.

Fix:

Added `recommendation/active-requirement-contract.md` separating observed Profile from current/long-term desired state. Current reachable requirements and long-term Gaokao requirements can coexist without grade staging.

### 2. Unknown prerequisite could be over-blocked

Risk: dependency traversal might treat null state as missing and push the learner backwards unnecessarily.

Fix: only **demonstrated** hard-prerequisite deficit blocks independent downstream training. Decision-critical unknown prerequisites produce a cheap `diagnose` move.

### 3. A lowest-score recommender would miss high-leverage bottlenecks

Risk: an isolated M0 task could outrank a shared M1 reasoning bottleneck.

Fix: priority uses causal dependency unlock + Gaokao relevance + learner gap + information value. Raw graph degree is explicitly rejected.

### 4. Maintenance work could be permanently starved

Risk: if all review is fixed at P3, core lexical/memorization knowledge may decay until it causes larger failures.

Fix: overdue/high-forgetting core maintenance can escalate to P1; demonstrated regression that blocks an active core target can become P0/P1 repair.

### 5. School relevance needed a deadline exception without reintroducing grade progression

Risk: “school relevance is only a tie-breaker” is too weak when there is a real near-term assessment.

Fix: school/unit identity remains only context; an explicit `school_sync` ActiveRequirement with a real deadline may temporarily raise priority. The reason is the requirement/deadline, not grade.

### 6. Recommendation could thrash after every Attempt

Risk: a highly reactive engine might switch targets constantly, preventing a learner from completing a local learning cycle.

Fix: added anti-thrashing rule. Keep the current move until its evidence purpose is satisfied, invalidated, saturated, or superseded by a materially higher-value blocker.

### 7. Repeated practice could remain high priority after its evidence purpose was met

Risk: globally important nodes could consume unlimited same-form practice.

Fix: recent saturation / diminishing evidence value lowers marginal priority. After M2 routine evidence is sufficient, shift to transfer/automation/another need instead of more identical items.

### 8. Question inventory could distort learner need

Risk: abundant content gets practiced while important gaps with weak supply disappear.

Fix: separated Need from Executability. `no_match` produces a content-supply gap while preserving the learner need and optionally selecting the next executable move.

### 9. Authentic questions are not always the most efficient diagnostic tool

Risk: insisting on full authentic tasks for every micro-diagnosis adds unnecessary load.

Fix: authentic traceable Questions are preferred for transfer/high-stakes validation; short clearly labeled generated/editorial probes are allowed for narrow diagnosis/scaffold fading.

### 10. Long-form writing could be overused as evidence

Risk: every writing weakness becomes “write another full essay”.

Fix: question-selection policy explicitly prefers paragraph, micro-writing or targeted revision when the target evidence is local expression/organization.

## Current semantic position

F1 now defines four distinct layers:

```text
ActiveRequirement: what level currently matters
Learner Profile: what current evidence says
TrainingMove: what evidence/training action should happen next
Question selection: which concrete task best realizes that move
```

After the learner responds:

```text
TrainingAttempt -> C3 update -> new Profile -> recompute recommendation
```

## Important non-goals retained

F1 does not yet:

- implement a numerical ML ranking model;
- define exact forgetting curves;
- perform live Notion migration;
- guarantee final Gaokao target levels for every node;
- author missing Questions automatically;
- replace the tutor's within-question diagnosis/intervention logic.

## Review verdict

The F1 semantics are coherent enough to open as a stacked PR on C3. The next architecture work should focus on review/forgetting scheduling and/or formal tutor intervention orchestration, then run end-to-end scenarios before production Notion migration.