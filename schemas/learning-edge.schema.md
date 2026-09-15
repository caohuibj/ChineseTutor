# LearningEdge canonical schema

Status: **B2 normative draft — self-reviewed**

This document defines stable semantic relationships between canonical `LearningNode` records. It builds on B1 and excludes learner-specific state.

## 1. Core rule

A `LearningEdge` states a durable semantic relationship between two canonical nodes. It answers questions such as:

- what is genuinely required before another operation is independently viable?
- what knowledge/ability materially supports another node without blocking it?
- which reusable Strategy coordinates an Ability?
- where does an operation naturally transfer?
- which concepts/operations should be explicitly contrasted?

Edges do **not** store one learner's readiness, mastery, recent performance or recommendation priority.

---

## 2. Relation types and direction

Canonical relation types:

```text
requires
supports
part_of
strategy_for
transfers_to
contrasts_with
```

Every directed relation is written so `FROM relation TO` reads naturally.

### `requires`

```text
DEPENDENT requires PREREQUISITE
```

Independent performance of the dependent node is not reasonably viable without the prerequisite.

Example:

```text
CN-A-translate-classical-sentence-accurately
  requires
CN-A-infer-classical-contextual-word-sense
```

### `supports`

```text
SUPPORT_NODE supports TARGET
```

The support node materially improves target performance/learning efficiency, but its absence does not make the target semantically unavailable.

Example:

```text
CN-K-memoir-genre-features
  supports
CN-A-judge-modern-genre-from-evidence
```

### `part_of`

```text
CHILD part_of PARENT
```

Structural projection only. In B2 it is **derived from B1 `parent_node_id`** and is not independently authored.

### `strategy_for`

```text
STRATEGY strategy_for ABILITY
```

The source must be a Strategy node; the target must be an Ability node. TaskType→Strategy mappings belong to the Task graph.

### `transfers_to`

```text
SOURCE transfers_to TARGET
```

Mastery/automation of SOURCE is expected to reduce learning cost or improve performance on TARGET, but SOURCE is not a prerequisite and never substitutes for TARGET evidence.

B2 restricts this relation to:

```text
Ability -> Ability
Strategy -> Strategy
```

Cross-type reuse should normally use `strategy_for`, `supports`, or a shared generalized node instead of `transfers_to`.

### `contrasts_with`

Symmetric distinction/confusion relation. Store one canonical pair only; do not infer direction or transitivity.

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

Deterministic identity where practical:

```text
LE-<RELATION>-<FROM_SLUG>--<TO_SLUG>
```

For `contrasts_with`, order endpoints lexicographically so `(A,B)` and `(B,A)` resolve to one identity.

Semantic uniqueness is the tuple:

```text
(from_node_id, relation_type, to_node_id)
```

with symmetric canonicalization for `contrasts_with`.

### `rationale`

Explains why the relation changes diagnosis/training behavior; “they are related” is insufficient.

### `scope_note`

Clarifies semantic boundary where needed. A `requires` edge must apply to the whole dependent node. If only occasionally needed, refine the dependent node or use `supports`.

### `evidence_level`

- `hypothesis` — plausible graph proposal, not yet supported by repeated operational/exam evidence;
- `supported` — supported by curriculum/material structure, repeated tutoring evidence, authentic tasks, or stable instructional logic;
- `validated` — repeatedly confirmed and sufficiently stable for downstream recommendation logic.

This is confidence in the **graph relation**, not learner performance.

### `strength`

Allowed primarily for `supports` and `transfers_to`; coarse semantic strength only, never a learner score.

---

## 4. Endpoint type constraints

| relation | allowed FROM | allowed TO | notes |
| --- | --- | --- | --- |
| requires | Knowledge / Ability / Strategy | Knowledge / Ability | Strategy may require component Knowledge/Abilities; Strategy is not a hard-prerequisite target |
| supports | Knowledge / Ability / Strategy | Knowledge / Ability / Strategy | source is the support; target is the beneficiary |
| part_of | same type as parent | same type as child | derived from B1 hierarchy; direction child→parent |
| strategy_for | Strategy only | Ability only | TaskType mapping lives elsewhere |
| transfers_to | Ability or Strategy | same type as source | directed, non-equivalent, non-blocking |
| contrasts_with | same node type | same node type | symmetric canonical pair |

Reject by default:

```text
Ability requires Strategy
```

A specific Strategy is normally one valid route rather than a semantic prerequisite. If future evidence suggests otherwise, first review node definitions.

---

## 5. Hard prerequisite test

Before authoring `DEPENDENT requires PREREQUISITE`, all must be true:

1. prerequisite is semantically needed for independent performance of the **whole** dependent node;
2. lack of prerequisite plausibly causes dependent failure;
3. relation is stronger than “usually helpful”;
4. relation does not depend on grade/year or normal teaching order;
5. the dependent node is not so broad that the prerequisite only applies in some cases.

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

Fail. Relevant image knowledge may `support` the target but is not globally necessary.

---

## 6. Graph integrity rules

### 6.1 No self edges
Reject `A relation A` for all relation types.

### 6.2 `requires` is acyclic
The active hard-prerequisite subgraph must be a DAG. Every new active `requires` edge must pass cycle detection/topological validation.

### 6.3 Do not persist transitive closure

If:

```text
A requires B
B requires C
```

do not automatically store `A requires C`. Add it only if C is also a direct semantic prerequisite whose explicit edge improves diagnosis.

### 6.4 Soft cycles are allowed but non-blocking

`supports` may be reciprocal. `transfers_to` may form cycles only as separately justified directional edges. Neither creates readiness deadlocks.

### 6.5 Symmetric relation canonicalization

For `contrasts_with`, store exactly one pair; reverse duplicates are invalid.

### 6.6 `part_of` has one source of truth

Do not hand-author `part_of`; derive it from `LearningNode.parent_node_id`.

### 6.7 No implicit inverse records

Views may expose `required_by` / `supported_by`, but only canonical authored direction is stored.

---

## 7. Recommendation semantics

B2 defines relation meaning, not the final recommendation formula.

Expected downstream behavior:

- missing `requires` prerequisite → target may be blocked or recommendation may switch to remediation;
- missing supporter (`SUPPORT supports TARGET`) → target remains trainable, possibly with more scaffold;
- `strategy_for` → candidate procedure for hints/scaffold;
- `transfers_to` → source success may justify a transfer probe or lower initial scaffold, but never mastery propagation;
- `contrasts_with` → candidate discriminative/confusion practice;
- `part_of` → reporting/navigation only.

Learner readiness thresholds belong to C1/F1.

---

## 8. Edge authoring discipline

Prefer the weakest relation that accurately captures reality. Do not use `requires` to impose teaching order.

A new edge should change at least one downstream behavior:

- readiness/blocking;
- remediation choice;
- scaffold choice;
- transfer-probe selection;
- confusion diagnosis;
- reporting/navigation.

If it changes nothing, do not add it.

---

## 9. Forbidden fields

Do not store learner-specific or dynamically computed values:

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
2. Every edge has exactly one relation type and an unambiguous direction.
3. `requires` is sparse, whole-target, grade-independent and acyclic.
4. `supports` direction is supporter→target and never blocks readiness.
5. `part_of` is derived from B1 parent hierarchy only.
6. `strategy_for` is Strategy→Ability.
7. `transfers_to` is same-type Ability→Ability or Strategy→Strategy and never implies equivalence/prerequisite.
8. `contrasts_with` is same-type, symmetric and deduplicated.
9. learner state never appears in canonical edges.
10. rationale and source basis are reviewable.
