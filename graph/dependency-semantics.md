# Dependency Graph semantics

Status: **B2 design draft — self-reviewed**

This document explains how ChineseTutor should interpret canonical graph edges in tutoring, profile updates and recommendation. The normative contract is in `schemas/learning-edge.schema.md`.

## 1. Why a dependency graph exists

The system must answer more than “which ability is weak?”. It must answer:

- is the target itself weak, or is an upstream prerequisite causing failure?
- should training continue on the target or switch to remediation?
- which mastered operation can transfer and reduce learning cost?
- which Strategy is appropriate as the smallest scaffold?
- which close concepts should be contrasted because confusion is likely?

Grade ordering cannot answer these questions reliably.

---

# 2. Edge semantics in operational language

## 2.1 `requires` — hard prerequisite

```text
TARGET requires PREREQUISITE
```

Meaning:

> Independent, semantically complete performance of TARGET presupposes usable access to PREREQUISITE.

Operational consequence:

- if learner evidence later shows the prerequisite is not ready, recommendation should normally remediate/scaffold upstream before treating the target as the main deficit;
- target attempts may still be diagnostic, but repeated unsupported drilling should be avoided.

Use `requires` sparingly.

### Necessity test

Could a learner who genuinely lacks the prerequisite still satisfy TARGET's full `observable_success` across authentic tasks without accidental guessing or hidden substitution?

- **No** → `requires` may be justified.
- **Yes, but less efficient/reliable** → use `supports`.

---

## 2.2 `supports` — non-blocking support

```text
SUPPORT_NODE supports TARGET
```

Meaning:

> SUPPORT_NODE materially improves TARGET performance or learning efficiency, but TARGET is not semantically impossible without it.

Operational consequence:

- may raise the value of support-node practice;
- may justify temporary scaffold;
- must never hard-block TARGET readiness.

Typical examples:

- scene reconstruction supports poetry emotion inference;
- relevant genre knowledge supports evidence-based genre judgment;
- sentence cohesion knowledge supports extended writing revision.

---

## 2.3 `strategy_for` — reusable procedure applies to an Ability

```text
STRATEGY strategy_for ABILITY
```

Meaning:

> The Strategy is an effective reusable procedure for coordinating operations needed by the Ability.

Operational consequence:

- if Ability fails and Strategy profile is weak/unknown, tutor may remind the Strategy;
- if Strategy is automated but Ability still fails, diagnose Knowledge/other Ability prerequisites instead of re-teaching the procedure;
- reduced hint dependence becomes Strategy automation evidence later.

A Strategy is normally **not** a hard prerequisite: learners may solve the same cognitive problem through another valid procedure.

---

## 2.4 `transfers_to` — learning in one node can accelerate another

```text
SOURCE transfers_to TARGET
```

Meaning:

> Mastery/automation of SOURCE creates reusable structure that can reduce learning cost or improve performance on TARGET.

B2 uses `transfers_to` only for same-type relations:

```text
Ability -> Ability
Strategy -> Strategy
```

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
- TARGET scaffolding may start lower than for an unrelated node;
- successful transfer strengthens generalization evidence but does not replace TARGET evidence.

If SOURCE and TARGET are actually the same observable operation, use one shared LearningNode rather than a transfer edge.

---

## 2.5 `contrasts_with` — discriminative relation

> Two same-type nodes are close enough to be commonly confused but require a meaningful distinction.

Examples:

- 回忆性散文特征 vs 传记特征;
- 因果关系 vs 条件关系;
- 借景抒情 vs 托物言志（when represented as distinct Knowledge nodes）.

Operational consequence:

- repeated confusion can trigger paired discrimination practice;
- success on one does not update the other automatically;
- relation is symmetric and non-transitive.

---

## 2.6 `part_of` — structural projection

B1 already defines `parent_node_id` as canonical taxonomy. B2 treats `part_of` as a derived graph view:

```text
child.parent_node_id = parent
=> child part_of parent
```

It is useful for traversal/reporting but must not be independently edited.

---

# 3. Hard vs soft prerequisite policy

The main design risk is overusing `requires`, recreating an artificial curriculum ladder.

### Rule A — prerequisite must apply to the whole target semantics

Bad:

```text
诗词思想感情 requires 用典知识
```

because many poems do not use allusions.

Better: narrower allusion-specific target, or:

```text
相关用典知识 supports broader poetry interpretation
```

### Rule B — task-specific condition is not a broad canonical hard dependency

