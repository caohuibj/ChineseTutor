# ADR-006 — Dependency-edge semantics

Status: **proposed in B2; two semantic review passes complete**

## Context

B1 defines stable Knowledge / Ability / Strategy nodes but not how they depend on or transfer to one another. The system needs explicit typed relations so tutoring can distinguish:

- a target deficit from an upstream prerequisite deficit;
- hard prerequisites from helpful support;
- reusable strategies from semantic prerequisites;
- same-operation generalization from reading→writing transfer;
- stable concept confusions from ordinary topical relationships.

Without typed edges, recommendation would still depend on manual intuition or grade order.

## Decision

ChineseTutor exposes six graph relation semantics:

```text
requires
supports
part_of
strategy_for
transfers_to
contrasts_with
```

However, only five are persisted as authored `LearningEdge` records:

```text
requires
supports
strategy_for
transfers_to
contrasts_with
```

`part_of` is a **virtual relation** derived exclusively from B1 `LearningNode.parent_node_id`.

### Direction conventions

```text
DEPENDENT requires PREREQUISITE
SUPPORT_NODE supports TARGET
CHILD part_of PARENT          # virtual projection only
STRATEGY strategy_for ABILITY
SOURCE transfers_to TARGET
A contrasts_with B            # symmetric canonical pair
```

## Hard prerequisite policy

`requires` is intentionally sparse. It is valid only when the prerequisite is semantically necessary for independent performance of the **entire** dependent node.

A relation that is merely helpful, common in teaching order, or needed only for one Task Type must not be a hard prerequisite.

The active `requires` subgraph must be acyclic.

## Soft support policy

`supports` is always supporter→target and never blocks target readiness. It is used when the source materially improves target learning/performance without being semantically necessary.

## Taxonomy source of truth

B1 `parent_node_id` remains the sole authoritative taxonomy representation. Graph readers may project `child part_of parent`, but `part_of` is never independently stored as `LearningEdge`, never receives separate rationale/evidence/status, and cannot drift from the B1 parent field.

## Strategy policy

A Strategy is normally an instructional/procedural route, not a hard prerequisite for an Ability.

Use:

```text
Strategy strategy_for Ability
```

rather than:

```text
Ability requires Strategy
```

A Strategy may itself require component Knowledge/Abilities needed to execute it.

## Generalization vs transfer

If two contexts use the **same observable operation**, create one shared LearningNode.

If they are **different operations of the same semantic type that reuse structure**, keep separate nodes and connect them with `transfers_to`.

B2 restricts `transfers_to` to:

```text
Ability -> Ability
Strategy -> Strategy
```

Cross-type reuse should use `strategy_for` or `supports`.

Example:

```text
modern character judgment + classical character judgment
=> one shared evidence-to-character Ability

analyze typical-event value -> select writing material
=> separate Abilities + transfers_to
```

## Edge evidence

Persisted authored edges carry rationale, source basis and evidence level (`hypothesis`, `supported`, `validated`). This expresses confidence in the graph relation, not learner state.

Virtual `part_of` inherits its validity from B1 parent integrity and carries no independent evidence record.

## Consequences

### Positive

- recommendation can remediate upstream causes instead of repeating downstream tasks;
- hard prerequisite blocking remains explainable;
- soft support remains useful without creating false gates;
- cross-domain transfer becomes explicit;
- Strategy selection can be individualized later;
- graph centrality/unlock value can be computed rather than authored;
- taxonomy has one storage source of truth;
- grade order is unnecessary for progression.

### Costs

- edge authoring requires strong semantic discipline;
- overuse of `requires` can create artificial ladders and must be actively prevented;
- task-specific conditional prerequisites still need Question/Task mappings later;
- readiness thresholds cannot be finalized until Learner Profile is defined;
- graph consumers must combine persisted edges with virtual taxonomy projection at read time.

## Rejected alternatives

### One generic `related_to` relation
Rejected because it cannot drive readiness, remediation, transfer or scaffold decisions.

### Make instructional sequence a hard prerequisite
Rejected because teaching order is not semantic necessity and would recreate grade-first progression.

### Store support as `TARGET supports SUPPORTER`
Rejected during self-review because the verb becomes directionally counterintuitive. Canonical storage uses `SUPPORTER supports TARGET`.

### Permit cross-type `transfers_to`
Rejected because Strategy→Ability is better represented by `strategy_for`, and Knowledge→Ability by `supports`/`requires`. Keeping transfer same-type improves interpretability.

### Persist `part_of` as an ordinary LearningEdge
Rejected during second review. Because taxonomy is already canonical in `parent_node_id`, a stored `part_of` edge would require duplicate metadata and could diverge. It is now virtual-only.

### Store transitive closure as explicit edges
Rejected because it creates redundancy and maintenance ambiguity. Traversal should derive indirect prerequisites.

### Automatically propagate mastery through `transfers_to`
Rejected. Transfer is a hypothesis to test with authentic target evidence, not a substitute for target evidence.
