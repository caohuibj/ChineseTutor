# QA gates — TrainingMove recommendation engine (F1)

These gates are normative acceptance checks for Issue #7.

## Gate 1 — Recommend a move, not merely a Question

Fail if output is only:

```text
Q-123
```

Pass only when target node, move type, conditions, rationale and success criterion are explicit.

## Gate 2 — Observed Profile and desired Requirement are separate

Fail if the engine calls a node “behind” without a relevant target requirement.

Pass when current and long-term requirements can coexist without grade staging.

## Gate 3 — Unknown is not maximum weakness

An untested node does not outrank demonstrated core gaps merely because state is null.

## Gate 4 — Hard prerequisite deficit can block independent downstream training

When B `requires` A and demonstrated failure of A makes B unobservable, independent B practice is ineligible as the primary move.

## Gate 5 — Unknown prerequisite triggers diagnosis, not assumed failure

Fail if null prerequisite state automatically blocks all downstream work.

## Gate 6 — `supports` does not become a hard block

A weak supporting node may raise value but cannot automatically make the target ineligible.

## Gate 7 — Central bottleneck can outrank isolated lower mastery

Pass when a shared evidence-reasoning bottleneck outranks an isolated task-specific weakness because it unlocks multiple core targets.

Fail if ranking is simply lowest M first.

## Gate 8 — Raw graph degree is not unlock value

Only relevant downstream targets and plausible causal relations count.

## Gate 9 — Priority is explainable without one opaque score

Selected priority class must be reconstructable from eligibility + semantic factors + tie-breaks.

## Gate 10 — School relevance is a tie-breaker/preference

Fail if current school unit becomes the global training path.

Pass when comparable canonical moves prefer timely school material.

## Gate 11 — Transfer is not C4

A C2 unfamiliar H0 task can be selected for transfer. A familiar C4 task cannot prove transfer merely by band.

## Gate 12 — Fade scaffold holds semantic demand roughly stable

When the goal is reducing H3→H1/H0 dependence, do not simultaneously increase text load, reasoning depth and openness without explicit reason.

## Gate 13 — Automation does not equal “harder question”

Pass when automation training raises self-trigger/time pressure while holding reasoning demand near current verified level.

## Gate 14 — Complexity extension is controlled

Prefer deliberate increases in one/few D2 dimensions. Fail if every extension means “raise all dimensions”.

## Gate 15 — Same TaskType can produce different moves

Required regression case: the same人物形象 TaskType yields establish/scaffold/transfer moves for different Profiles.

## Gate 16 — Review seeks cheapest valid verification first

Previously stable but stale knowledge should receive short re-verification before full reteaching.

## Gate 17 — Expression failure does not trigger comprehension reteach

If evidence shows reasoning is correct and written expression is weak, recommend expression conversion/revision, not another comprehension lecture.

## Gate 18 — Question availability affects executability, not need

No matching Question can defer execution or create a content-gap request, but cannot erase the learner need.

## Gate 19 — Abundant inventory does not create training priority

Fail if the system trains a node merely because many questions exist.

## Gate 20 — Authenticity fits evidence purpose

Transfer/high-stakes validation should prefer traceable authentic Questions. Narrow diagnosis may use clearly labeled generated/editorial probes.

## Gate 21 — Familiarity is learner-relative

A Question cannot be statically marked “unfamiliar for everyone”. Selection checks learner history / variant exposure.

## Gate 22 — Success criterion is local and testable

Every move has an explicit stopping condition.

Fail vague criteria such as “练熟一点”.

## Gate 23 — Move completion does not bypass C3

A successful TrainingMove produces Attempts. C3 still determines actual Profile update.

## Gate 24 — Recommendation can stop after one informative diagnostic

Do not require arbitrary attempt counts after uncertainty is resolved.

## Gate 25 — Recompute after meaningful evidence, not every metadata edit

Recommendation stability should avoid thrashing between moves without new evidence.

## Gate 26 — Long-form writing cost is respected

If a narrow writing subskill can be tested through a paragraph/revision, do not require a full essay solely to collect evidence.

## Gate 27 — Question selection uses eligibility before fit ranking

An ineligible question cannot win via high soft-fit score.

## Gate 28 — Recent near-identical variants are excluded when diversity matters

Especially for M2/M3 validation and transfer probes.

## Gate 29 — Requirement conflict is surfaced

If current-school and long-term requirements imply different actions, expose the tradeoff; do not silently redefine the target.

## Gate 30 — Why-now explanation has four parts

A selected move should answer:

1. current gap/uncertainty;
2. why the node matters;
3. why this move shape/support/complexity;
4. how completion will be judged.

---

# Representative regression scenarios

- current Zhou Yafu character reasoning -> fade scaffold, not reteach labels;
- same TaskType novice -> establish location/evidence prerequisite;
- same TaskType M2/A2 -> unfamiliar H0 transfer probe;
- classical lexical M0 blocks independent translation;
- unknown lexical prerequisite -> cheap diagnosis;
- shared evidence-warrant bottleneck outranks isolated title weakness;
- stale M2 lexical knowledge -> cheap review probe;
- reasoning strong / expression weak -> micro-writing conversion task;
- missing Question supply -> content gap + next executable move;
- M2/A1 -> automation without increasing reasoning depth.

---

# F1 definition of done

- [x] TrainingMove schema defined;
- [x] active requirement contract defined;
- [x] move-type mapping defined;
- [x] prerequisite blocking/uncertainty rules defined;
- [x] explainable priority classes/tie-breakers defined;
- [x] question-selection contract defined;
- [x] support/complexity/novelty controls separated;
- [x] success criteria/stopping rules defined;
- [x] same-TaskType different-Profile scenarios added;
- [x] current project scenario included;
- [ ] semantic self-review complete;
- [ ] stacked PR opened.