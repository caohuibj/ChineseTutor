# Issue #31 semantic self-review — evidence sufficiency / claim support

Verdict: **PASS for pre-launch semantic closure**

This review checks the gap closure against B1/B2/C1/C2/C3/G1/I1 and the v2 MVP release contract.

## Review findings and resolutions

### 1. Risk: collapsing “find evidence” and “judge sufficiency”

**Finding:** A learner may select a genuinely relevant fact yet still overclaim beyond what it supports.

**Resolution:** The canonical definition requires relevance + warrant + collective coverage + boundary control. C2 fixture `ES-C2-01` explicitly records `evidence_selection=2` while the new Ability remains `mixed`.

### 2. Risk: recreating generic “critical thinking”

**Finding:** `claim validity` could become an unbounded label for all evaluation/reasoning.

**Resolution:** Scope is restricted to the support relation:

```text
evidence/reasons -> explicit claim/interpretation/conclusion
```

Source credibility, creativity, general decision quality and formal logic remain outside unless represented by separate nodes.

### 3. Risk: confusing Ability with S5 Strategy

**Finding:** G1 already contains `观点—理由—证据—论证—边界`.

**Resolution:** S5 remains a promptable/fadeable procedure; the new node is the learner outcome. S1/S2/S5 therefore use `strategy_for`, never `requires`.

### 4. Risk: overusing hard prerequisites

**Finding:** Evidence location/selection often precedes sufficiency evaluation, but some tasks directly provide candidate evidence.

**Resolution:** No global `requires` edge is added. Concrete Questions may declare local prerequisites when decoding/selection is actually necessary.

### 5. Risk: hiding the #30 rule-application gap

**Finding:** Theory-to-case evaluation can fail before sufficiency is observable because the learner mapped the rule incorrectly.

**Resolution:** Fixtures retain `GAP-RULE-APPLICATION` as a coexisting gap. C2 may mark the sufficiency node `not_observed` when rule application causally blocks it.

### 6. Risk: splitting too early into relevance / warrant / sufficiency / boundary nodes

**Finding:** These facets are diagnostically meaningful, but the current corpus does not yet prove separate Profile nodes are worth the graph cost.

**Resolution:** Keep one canonical Ability for MVP and record facet-level partial patterns in Attempt evidence. Revisit decomposition only if pilot data shows stable independent error/intervention/retention behavior.

### 7. Risk: writing compatibility becoming a fake authentic source

**Finding:** Issue #31 requires cross-domain use including argumentative writing, but current I1 extraction does not provide a single exact writing subquestion proving the node.

**Resolution:** The writing fixture is explicitly `synthetic-cross-domain-compatibility`; authentic provenance is claimed only for the audited Beijing/National demand fixtures.

### 8. Risk: M3 becoming another complexity label

**Finding:** Cross-domain sufficiency evaluation is a natural transfer target and could regress to `M3=C4`.

**Resolution:** C3 fixtures state that M3 eligibility comes from established M2 plus meaningfully different unfamiliar independent evidence; C-band is evaluated separately.

## Acceptance check

Issue #31 requirements:

- [x] success semantics are independent of one exam label;
- [x] evidence identification is separated from sufficiency/validity evaluation;
- [x] relevant-but-insufficient evidence is representable;
- [x] S1/S2/S5 are recommended Strategies, not hard prerequisites;
- [x] modern/information reading mapping exists;
- [x] classical interpretation mapping exists;
- [x] argumentative-writing compatibility exists;
- [x] node is narrower than generic critical thinking;
- [x] C1/C2/C3 compatibility is represented;
- [x] no grade-first progression rule is introduced;
- [x] no new architecture entity/axis is introduced.

## Remaining non-blocking work

- #30 rule induction/application semantics;
- #34 permanent TaskType inventory for any pilot tasks that require new environments;
- #35 supporting Knowledge population;
- #36 authentic complexity ladder for the new Ability.

Those are separate MVP slices/backlog items and should not keep Issue #31 semantically open after this PR merges.
