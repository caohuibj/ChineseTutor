# Dependency Graph semantics

Status: **B2 design draft**

This document explains how ChineseTutor should interpret canonical graph edges in tutoring, profile updates and recommendation. The normative field contract is in `schemas/learning-edge.schema.md`.

## 1. Why a dependency graph exists

The system must answer more than “which ability is weak?”. It must answer:

- is the target itself weak, or is a prerequisite causing failure?
- should the learner continue the target or remediate an upstream node?
- which mastered operation can be transferred to reduce learning cost?
- which Strategy is suitable as the smallest scaffold?
- which concept pair should be contrasted because confusion is recurrent?

Grade ordering cannot answer these questions reliably.

---

# 2. Edge semantics in operational language

## 2.1 `requires` — hard prerequisite

Form:

```text
TARGET requires PREREQUISITE
```

Meaning:

> Independent, semantically complete performance of TARGET presupposes at least usable access to PREREQUISITE.

Operational consequence:

- if learner evidence later shows the prerequisite is not ready, recommendation should normally remediate or scaffold it before treating the target as the main deficit;
- target attempts may still be shown diagnostically, but repeated unsupported drilling should be avoided.

Use `requires` sparingly.

### Necessity test

Imagine a learner who genuinely lacks the prerequisite. Could they still satisfy the target's `observable_success` across authentic tasks without accidental guessing or hidden substitution?

- **No** → `requires` may be justified.
- **Yes, but performance would be less efficient/reliable** → use `supports`.

---

## 2.2 `supports` — non-blocking support

Form:

```text
TARGET supports SUPPORT_NODE
```

Meaning:

> SUPPORT_NODE materially improves TARGET performance or learning efficiency, but TARGET is not semantically impossible without it.

Operational consequence:

- may increase recommendation value for support practice;
- may justify temporary scaffold;
- must never hard-block TARGET readiness.

Typical examples:

- scene reconstruction supports poetry emotion inference;
- genre knowledge supports richer literary interpretation;
- sentence cohesion knowledge supports extended writing revision.

---

## 2.3 `strategy_for` — reusable procedure applies to an Ability

Form:

```text
STRATEGY strategy_for ABILITY
```

Meaning:

> The Strategy is an effective reusable procedure for coordinating operations needed by the Ability.

Operational consequence:

- if Ability fails and Strategy profile is weak/unknown, tutor may remind the Strategy;
- if Strategy is automated but Ability still fails, diagnose Knowledge/other Ability prerequisites instead of re-teaching the procedure;
- reduced hint dependence on the Strategy becomes evidence for automation later.

A Strategy is normally **not** a hard prerequisite: learners may solve the same cognitive problem through another valid procedure.

---

## 2.4 `transfers_to` — learning in one node can accelerate another

Form:

```text
SOURCE transfers_to TARGET
```

Meaning:

> Mastery/automation of SOURCE creates reusable structure that can reduce learning cost or improve performance on TARGET.

This relation is directional.

Examples:

```text
分析典型事件的表现力
  transfers_to
选择能集中表现中心的写作材料

分析文本修改前后差异
  transfers_to
修改自己的句段表达
```

Operational consequence:

- after strong SOURCE evidence, TARGET can be selected as a transfer probe;
- scaffolding on TARGET can start lower than for a completely unrelated node;
- successful transfer strengthens both the transfer hypothesis and learner generalization evidence.

`transfers_to` does not mean TARGET is guaranteed, and it never substitutes for authentic TARGET evidence.

---

## 2.5 `contrasts_with` — discriminative relation

Meaning:

> Two same-type nodes are close enough to be commonly confused, but require a meaningful distinction.

Examples:

- 回忆性散文特征 vs 传记特征;
- 因果关系 vs 条件关系;
- 借景抒情 vs 托物言志（if represented as distinct Knowledge nodes）.

Operational consequence:

- repeated confusion can trigger paired discrimination practice;
- one node's success should not automatically update the other;
- the relation is symmetric but non-transitive.

---

## 2.6 `part_of` — structural projection

B1 already defines `parent_node_id` as the canonical taxonomic parent. B2 therefore treats `part_of` as a derived graph view:

```text
child.parent_node_id = parent
=> child part_of parent
```

It is useful for graph traversal/reporting but must not be independently edited.

---

# 3. Hard vs soft prerequisite policy

The main design risk is overusing `requires`, turning the graph into an artificial curriculum ladder.

Use these rules:

### Rule A — prerequisite must apply to the whole target semantics

Bad:

```text
诗词思想感情 requires 用典知识
```

because many poems do not use allusions.

Better:

```text
解释某用典在诗中的作用 requires 识别该典故相关知识
```

or keep allusion knowledge as `supports` for broader emotion inference.

