# ADR-018 — Separate rule induction from rule application

Status: Accepted for v2 MVP minimum slice

## Context

I1 found recurring tasks that require either:

1. deriving a usable rule/principle from examples or materials; or
2. applying an already usable rule/principle to a concrete case.

These operations can fail independently. Treating both as generic “information integration”, “transfer”, or one opaque higher-order skill would hide the actionable failure.

## Decision

Create two canonical cross-domain Ability nodes:

- `CN-A-induce-rule-from-examples` — derive a rule/principle from instances/material relations while preserving important conditions and boundaries;
- `CN-A-apply-rule-to-case` — map an already available rule/principle onto a concrete case and adjust the conclusion when conditions differ.

Both live in `META.rule` because the observable operations recur across content domains.

## Diagnostic split

### Induction failure

The learner can find facts but cannot form a stable relation/condition that explains them.

### Application failure

The rule is already supplied or correctly derived, but the learner mismatches its conditions to the new case, ignores a changed condition, or mechanically copies the rule.

A derive-then-apply item therefore does not justify copying final correctness to both nodes.

```text
bad induction
→ application may be not_observed

correct induction + bad case mapping
→ induction positive
→ application negative
```

When a task supplies the rule directly:

```text
induction = not_observed
application = observable
```

## Why application does not `requires` induction

Many authentic tasks provide the theory/rule explicitly. A learner can therefore demonstrate rule application without deriving the rule in that task. `CN-A-induce-rule-from-examples supports CN-A-apply-rule-to-case` is appropriate only as a soft, scoped relation for derive-then-apply situations.

## Strategy relations

Existing mother Strategies remain advisory procedures:

- S4 `定位—分类—合并—压缩` → induction;
- S13 `关系链` → induction and application;
- S10 `题目限制—核心任务—候选答案—逐项校验` → application.

They are `strategy_for`, not hard prerequisites. No new Strategy is added.

## Boundary with evidence sufficiency (#31)

These Abilities answer different questions:

- induction: **what rule follows from the examples/materials?**
- application: **how does this available rule map to this case?**
- evidence sufficiency: **is the support enough for the claim/conclusion being asserted?**

A learner may induce a plausible rule but overstate its boundary; the induction and evidence-sufficiency nodes can therefore receive different observations.

## Boundary with multi-constraint synthesis (#33)

Rule application may produce one local judgment/action. Constructing a complete plan that jointly satisfies several independent constraints remains a separate candidate Ability under #33.

## Transfer and complexity

“Novel-case” is not encoded in the node identity. Familiarity, variant diversity and transfer-probe status remain C2 Attempt context. A routine C2 unfamiliar application can be transfer evidence; a familiar C4 application is not automatically transfer.

## MVP scope

This ADR intentionally stops before:

- adding new TaskType identities (#34);
- completing the Knowledge population (#35);
- building full authentic complexity ladders (#36);
- resolving multi-constraint solution synthesis (#33).

The purpose is to close the semantic minimum slice required by the v2 MVP release contract.
