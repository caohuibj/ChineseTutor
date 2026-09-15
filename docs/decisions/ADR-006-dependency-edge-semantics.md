# ADR-006 — Dependency-edge semantics

Status: **proposed in B2; first semantic self-review complete**

## Context

B1 defines stable Knowledge / Ability / Strategy nodes but not how they depend on or transfer to one another. The system needs explicit typed relations so tutoring can distinguish:

- a target deficit from an upstream prerequisite deficit;
- hard prerequisites from helpful support;
- reusable strategies from semantic prerequisites;
- same-operation generalization from reading→writing transfer;
- stable concept confusions from ordinary topical relationships.

Without typed edges, recommendation would still depend on manual intuition or grade order.

## Decision

ChineseTutor uses six canonical graph relations:

```text
requires
supports
part_of
strategy_for
transfers_to
contrasts_with
```

### Direction conventions

Every directed relation reads naturally from `FROM` to `TO`:

```text
DEPENDENT requires PREREQUISITE
SUPPORT_NODE supports TARGET
CHILD part_of PARENT
STRATEGY strategy_for ABILITY
SOURCE transfers_to TARGET
A contrasts_with B   # symmetric canonical pair
```

## Hard prerequisite policy

`requires` is intentionally sparse. It is valid only when the prerequisite is semantically necessary for independent performance of the **entire** dependent node.

A relation that is merely helpful, common in teaching order, or needed only for one Task Type must not be a hard prerequisite.

The active `requires` subgraph must be acyclic.

## Soft support policy

`supports` is always supporter→target and never blocks target readiness. It is used when the source materially improves target learning/performance without being semantically necessary.

## Taxonomy source of truth

B1 `parent_node_id` remains authoritative taxonomy. `part_of` is a derived graph projection rather than an independently authored relation, avoiding drift between two sources of truth.

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

B2 therefore restricts `transfers_to` to:

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

Edges carry rationale, source basis and evidence level (`hypothesis`, `supported`, `validated`). This expresses confidence in the graph relation, not learner state.

## Consequences

### Positive

- recommendation can remediate upstream causes instead of repeating downstream tasks;
- hard prerequisite blocking remains explainable;
- soft support remains useful without creating false gates;
- cross-domain transfer becomes explicit;
- Strategy selection can be individualized later;
- graph centrality/unlock value can be computed rather than authored;
- grade order is unnecessary for progression.

### Costs

- edge authoring requires strong semantic discipline;
- overuse of `requires` can create artificial ladders and must be actively prevented;
- task-specific conditional prerequisites still need Question/Task mappings later;
- readiness thresholds cannot be finalized until Learner Profile is defined.

## Rejected alternatives

### One generic `related_to` relation
Rejected because it cannot drive readiness, remediation, transfer or scaffold decisions.

### Make instructional sequence a hard prerequisite
Rejected because teaching order is not semantic necessity and would recreate grade-first progression.

### Store support as `TARGET supports SUPPORTER`
Rejected during self-review because the verb becomes directionally counterintuitive. Canonical storage now uses `SUPPORTER supports TARGET`.

### Permit cross-type `transfers_to`
Rejected in B2 because Strategy→Ability is better represented by `strategy_for`, and Knowledge→Ability by `supports`/`requires`. Keeping transfer same-type improves interpretability.

### Store transitive closure as explicit edges
Rejected because it creates redundancy and maintenance ambiguity. Traversal should derive indirect prerequisites.

### Let `part_of` duplicate `parent_node_id`
Rejected because two editable taxonomy sources would drift.

### Automatically propagate mastery through `transfers_to`
Rejected. Transfer is a hypothesis to test with authentic target evidence, not a substitute for target evidence.