### Rule B — task-specific condition is not a canonical hard dependency

If a prerequisite only appears because one particular question asks for it, attach it to Question/Task mapping rather than the broad Ability edge.

### Rule C — direct causal diagnosis matters

A hard edge should make a meaningful diagnostic statement:

> if prerequisite is absent, failure on target is expected for a clear reason.

### Rule D — strategies are usually routes, not prerequisites

Prefer:

```text
strategy_for
```

over:

```text
Ability requires Strategy
```

### Rule E — preserve multiple valid pathways

Do not create hard chains merely because the teacher usually teaches in that order.

---

# 4. How dependencies interact with Learner Profile later

B2 does not define M/A thresholds, but it establishes the decision pattern.

Suppose:

```text
T requires P1
T requires P2
T supports S1
STR strategy_for T
```

Future recommendation logic should inspect learner state in this order:

1. Is P1/P2 evidence sufficiently ready?
2. If not, is target failure plausibly caused upstream?
3. If prerequisites are ready, is the Strategy unknown/not automated?
4. If Strategy is ready too, target-specific practice is justified.
5. S1 may improve efficiency but never blocks training.

This creates a causal diagnostic path rather than “wrong answer → repeat same type”.

---

# 5. Dependency unlock value

A prerequisite that supports many important downstream nodes may deserve high training priority.

Example conceptually:

```text
解释证据为何支持结论
        ↓
人物判断
主旨判断
开放评价
诗歌情感解释
论证写作
```

The **unlock value** is derived analytics, not an authored edge property. It can be calculated from:

- number of downstream high-relevance nodes;
- edge type;
- path depth;
- current learner gaps.

Do not hard-code centrality into `LearningEdge`.

---

# 6. Cross-domain transfer as a first-class relation

One design objective is to stop treating reading, writing and different text eras as silos.

High-value transfer patterns already present in current materials include:

```text
阅读：分析典型事件为什么典型
        transfers_to
写作：选择能集中表现中心的材料

阅读：分析细节如何塑造人物
        transfers_to
写作：选择/构造有表现力的细节

阅读：比较原稿与修改稿的表达效果
        transfers_to
写作：诊断并修改自己的语言

现代/文言：证据→人物判断
        shared canonical Ability
```

The first three are transfer relations because analysis and production are different operations. The last is shared identity because the reasoning operation itself is identical.

This distinction is important:

- **same operation** → generalize one node;
- **different operation with reusable structure** → `transfers_to`.

---

# 7. Relation choice decision tree

When two nodes appear related, ask:

```text
Is one a canonical taxonomic child of the other?
  yes -> parent_node / derived part_of
  no  -> continue

Is B semantically necessary for complete independent performance of A?
  yes -> A requires B
  no  -> continue

Does B materially improve A but not block it?
  yes -> A supports B
  no  -> continue

Is A a Strategy and B an Ability the procedure coordinates?
  yes -> A strategy_for B
  no  -> continue

Does mastering A plausibly lower learning cost for B without being a prerequisite?
  yes -> A transfers_to B
  no  -> continue

Are A and B same-type concepts/operations that are commonly confused?
  yes -> A contrasts_with B
  no  -> probably no canonical edge
```

Not every topical association deserves an edge.

---

# 8. Cycle policy

## `requires`
Must be acyclic. A cycle indicates one of:

- the nodes are actually one composite construct;
- at least one edge should be `supports` rather than `requires`;
- direction is wrong;
- a middle node is missing.

## `supports`
Cycles are allowed because support can be reciprocal.

## `transfers_to`
Cycles are allowed only as two separately justified directional relations. Bidirectional transfer does not imply equivalence.

## `contrasts_with`
Symmetric by definition; store one canonical pair.

---

# 9. Activation and evidence policy

Draft edges can be proposed from:

- curriculum/material structure;
- authentic exam analysis;
- repeated tutoring evidence;
- stable pedagogical reasoning.

For an edge to become `active`:

### `requires`
Needs the strongest review standard:

- necessity rationale;
- whole-target applicability;
- no cycle;
- at least `supported` evidence level.

### `supports` / `transfers_to`
May activate at `supported`, but should remain revisable as learner evidence accumulates.

### `strategy_for`
Requires demonstrated reuse across more than one concrete item/material.

### `contrasts_with`
Should be backed by actual or expected confusion that changes practice design.

---

# 10. What B2 intentionally does not decide

- learner readiness thresholds (`M1` vs `M2` etc.);
- how much an edge changes recommendation score;
- automatic mastery propagation;
- forgetting/recency models;
- Question-specific prerequisite tagging;
- probabilistic causal inference from large learner datasets.

Those belong to Profile/Attempt/Recommendation PRs.
