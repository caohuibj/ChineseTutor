# TaskType canonical schema

Status: **D1 normative draft**

A `TaskType` is a reusable authentic problem form: the way a learner is asked to operate on material or produce language.

Examples:

- 人物形象分析;
- 多文本信息整合;
- 文言翻译;
- 诗歌炼字;
- 证据充分性评价;
- 作文选材;
- 情境改写.

A TaskType is a **training/assessment environment**, not a Knowledge or Ability node.

---

## 1. Identity

```yaml
task_type_id: TT-...
name_zh: string
domain: LAN | MRD | CLA | POE | LIT | WRT | COM | META
subdomain: string
status: draft | active | deprecated
```

The ID is stable across wording changes and independent of grade/year.

One TaskType may exercise many LearningNodes; one LearningNode may appear across many TaskTypes.

---

## 2. Pedagogical definition

```yaml
definition: string
recognition_signals: string
core_question: string
output_skeleton: string | null
self_checks: string
boundaries: string
```

### `recognition_signals`
Question wording/presentation cues that help identify the task family.

### `core_question`
The underlying problem the learner must solve, not a model answer.

### `output_skeleton`
Optional minimal answer structure. It must not turn into a fixed sentence template that bypasses reasoning.

### `boundaries`
Clarifies neighboring TaskTypes and when this TaskType label should not be used.

---

## 3. Graph mappings

```yaml
default_knowledge_node_ids: string[]
default_ability_node_ids: string[]
recommended_strategy_node_ids: string[]
```

These are **defaults/typical mappings**, not claims that every concrete Question uses every listed node.

The concrete Question remains authoritative for its actual target-node mapping.

### Why TaskType is separate from Ability

`标题含义与作用题` may require:

- semantic inference;
- structure integration;
- theme inference;
- evidence explanation.

It is therefore a TaskType, not one atomic Ability.

Likewise, `炼字` is a recurring task environment whose underlying operations may include contextual word meaning, comparison/substitution, effect explanation and emotion/theme integration.

---

## 4. Strategy relation

TaskType may recommend Strategies because certain mother procedures are often useful in that environment.

This is different from B2 `Strategy strategy_for Ability`:

```text
TaskType -> recommended Strategy
```

is a Task-graph recommendation mapping.

```text
Strategy strategy_for Ability
```

is a canonical learning-graph semantic relation.

Do not store TaskType IDs in `LearningEdge`.

---

## 5. Source/grade neutrality

TaskTypes are not named by grade or exam year.

Reject:

```text
初二人物形象题
高一信息整合题
2026高考开放题
```

Prefer stable forms:

```text
人物形象分析
多文本信息整合
证据充分性评价
```

Authentic source/year belongs to Material/Question provenance.

---

## 6. Learner state is forbidden

TaskType does not contain:

```text
学生掌握度
训练状态
最近做过
待复练
个人正确率
```

A future analytics view may aggregate Attempts by TaskType, but those values are derived learner analytics, not canonical TaskType fields.

---

## 7. Migration from current 题型地图

The existing Notion `题型地图` maps naturally to TaskType:

| current field | D1 destination |
| --- | --- |
| `题型` | `name_zh` |
| `大类` | `domain/subdomain` mapping |
| `识别信号` | `recognition_signals` |
| `核心问题` | `core_question` |
| `3—5步算法` | usually recommended Strategy mapping; do not automatically create one Strategy per TaskType |
| `输出骨架` | `output_skeleton` |
| `自检点` | `self_checks` |
| `适用边界` | `boundaries` |
| `核心能力` relation | default LearningNode mapping after B1 normalization |
| `初二定位` | legacy planning metadata; not canonical TaskType identity |
| `真实来源示例` | examples/provenance references, not definition |

The migration must avoid recreating “35 TaskTypes = 35 Strategies”. Strategy consolidation remains G1 scope.

---

## 8. TaskType invariants

1. TaskType describes how a task asks the learner to operate, not what the learner permanently knows/can do.
2. TaskType identity is grade-independent.
3. Concrete Question target mappings override broad TaskType defaults.
4. Recommended Strategies are reusable procedures, not mandatory prerequisites.
5. Learner state never lives on TaskType.
6. Output skeletons cannot substitute for reasoning.
7. Existing school terminology can be retained where it is a stable task label.
