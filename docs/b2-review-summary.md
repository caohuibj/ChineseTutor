# B2 review summary

Issue: #2 — Define prerequisite/dependency graph semantics

## Implemented

- canonical persisted `LearningEdge` schema;
- five authored relation types: `requires`, `supports`, `strategy_for`, `transfers_to`, `contrasts_with`;
- sixth graph relation `part_of` as a virtual projection from B1 `parent_node_id`;
- natural direction convention for every relation;
- hard-prerequisite necessity test and whole-target applicability rule;
- endpoint-type constraints;
- acyclic hard-prerequisite policy;
- Strategy-vs-prerequisite distinction;
- same-operation generalization vs cross-operation transfer rule;
- edge evidence/confidence model;
- current-asset seed edges;
- downstream recommendation semantics without learner-state leakage;
- QA gates and ADR-006.

## Semantic self-review findings

Two review passes exposed six issues and corrected them.

### 1. `supports` direction was linguistically reversed — fixed

Canonical direction is now:

```text
SUPPORT_NODE supports TARGET
```

### 2. Cross-type `transfers_to` was ambiguous — fixed

B2 restricts transfer to:

```text
Ability -> Ability
Strategy -> Strategy
```

Cross-type reuse uses `strategy_for`, `supports`, or shared-node generalization.

### 3. Generic `supports` could swallow specialized relations — fixed

Relation selection now prefers taxonomy/same identity, hard prerequisite, `strategy_for`, same-type `transfers_to`, then generic `supports`, then `contrasts_with`.

### 4. Seed-edge confidence was overstated — fixed

An evidence→character Strategy relation initially marked `validated` was downgraded to `supported` pending repeated graph/learner evidence.

### 5. Derived taxonomy needs cycle safety — fixed

Virtual `part_of` inherits B1 parent constraints and must remain acyclic.

### 6. `part_of` was still modeled like a persisted edge — fixed

The first reviewed schema correctly said `part_of` was derived but still listed it in the persisted LearningEdge union, forcing unnecessary authored fields such as rationale/evidence/source_basis and leaving room for duplicate storage.

Final B2 contract:

```text
Persisted LearningEdge:
requires | supports | strategy_for | transfers_to | contrasts_with

Virtual graph relation:
part_of := projection of LearningNode.parent_node_id
```

No stored `part_of` row is valid.

## Acceptance judgment

**PASS for B2 semantic architecture.**

The remaining questions require learner-state/attempt evidence rather than more edge-schema design:

- what learner state makes a hard prerequisite “ready”;
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
