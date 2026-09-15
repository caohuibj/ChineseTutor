# ADR-017 — Evidence sufficiency is a canonical cross-domain Ability

Status: accepted in Issue #31 gap-closure PR

## Context

I1 found recurring authentic demands in which the learner must do more than locate evidence or explain one evidence→conclusion link. The learner must judge whether the **available evidence set is relevant, collectively sufficient and appropriately bounded for a claim**.

Examples include:

- adjudicating between competing classical-text interpretations with multiple pieces of evidence;
- stating a rule from multi-text material without overgeneralizing beyond the evidence;
- applying a supplied theory to a concrete language case and judging whether the cited features actually justify the evaluation;
- deciding whether claims are reasonable and explaining whether the evidence is enough.

G1 already defines procedures such as S1 `证据—解释—结论`, S2 `比较维度—分别取证—同异—意义`, and S5 `观点—理由—证据—论证—边界`. Those are Strategies. They do not replace the observable learner outcome.

## Decision

Add one canonical Ability:

```text
CN-A-evaluate-evidence-sufficiency-for-claim
评估证据对主张的支持是否充分
```

Primary domain: `META`  
Subdomain: `META.evidence`

The Ability observes whether the learner can:

1. distinguish relevant evidence from irrelevant evidence;
2. explain the warrant linking evidence to the claim;
3. judge whether the evidence set covers the important parts/conditions of the claim;
4. detect when a conclusion is stronger than its evidence;
5. add an appropriate qualification/boundary or identify what further evidence is needed;
6. compare competing claims/interpretations by support quality rather than evidence count alone.

## Why one Ability rather than several new nodes now

Relevance, warrant fit, sufficiency and boundary control are diagnostically distinguishable facets, but current authentic evidence does not yet justify four separate Profile nodes.

For MVP, C2 Attempt evidence records partial patterns such as:

```text
relevant-but-insufficient
list-without-warrant
overclaim
missing-boundary
irrelevant-support
```

If real pilot data later shows stable, independently trainable failure families with different interventions/retention behavior, B1 split rules permit decomposition without changing this decision's conceptual boundary.

## Why this is not `identify evidence`

A task can directly provide candidate evidence and still test this Ability. A learner may select a fact that is genuinely relevant yet still fail because it supports only one part of a broad claim.

Therefore:

```text
found relevant evidence
!=
evidence is sufficient for the claim
```

## Why this is not a mother Strategy

S1/S2/S5 are reusable procedures that can be prompted, faded and automated. The new Ability is the **outcome being evaluated**.

Persist:

```text
Strategy strategy_for Ability
```

Do not persist:

```text
Ability requires Strategy
```

A learner may reach a valid sufficiency judgment through another procedure.

## Why no global hard prerequisite is added

B2 requires hard prerequisites to apply to the whole target semantics. This Ability can be tested with candidate claims/evidence already supplied, so broad text-location or evidence-selection abilities are not globally necessary.

Domain-specific decoding, source knowledge, rule application and evidence selection remain Question-level or narrower-node prerequisites when the concrete task demands them.

## Boundary with rule application (#30)

A theory-to-case task can fail in two different places:

```text
A. learner does not map the rule correctly to the case
   -> rule-application gap (#30)

B. learner maps the rule but accepts weak/partial support as sufficient
   -> evidence-sufficiency Ability
```

C2 must be able to mark one as `not_observed` when the other causally blocks it.

## Boundary with argumentative writing

The Ability applies inside writing when the learner evaluates whether chosen reasons/examples actually support a claim. It does not replace:

- argument structure Knowledge;
- claim generation;
- material selection as a production Ability;
- paragraph organization;
- full-essay construction.

## Boundary with “critical thinking”

The node is deliberately narrower than a broad critical-thinking label. The observable object is always:

```text
evidence/reasons -> explicit claim/interpretation/conclusion
```

The node does not aggregate all evaluation, creativity, source criticism, logical reasoning or decision-making into one score.

## Consequences

- Profile can distinguish “finds evidence” from “judges evidence adequacy”.
- F2 can diagnose a learner who has correct evidence but overclaims without reteaching reading location.
- F1 can target evidence evaluation directly across modern reading, classical interpretation and writing.
- C3 can require diverse independent evidence before M2/M3 claims.
- #36 can later build authentic complexity ladders for this node.
- #30 remains independently necessary for induction/application semantics.

## Rejected alternatives

### Keep it inside S5 only
Rejected because Strategy execution and Ability success are different Profile claims.

### Use generic `critical thinking`
Rejected because it is too broad to diagnose, scaffold or verify reliably.

### Treat evidence count as sufficiency
Rejected because several weak/redundant pieces of evidence may still fail to cover a claim.

### Make evidence selection a universal `requires`
Rejected because some authentic tasks provide candidate evidence directly; this fails B2's whole-target necessity test.

### Split relevance/warrant/sufficiency/boundary into four nodes immediately
Deferred. Pilot evidence should first establish whether separate learner-state tracking creates enough diagnostic value to justify the extra graph complexity.
