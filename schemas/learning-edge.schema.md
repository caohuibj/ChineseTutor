# LearningEdge canonical schema

Status: **B2 normative draft**

This document defines relationships between canonical `LearningNode` records. It builds on B1 and deliberately excludes learner-specific state.

## 1. Core rule

A `LearningEdge` states a stable semantic relationship between two canonical nodes. It answers questions such as:

- what is genuinely required before another operation is independently viable?
- what knowledge/ability materially supports another node without blocking it?
- which reusable Strategy coordinates an Ability?
- where does an operation naturally transfer?
- which concepts/operations are easy to confuse and should be contrasted?

Edges do **not** store one learner's readiness, mastery, recent performance or recommendation priority.

---

## 2. Relation types

Canonical relation types:

```text
requires
supports
part_of
strategy_for
transfers_to
contrasts_with
```

### `requires`

Read as:

```text
FROM requires TO
```

`FROM` is the dependent node; `TO` is a hard prerequisite.

Example:

```text
CN-A-translate-classical-sentence-accurately
  requires
CN-A-infer-classical-contextual-word-sense
```

Use only when independent performance of the entire `FROM` semantics is not reasonably viable without `TO`.

### `supports`

Read as:

```text
FROM is materially supported by TO
```

`TO` improves success, lowers learning cost, or strengthens quality, but missing it does not make the entire target semantically unready.

Example:

```text
CN-A-link-poetic-image-to-emotion
  supports
CN-A-reconstruct-poetic-scene
```

### `part_of`

Structural decomposition only. In B2 this relation is **derived from B1 `parent_node_id`** and is not independently authored.

Direction:

```text
CHILD part_of PARENT
```

This prevents two competing sources of truth for taxonomy.

### `strategy_for`

Direction:

```text
STRATEGY strategy_for ABILITY
```

The source must be a Strategy node; the target must be an Ability node.

TaskType→Strategy mappings are stored in the Task graph, not in `LearningEdge`.

### `transfers_to`

Direction:

```text
SOURCE transfers_to TARGET
```

Mastery/automation of `SOURCE` is expected to reduce learning cost or improve performance on `TARGET`, but `SOURCE` is not a hard prerequisite.

Example:

```text
analyze-typical-event-value
  transfers_to
select-typical-writing-material
```

### `contrasts_with`

Symmetric confusion/distinction relation. Store one canonical pair only.

Example:

```text
memoir-genre-features contrasts_with biography-genre-features
```

No direction, transitivity or prerequisite meaning is inferred.

---

## 3. Canonical fields

```yaml
edge_id: string
from_node_id: string
relation_type: requires | supports | part_of | strategy_for | transfers_to | contrasts_with
to_node_id: string
rationale: string
scope_note: string | null
evidence_level: hypothesis | supported | validated
source_basis: reference[]
status: draft | active | deprecated
```

Optional:

```yaml
strength: weak | medium | strong | null
notes: string | null
deprecated_by: string | null
```

### `edge_id`

Machine identity should be deterministic from the semantic tuple where practical:

```text
LE-<RELATION>-<FROM_SLUG>--<TO_SLUG>
```

For `contrasts_with`, order endpoints lexicographically so the symmetric pair has one identity.

`edge_id` is storage identity; the authoritative semantic uniqueness rule is the tuple:

```text
(from_node_id, relation_type, to_node_id)
```

except symmetric canonicalization for `contrasts_with`.

### `rationale`

Why this relation exists. It must explain the semantic dependency/transfer, not merely say “related”.

### `scope_note`

Use only when useful to clarify the semantic boundary. A hard `requires` edge must apply to the entire target semantics; if it only applies in one occasional context, refine the target node or use `supports` instead.

### `evidence_level`

- `hypothesis` — plausible architecture proposal not yet supported by repeated operational/exam evidence;
- `supported` — supported by curriculum structure, repeated tutoring evidence, authentic tasks or established instructional logic;
- `validated` — repeatedly confirmed by mapping/learner evidence and stable enough for recommendation logic.

This describes confidence in the **graph relation**, not learner performance.

### `strength`

Allowed primarily for `supports` and `transfers_to`. It is a coarse semantic strength, not a personal score.

---

## 4. Endpoint type constraints