If a prerequisite appears only because one question asks for it, attach it to Question/Task mapping or refine the Ability.

### Rule C — hard edge should explain causal failure

A good hard edge supports this diagnosis:

> without prerequisite, failure on target is expected for a clear semantic reason.

### Rule D — strategies are routes, not usually prerequisites

Prefer `strategy_for` over `Ability requires Strategy`.

### Rule E — preserve multiple valid pathways

Do not make a hard chain simply because that is the normal teaching sequence.

---

# 4. How dependencies interact with Learner Profile later

B2 does not define M/A thresholds, but it defines causal inspection order.

Suppose:

```text
T requires P1
T requires P2
S1 supports T
STR strategy_for T
```

Future recommendation logic should ask:

1. Are P1/P2 sufficiently ready?
2. If not, is target failure plausibly upstream-caused?
3. If prerequisites are ready, is the relevant Strategy unknown/not automated?
4. If Strategy is ready too, target-specific practice is justified.
5. Weak S1 may increase scaffold/support-practice priority but never blocks T.

This replaces “wrong answer → repeat same type” with causal diagnosis.

---

# 5. Dependency unlock value

A prerequisite feeding many high-value downstream nodes may deserve high training priority.

Conceptually:

```text
解释证据为何支持结论
        ↓
人物判断
主旨判断
开放评价
诗歌情感解释
论证写作
```

`unlock_value` is derived analytics, not an authored edge property. It may later depend on:

- number/importance of downstream nodes;
- edge type;
- path depth;
- current learner gaps.

Do not hard-code centrality in `LearningEdge`.

---

# 6. Cross-domain transfer as a first-class relation

Current school materials already contain high-value transfer patterns:

```text
阅读：分析典型事件为什么典型
        transfers_to
写作：选择能集中表现中心的材料

阅读：分析细节如何塑造人物
        transfers_to
写作：选择/构造有表现力的细节

阅读：比较原稿与修改稿的效果
        transfers_to
写作：诊断并修改自己的表达

现代/文言：证据→人物判断
        shared canonical Ability
```

Distinction:

- **same observable operation** → one generalized node;
- **different same-type operations with reusable structure** → `transfers_to`;
- **Strategy applied to Ability** → `strategy_for`;
- **helpful different-type relation** → `supports`.

---

# 7. Relation choice decision tree

```text
Is one a canonical taxonomic child of the other?
  yes -> parent_node / derived part_of
  no  -> continue

Is B semantically necessary for complete independent performance of A?
  yes -> A requires B
  no  -> continue

Does B materially improve A but not block it?
  yes -> B supports A
  no  -> continue

Is A a Strategy and B an Ability the procedure coordinates?
  yes -> A strategy_for B
  no  -> continue

Are A and B same-type operations/procedures, and does mastering A lower learning cost for B?
  yes -> A transfers_to B
  no  -> continue

Are A and B same-type nodes commonly confused?
  yes -> A contrasts_with B
  no  -> probably no canonical edge
```

Not every topical association deserves an edge.

---

# 8. Cycle policy

## `requires`
Must be acyclic. A hard cycle usually means:

- nodes are one composite construct;
- at least one edge should be `supports`;
- direction is wrong;
- a middle abstraction is missing.

## `supports`
Reciprocal soft support is allowed and non-blocking.

## `transfers_to`
Cycles are allowed only as two separately justified directional relations. Bidirectional transfer does not imply equivalence.

## `contrasts_with`
Symmetric by definition; store one canonical pair.

---

# 9. Activation and evidence policy

Draft edges may be proposed from:

- curriculum/material structure;
- authentic exam analysis;
- repeated tutoring evidence;
- stable pedagogical reasoning.

For activation:

### `requires`
Needs strongest review:

- necessity rationale;
- whole-target applicability;
- no cycle;
- at least `supported` evidence level.

### `supports` / `transfers_to`
May activate at `supported`, remaining revisable as evidence accumulates.

### `strategy_for`
Should demonstrate reuse across more than one item/material; broad mother strategies eventually need stronger cross-task evidence.

### `contrasts_with`
Should correspond to an actual or credible confusion that changes practice design.

---

# 10. What B2 intentionally does not decide

- learner readiness thresholds (`M1` vs `M2` etc.);
- numerical recommendation weights;
- automatic mastery propagation;
- forgetting/recency models;
- Question-specific prerequisite tagging;
- probabilistic causal inference from large learner datasets.

Those belong to Profile/Attempt/Recommendation PRs.
