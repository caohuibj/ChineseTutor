# QA gates — LearningEdge / dependency graph (B2)

These gates are normative acceptance checks for Issue #2.

## Gate 1 — Relation semantics are unambiguous

Every edge must read naturally as:

```text
FROM relation TO
```

Examples:

```text
translation requires contextual-word-sense
translation-five-step strategy_for translation
reading-event-analysis transfers_to writing-material-selection
```

Fail if direction must be guessed from prose.

---

## Gate 2 — No self edge

Reject `A relation A` for every relation type.

---

## Gate 3 — Hard prerequisite necessity test

For `FROM requires TO`, ask:

> Could a learner who genuinely lacks TO still satisfy the full observable-success semantics of FROM across authentic tasks without accidental guessing or hidden substitution?

- yes → edge is too strong; use `supports` or refine target;
- no → `requires` may be valid.

---

## Gate 4 — Hard prerequisite applies to whole target semantics

Fail:

```text
诗词思想感情 requires 用典知识
```

because only some poems require allusion knowledge.

Prefer a narrower target or `supports`.

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

Do not automatically author `A requires C`.

Add it only if C is also a direct semantic prerequisite whose explicit edge improves diagnosis.

---

## Gate 7 — Strategy endpoint types are valid

`strategy_for` must be:

```text
Strategy -> Ability
```

TaskType strategy recommendations live in the Task graph.

Fail Strategy→Knowledge, Ability→Ability as `strategy_for`, or TaskType IDs in LearningEdge.

---

## Gate 8 — Strategy is not used as a hard prerequisite target

Reject by default:

```text
Ability requires Strategy
```

A Strategy is usually one valid route, not a semantic prerequisite. Use `strategy_for`.

A Strategy itself may `require` component Knowledge/Abilities necessary to execute the procedure.

---

## Gate 9 — `part_of` is derived, not hand-authored

If:

```text
child.parent_node_id = parent
```

then a graph view may expose:

```text
child part_of parent
```

Fail any independently maintained `part_of` record that could diverge from B1 taxonomy.

---

## Gate 10 — `contrasts_with` is canonical symmetric pair

Requirements:

- endpoints have same node type;
- no self pair;
- store one lexicographically canonical pair;
- no transitive inference.

Fail duplicate `(A,B)` + `(B,A)` records.

---

## Gate 11 — `transfers_to` is directional, non-equivalent and non-blocking

Pass:

```text
analyze typical event value transfers_to select typical writing material
```

Fail if the system interprets transfer as:

- target already mastered;
- target prerequisite satisfied automatically;
- source and target are identical semantics.

If semantics are identical, use one shared LearningNode instead.

---

## Gate 12 — `supports` never blocks readiness

A missing support node may influence scaffold/recommendation, but cannot make the target categorically unavailable.

If it must block, re-evaluate whether the relation is truly `requires`.

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

## Gate 15 — Edge should alter downstream behavior

At least one must be true:

- changes readiness/remediation;
- changes scaffold selection;
- creates a meaningful transfer probe;
- enables confusion discrimination;
- represents canonical taxonomy in graph view.

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

because the target's own semantics are defined relative to prompt/central constraints.

## Scenario D — Genre knowledge

Expected broad relation:

```text
judge modern genre from evidence
  supports memoir genre features
```

not `requires`, because the broad Ability can be demonstrated on other genres without memoir knowledge. A narrower memoir-specific judgment node could have a hard prerequisite if later needed.

## Scenario E — Reading→writing transfer

Expected:

```text
analyze typical-event value
  transfers_to select typical writing material
```

Analysis and production are different nodes but share reusable structural insight.

---

# B2 draft definition of done

- [x] relation types and directions defined;
- [x] hard vs soft prerequisite rule defined;
- [x] endpoint-type constraints defined;
- [x] `part_of` source-of-truth conflict resolved;
- [x] cycle policy defined;
- [x] transitive closure policy defined;
- [x] transfer/generalization distinction defined;
- [x] recommendation semantics described without learner-state leakage;
- [x] current-asset seed edges added;
- [x] QA gates added;
- [ ] semantic self-review completed;
- [ ] stacked PR opened.