| relation | allowed source | allowed target | notes |
| --- | --- | --- | --- |
| requires | Knowledge / Ability / Strategy | Knowledge / Ability | a Strategy may require component abilities; a Strategy should not be a hard prerequisite target |
| supports | Knowledge / Ability / Strategy | Knowledge / Ability / Strategy | broad soft support; rationale required |
| part_of | same type as parent | same type as child | derived from B1 parent hierarchy; normally same primary domain |
| strategy_for | Strategy only | Ability only | TaskType mapping lives elsewhere |
| transfers_to | Ability or Strategy | same semantic type preferred | directed; do not infer equivalence |
| contrasts_with | same node type | same node type | symmetric canonical pair |

`requires(A, Strategy)` is rejected by default: a specific procedure is normally an instructional route rather than a semantic prerequisite for performing an Ability. If future evidence proves otherwise, the Ability/Strategy definitions should first be reviewed.

---

## 5. Hard prerequisite test

Before authoring `FROM requires TO`, all must be true:

1. `TO` is semantically needed for independent performance of the whole `FROM` node, not just one Task Type.
2. Failure on `TO` plausibly causes failure on `FROM`.
3. The relationship is not merely “usually helpful”.
4. The relation does not depend on grade/year.
5. The target node is not too broad; if the prerequisite is only conditionally needed, refine the target or use `supports`.

Examples:

```text
translate classical sentence
requires contextual classical word-sense inference
```

Pass.

```text
infer poem emotion
requires knowing every conventional image association
```

Fail; local textual reasoning can succeed without exhaustive convention knowledge. Use `supports` for relevant image knowledge.

---

## 6. Graph integrity rules

### 6.1 No self edges

Reject `A relation A` for every relation type.

### 6.2 `requires` must be acyclic

The active `requires` subgraph must be a DAG.

Any new active hard prerequisite must pass cycle detection before merge/activation.

Rationale: a hard cycle would mean no node in the cycle can become ready without already being ready.

### 6.3 Do not persist transitive closure

If:

```text
A requires B
B requires C
```

do not automatically store `A requires C` unless C is also a direct semantic prerequisite whose explicit edge improves diagnosis/recommendation.

Traversal computes transitive dependencies.

### 6.4 Soft cycles are allowed but non-blocking

`supports` and `transfers_to` may form cycles when justified. They never create readiness deadlocks.

### 6.5 Symmetric relation canonicalization

For `contrasts_with` store exactly one pair; `(A,B)` and `(B,A)` are duplicates.

### 6.6 `part_of` has one source of truth

Do not hand-author `part_of` edges. Generate them from `LearningNode.parent_node_id`.

### 6.7 No implicit inverse edges

The system may expose readable inverses (`required_by`, `supported_by`) in views, but only the canonical authored direction is stored.

---

## 7. Recommendation semantics

B2 defines semantics, not the final recommendation formula.

Expected downstream behavior:

- missing `requires` prerequisite → target may be blocked or recommendation may switch to remediation;
- missing `supports` node → target remains trainable, possibly with more scaffold;
- `strategy_for` → candidate Strategy for hinting/scaffold selection;
- `transfers_to` → source success may increase confidence/readiness or justify scaffold reduction on target;
- `contrasts_with` → useful for confusion diagnosis and discriminative practice;
- `part_of` → reporting/navigation only, not readiness by itself.

Readiness thresholds are learner-profile logic and belong to C1/F1, not canonical edge identity.

---

## 8. Edge authoring discipline

Prefer the weakest relation that accurately captures reality.

```text
requires > supports
```

Do not use `requires` merely to make the graph look orderly. Hard prerequisites should remain sparse and defensible.

A new edge should change at least one downstream behavior:

- readiness/blocking;
- remediation choice;
- scaffold choice;
- transfer candidate selection;
- confusion diagnosis;
- reporting/navigation.

If it changes nothing, do not add it.

---

## 9. Forbidden fields

Do not store learner-specific or dynamically computed values in canonical edges:

```text
learner_mastery
current_readiness
student_priority
last_failed
personal_strength
attempt_count
next_review_date
recommendation_score
unlock_centrality
```

These belong to Profile/Attempt/Recommendation analytics.

---

## 10. B2 invariants

1. Every active edge has valid canonical endpoints.
2. Every edge has one defined relation type.
3. `requires` is sparse, global-to-target semantics, and acyclic.
4. `part_of` is derived from B1 parent hierarchy only.
5. `strategy_for` is Strategy→Ability.
6. `contrasts_with` is same-type, symmetric and deduplicated.
7. `transfers_to` never implies equivalence or prerequisite.
8. learner state never appears in canonical edge records.
9. grade/year never creates prerequisite semantics.
10. relation rationale and source basis are reviewable.
