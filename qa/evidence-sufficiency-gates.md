# QA gates — evidence sufficiency / claim support Ability

These gates close I1 Issue #31 without introducing a new architecture layer.

## Canonical-node semantics

- [x] one canonical Ability exists with stable ID `CN-A-evaluate-evidence-sufficiency-for-claim`.
- [x] primary domain is `META`, because the operation recurs across modern/information reading, classical interpretation and writing.
- [x] the definition names an observable operation, not an exam stem or broad course label.
- [x] success distinguishes relevant evidence from collectively sufficient evidence.
- [x] success includes warrant/coverage/boundary control rather than evidence counting.
- [x] scope excludes pure evidence location/extraction.
- [x] scope excludes formal-logic validity as a whole discipline.
- [x] scope excludes generic `critical thinking` as an undifferentiated learner score.
- [x] grade/year does not appear in node identity or progression semantics.

## Partial-state diagnosis

- [x] `relevant-but-insufficient` can be observed as `mixed` rather than forced into positive/negative only.
- [x] `evidence-list-without-warrant` is distinguishable from failure to locate evidence.
- [x] `overclaim` is distinguishable from evidence falsity.
- [x] `missing-boundary` is distinguishable from complete failure to understand the claim.
- [x] fluent written expression cannot turn invalid support into positive reasoning evidence.
- [x] a causal upstream failure may make this Ability `not_observed` rather than negative.

## B2 relation integrity

- [x] S1 `CN-S-evidence-explanation-conclusion` is `strategy_for` the Ability.
- [x] S2 `CN-S-compare-dimension-evidence-significance` is `strategy_for` the Ability where competing claims/interpretations are involved.
- [x] S5 `CN-S-claim-reason-evidence-warrant-boundary` is `strategy_for` the Ability.
- [x] none of S1/S2/S5 is stored as a hard prerequisite.
- [x] no global evidence-location/selection hard prerequisite is invented when candidate evidence can be supplied directly.
- [x] Question-specific decoding/knowledge prerequisites remain local when they are not necessary for the whole target semantics.

## Cross-domain coverage

- [x] authentic-demand mapping includes classical interpretation.
- [x] authentic-demand mapping includes modern/information reading.
- [x] authentic-demand mapping includes theory/case evaluation.
- [x] writing compatibility is demonstrated without claiming a synthetic fixture is an authentic exam question.
- [x] the same node identity is reused across domains rather than duplicated as `文言证据充分性` / `现代文证据充分性` / `作文证据充分性`.

## C2/C3 compatibility

- [x] C2 can record `mixed` independent evidence when the learner finds relevant but insufficient support.
- [x] C2 can preserve written-expression success while reasoning on sufficiency is negative.
- [x] H3 repair produces guided/partial evidence rather than fake H0 mastery.
- [x] diverse independent contexts can support M2 eligibility.
- [x] repeated same-variant items cannot fabricate M3 transfer.
- [x] established M2 plus meaningfully different unfamiliar transfer can support M3 eligibility without requiring C4 complexity.
- [x] automation remains independently assessed from mastery.

## Boundary with adjacent gaps

- [x] #30 rule application remains separately diagnosable from evidence sufficiency.
- [x] #34 TaskType identity is not created inside this Ability PR.
- [x] #35 Knowledge population is not silently folded into the Ability.
- [x] #36 authentic complexity ladder remains a separate material-supply task.

## MVP release gate

Issue #31 is considered semantically closed when:

```text
canonical node
+ B2 strategy relations
+ >=2 authentic cross-domain demand mappings
+ writing compatibility example
+ C2/C3 fixtures
+ semantic QA
```

are merged to `main`.

Full authentic material ladders are explicitly not required for this closure; that remains #36.
