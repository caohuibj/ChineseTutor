# ADR-006 — Dependency-edge semantics

Status: **proposed in B2**

## Context

B1 defines stable Knowledge / Ability / Strategy nodes but does not state how they depend on or transfer to one another. The system needs explicit relations so tutoring can distinguish:

- a target deficit from an upstream prerequisite deficit;
- hard prerequisites from merely helpful support;
- reusable strategies from semantic prerequisites;
- same-operation generalization from reading→writing transfer;
- stable concept confusions from ordinary topical relationships.

Without typed edges, recommendation would still depend on manual intuition or grade order.

## Decision

ChineseTutor will use six canonical graph relations:

```text
requires
supports
part_of
strategy_for
transfers_to
contrasts_with
```

### Direction conventions

```text
TARGET requires PREREQUISITE
TARGET supports SUPPORT_NODE
CHILD part_of PARENT
STRATEGY strategy_for ABILITY
SOURCE transfers_to TARGET
A contrasts_with B   # symmetric canonical pair
```

## Hard prerequisite policy

`requires` is intentionally sparse. It is valid only when the prerequisite is semantically necessary for independent performance of the entire target node.

A relation that is merely helpful, common in teaching order, or only needed for one Task Type must not be encoded as a hard prerequisite.

The active `requires` subgraph must be acyclic.

## Taxonomy source of truth

B1 `parent_node_id` remains the authoritative taxonomic hierarchy. `part_of` is a derived graph projection rather than an independently authored relation.

This avoids parent/edge divergence.

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

If they are **different operations that reuse structure**, keep separate nodes and connect them with `transfers_to`.

Example:

```text
modern character judgment + classical character judgment
=> one shared evidence-to-character Ability

analyze typical event value -> select writing material
=> separate Abilities + transfers_to
```

## Edge evidence

Edges carry rationale, source basis and evidence level (`hypothesis`, `supported`, `validated`). This expresses confidence in the graph relation, not learner state.

## Consequences

### Positive

- recommendation can remediate upstream causes rather than repeat downstream tasks;
- hard prerequisite blocking remains explainable;
- cross-domain transfer becomes explicit;
- Strategy selection can be individualized later;
- graph centrality/unlock value can be computed rather than hand-authored;
- grade order is unnecessary for progression.

### Costs

- edge authoring requires stronger semantic discipline;
- overuse of `requires` can create artificial ladders and must be actively prevented;
- conditional task-specific prerequisites still need Question/Task mappings later;
- readiness thresholds cannot be finalized until Learner Profile is defined.

## Rejected alternatives

### One generic `related_to` relation
Rejected because it cannot drive readiness, remediation, transfer or scaffold decisions.

### Make all instructional sequencing a hard prerequisite
Rejected because teaching order is not semantic necessity and would recreate grade-first progression in another form.

### Store transitive closure as explicit edges
Rejected because it creates redundancy and makes maintenance/error diagnosis harder. Traversal should derive indirect prerequisites.

### Let `part_of` duplicate `parent_node_id`
Rejected because two editable sources of taxonomy would drift.

### Automatically propagate mastery through `transfers_to`
Rejected. Transfer is a hypothesis to test with authentic target evidence, not a substitute for target evidence.
