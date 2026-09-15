# QA gates — LearningEdge / dependency graph (B2)

These gates are normative acceptance checks for Issue #2.

## Gate 1 — Relation semantics are unambiguous

Every persisted directed edge must read naturally as:

```text
FROM relation TO
```

Examples:

```text
translation requires contextual-word-sense
memoir-genre-knowledge supports genre-judgment
translation-five-step strategy_for translation
reading-event-analysis transfers_to writing-material-selection
```

Fail if direction must be guessed from prose.

`part_of` is not a persisted LearningEdge; it is a virtual projection from B1 `parent_node_id`.

---

## Gate 2 — No self edge

Reject `A relation A` for every persisted relation type. B1 parent validation separately rejects self-parenting.

---

## Gate 3 — Hard prerequisite necessity test

For `DEPENDENT requires PREREQUISITE`, ask:

> Could a learner who genuinely lacks PREREQUISITE still satisfy the full observable-success semantics of DEPENDENT across authentic tasks without accidental guessing or hidden substitution?

- yes → edge is too strong; use `supports` or refine target;
- no → `requires` may be valid.

---

## Gate 4 — Hard prerequisite applies to whole dependent semantics

Fail:

```text
诗词思想感情 requires 用典知识
```

because only some poems require allusion knowledge.

Prefer a narrower dependent Ability or a support relation.

---

## Gate 5 — Active `requires` graph is acyclic

Before activating a new hard edge, run cycle detection/topological-sort validation.

Fail:

```text
A requires B
B requires C
C requires A
```

A hard cycle indicates wrong granularity, wrong direction, or an edge that should be soft.

---

## Gate 6 — Transitive closure is not redundantly stored

Given:

```text
A requires B
B requires C
```

do not automatically author `A requires C`.

Add it only if C is also a direct semantic prerequisite whose explicit edge improves diagnosis.

---

## Gate 7 — `supports` direction and non-blocking semantics

Canonical grammar:

```text
SUPPORT_NODE supports TARGET
```

A missing support node may influence scaffold/recommendation, but cannot make TARGET categorically unavailable.

Fail reversed records whose prose says “B supports A” while storage encodes `A supports B`.

---

## Gate 8 — Strategy endpoint types are valid

`strategy_for` must be:

```text
Strategy -> Ability
```

TaskType strategy recommendations live in the Task graph.

Fail Strategy→Knowledge, Ability→Ability as `strategy_for`, or TaskType IDs in LearningEdge.

---

## Gate 9 — Strategy is not a hard-prerequisite target

Reject by default:

```text
Ability requires Strategy
```

A Strategy is usually one valid route, not a semantic prerequisite. Use `strategy_for`.

A Strategy itself may `require` component Knowledge/Abilities needed to execute the procedure.

---

## Gate 10 — `part_of` is virtual, never a persisted LearningEdge

If:

```text
child.parent_node_id = parent
```

then graph views may expose:

```text
child part_of parent
```

Reject:

- any stored LearningEdge row with `relation_type: part_of`;
- independently authored rationale/evidence/status for `part_of`;
- any graph representation that can disagree with `parent_node_id`.

The derived taxonomy must satisfy B1 parent constraints and remain acyclic.

---

## Gate 11 — `contrasts_with` is one canonical symmetric pair

Requirements:

- endpoints have same node type;
- no self pair;
- store one lexicographically canonical pair;
- no transitive inference.

Fail duplicate `(A,B)` + `(B,A)` records.

---

## Gate 12 — `transfers_to` is same-type, directional and non-blocking

Allowed in B2:

```text
Ability -> Ability
Strategy -> Strategy
```

Pass:

```text
analyze typical-event value transfers_to select typical writing material
```

Fail if the system interprets transfer as:

- target already mastered;
- target prerequisite satisfied automatically;
- source and target are identical semantics;
- Strategy→Ability (use `strategy_for`);
- Knowledge→Ability (usually use `supports`).

If semantics are identical, use one shared LearningNode.

---

## Gate 13 — No grade-derived dependency

Fail:

```text
高一节点 requires 初三节点 because grade order
```

Pass only when semantic necessity independently justifies the edge.

Source grade may remain metadata elsewhere.

---

## Gate 14 — Edge rationale is causal/useful, not topical

Fail:

> “两者都与人物有关。”

Pass:

> “准确文言翻译必须先解析关键词在当前语境中的义项，否则核心命题无法稳定重构。”

---

## Gate 15 — Authored edge should alter downstream behavior

At least one must be true:

- changes readiness/remediation;
- changes scaffold selection;
- creates a meaningful transfer probe;
- enables confusion discrimination.

Taxonomic navigation is not a reason to author a LearningEdge because it is already derived from B1 parent hierarchy.

If none, do not create the edge.

---

# Representative scenarios

## Scenario A — Classical translation

Expected:

```text
translate classical sentence
  requires contextual classical word sense

classical translation five-step
  strategy_for translation
```

This allows diagnosis to separate missing lexical decoding from missing procedure.

## Scenario B — Character reasoning

Expected:

```text
evidence-explanation-conclusion
  strategy_for evidence-to-character-judgment
```

Do not create separate modern/classical reasoning nodes merely to reproduce domain silos.

## Scenario C — Writing material selection

Expected:

```text
select typical writing material
  requires parse writing prompt constraints
```

because target semantics are defined relative to prompt/central constraints.

## Scenario D — Genre knowledge

Expected broad relation:

```text
memoir genre features
  supports judge modern genre from evidence
```

not `requires`, because the broad Ability can be demonstrated on other genres without memoir knowledge. A narrower memoir-specific judgment node could later have a hard prerequisite.

## Scenario E — Reading→writing transfer

Expected:

```text
analyze typical-event value
  transfers_to select typical writing material
```

Analysis and production are different Abilities but share reusable structural insight.

---

# B2 definition of done

- [x] persisted relation types and natural directions defined;
- [x] virtual `part_of` projection separated from persisted LearningEdge records;
- [x] hard vs soft prerequisite rule defined;
- [x] endpoint-type constraints defined;
- [x] taxonomy source-of-truth conflict resolved;
- [x] cycle policy defined;
- [x] transitive closure policy defined;
- [x] transfer/generalization distinction defined;
- [x] recommendation semantics described without learner-state leakage;
- [x] current-asset seed edges added;
- [x] QA gates added;
- [x] two semantic review passes completed;
- [x] stacked PR opened (#12).
