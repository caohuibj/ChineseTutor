# B2 review summary

Issue: #2 — Define prerequisite/dependency graph semantics

## Implemented

- canonical `LearningEdge` schema;
- six relation types: `requires`, `supports`, `part_of`, `strategy_for`, `transfers_to`, `contrasts_with`;
- natural direction convention for every relation;
- hard-prerequisite necessity test and whole-target applicability rule;
- endpoint-type constraints;
- acyclic hard-prerequisite policy;
- derived-only `part_of` policy tied to B1 `parent_node_id`;
- Strategy-vs-prerequisite distinction;
- same-operation generalization vs cross-operation transfer rule;
- edge evidence/confidence model;
- current-asset seed edges;
- downstream recommendation semantics without learner-state leakage;
- QA gates and ADR-006.

## Semantic self-review findings

The first B2 draft exposed five issues worth correcting before opening the PR.

### 1. `supports` direction was linguistically reversed — fixed

The first draft encoded:

```text
TARGET supports SUPPORT_NODE
```

while the prose meant “SUPPORT_NODE supports TARGET”. This would be error-prone in traversal and authoring.

Canonical direction is now:

```text
SUPPORT_NODE supports TARGET
```

and seed data/QA/ADR are aligned.

### 2. Cross-type `transfers_to` was ambiguous — fixed

Allowing Strategy→Ability transfer overlaps with `strategy_for`, while Knowledge→Ability overlaps with `supports/requires`.

B2 now restricts transfer to:

```text
Ability -> Ability
Strategy -> Strategy
```

If two operations are actually semantically identical, they should be one generalized node rather than a transfer edge.

### 3. Generic `supports` could swallow specialized relations — fixed

The relation-choice decision tree now checks:

1. taxonomy / same identity;
2. hard prerequisite;
3. `strategy_for`;
4. same-type `transfers_to`;
5. generic `supports`;
6. `contrasts_with`.

This preserves more informative semantics.

### 4. Seed edge confidence was overstated — fixed

The evidence→character Strategy relation was initially marked `validated`, but current evidence is strong design/teaching support rather than repeated graph validation. It has been downgraded to `supported`.

### 5. Derived taxonomy also needs cycle safety — clarified

`part_of` is derived from B1 parent hierarchy and must remain acyclic, even though it does not participate in readiness blocking.

## Acceptance judgment

**PASS for B2 draft architecture.**

The remaining questions require later learner-state/attempt evidence rather than more edge-schema design:

- what mastery level makes a prerequisite “ready”;
- how recommendation scores use hard/soft/transfer edges;
- whether graph evidence levels can be upgraded from actual learner data;
- how task-specific conditional prerequisites attach to Questions.

These are intentionally deferred to C1/C2/F1.

## Deliberately deferred

- Learner Profile readiness thresholds — C1;
- TrainingAttempt evidence — C2;
- automatic recommendation weighting — F1;
- Task/Question-specific dependency mapping — Task graph PR;
- broad graph population and Gaokao coverage audit — I1.
