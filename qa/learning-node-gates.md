# QA gates — canonical LearningNode (B1)

These gates are normative acceptance tests for Issue #1.

## Gate 1 — Semantic type can be decided

For each candidate node, editors must be able to answer exactly one primary question:

```text
Knowledge: what stable fact/concept/distinction is known?
Ability: what observable operation is performed?
Strategy: what reusable multi-step procedure is invoked?
```

Fail if one row requires two answers, unless it is deliberately represented by multiple nodes.

### Test cases

| candidate | expected | reason |
| --- | --- | --- |
| 比喻 | Knowledge | concept/device identity |
| 解释比喻在本句中的具体效果 | Ability | observable contextual analysis |
| 词句→语境义→手法→效果→主旨 | Strategy | reusable sequence |
| 标题含义与作用题 | not LearningNode leaf | Task Type/environment |
| 文言实词 | split | lexical Knowledge + contextual inference Ability |

---

## Gate 2 — Identity survives operational storage changes

Given the same semantic node:

- moving it to another Notion database;
- changing a Notion page ID;
- changing Chinese display wording without semantic change;
- importing it to code;

must not change `node_id`.

Fail if the canonical ID derives from Notion IDs or a mutable title.

---

## Gate 3 — No learner state leaks into canonical node

Reject any canonical schema containing:

```text
mastery
automation
current score
last trained
review due
needs improvement
attempt count
personal bottleneck
```

Those fields belong to Learner Profile / Attempt layers.

---

## Gate 4 — No grade-first progression semantics

A node may cite source grade/curriculum evidence externally, but its canonical definition, identity and prerequisite structure must not depend on:

```text
初二
初三
高一
...
```

Fail examples:

```text
CN-A-MRD-grade8-character-analysis
高一议论文概括能力
```

Pass:

```text
CN-A-MRD-evidence-to-character-trait
CN-A-MRD-identify-argument-structure
```

Task complexity and learner readiness decide when these are trained.

---

## Gate 5 — Ability is observable

Every active Ability must have an `observable_success` that can be judged from a real Attempt/artifact.

Fail:

> “理解人物形象。”

Pass:

> “在陌生叙事文本中，能引用事实/情境并显性解释为什么这些证据支持某项稳定人物特征。”

---

## Gate 6 — Strategy is reusable and fadeable

A Strategy passes only if:

1. it coordinates two or more operations;
2. it is useful across multiple questions/materials;
3. tutor can remind it at a lower hint level;
4. learner can later invoke it without reminder;
5. it is not a fixed answer sentence.

Fail:

> “这句话运用了……生动形象地……”

Pass:

> `比较维度 → 分别取证 → 同异 → 意义`

---

## Gate 7 — Parent is taxonomy, not dependency

A parent relationship should answer:

> “This node is a kind/part of what?”

not:

> “What must I master before this?”

Fail:

```text
parent(文言翻译) = 文言实词
```

Pass:

```text
parent(宾语前置) = 文言句法知识
requires(文言翻译, 文言实词语境义)
```

The latter edge is B2 scope.

---

## Gate 8 — Mixed v1 labels have explicit disposition

All 42 v1 ability-map rows must appear in `graph/seed/v1-ability-map-migration.md`.

Required disposition classes include at least:

- keep Ability;
- split K+A;
- split family;
- Task Type + underlying nodes;
- generalize/merge cross-domain;
- transfer-evidence;
- performance family.

Fail if a row is silently dropped.

---

## Gate 9 — Cross-domain transfer is not blocked by duplicate nodes

If the observable reasoning operation is materially identical across domains, default to a shared node plus domain-specific prerequisites.

Representative test:

```text
modern character reasoning
classical character reasoning
```

Expected architecture:

```text
shared evidence-to-character judgment Ability
+ modern/classical decoding prerequisites as appropriate
```

Fail if profile treats identical reasoning as unrelated solely because source language differs.

---

## Gate 10 — Diagnostic granularity is sufficient but not excessive

For a proposed split, require at least one practical reason:

- distinct intervention;
- distinct prerequisites;
- distinct evidence;
- distinct forgetting/review behavior;
- distinct recommendation behavior.

Fail if creating a micro-node adds no diagnostic or recommendation value.

Fail also if a huge node prevents locating the failure.

---

# Representative B1 acceptance scenarios

## Scenario A — Modern reading

Input label: `修辞与表达效果`

Expected:

```text
Knowledge: specific rhetorical device/construction
Ability: explain its concrete effect in current context
Task Type may ask “赏析/表达效果”
```

Do not preserve the mixed row as one canonical leaf.

## Scenario B — Classical Chinese

Input label: `文言实词`

Expected:

```text
Knowledge: accumulated word-sense network
Ability: choose/infer contextual sense
```

Profile can then distinguish “did not know the word” from “knew meanings but selected the wrong one in context”.

## Scenario C — Poetry

Input label: `意象与意境`

Expected:

```text
Knowledge: conventional image associations where relevant
Ability: construct current poem's scene/atmosphere from local evidence
Ability: explain relation to emotion if demanded
```

Do not permit memorized symbolic associations to replace textual evidence.

## Scenario D — Writing

Input label: `作文审题与立意`

Expected decomposition:

```text
parse prompt constraints
form central question
form controlling idea
```

A learner can therefore be strong at constraint parsing but weak at converting it into a meaningful central question.

## Scenario E — Language use

Input label: `语病与句子表达`

Expected:

```text
Knowledge: syntax/logical-relation concepts
Ability: diagnose problem
Ability: perform minimal valid revision
```

The system should know whether the learner could detect but not repair, or repaired accidentally without explaining the defect.

---

# B1 definition of done

B1 is complete when:

- [x] stable ID convention is specified;
- [x] canonical required/optional fields are specified;
- [x] Knowledge/Ability/Strategy semantic decision rules are specified;
- [x] domain/subdomain taxonomy is specified without grade progression;
- [x] split/merge/generalization rules are specified;
- [x] every v1 ability-map row has a migration disposition;
- [x] examples cover modern reading, classical Chinese, poetry, writing and language use;
- [x] learner-state fields are explicitly prohibited from LearningNode;
- [x] parent-vs-prerequisite distinction is explicit;
- [ ] architecture PR review accepts these semantics;
- [ ] live Notion schema is changed — intentionally deferred to migration PRs.
