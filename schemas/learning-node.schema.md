# LearningNode canonical schema

Status: **B1 normative draft**

This document defines the canonical unit of the ChineseTutor Learning Graph. It is a semantic contract, not a Notion-specific database design.

## 1. Core rule

A `LearningNode` describes something stable about the learning domain. It must not describe one learner's current state, one particular question, one school year, or one study session.

The canonical registry contains exactly three semantic node types:

- `knowledge`: facts, concepts, distinctions, language/literary/cultural rules or conventions that can be known, recognized, recalled, or understood;
- `ability`: observable operations that can be performed on a text, task, artifact, or communicative situation;
- `strategy`: reusable procedures that organize multiple operations and can be deliberately invoked, faded, automated, and transferred.

If a proposed node mixes these meanings, split it before adding it to the canonical graph.

---

## 2. Stable Node ID

### 2.1 Format

```text
CN-<TYPE>-<DOMAIN>-<SLUG>
```

where:

- `<TYPE>` is `K`, `A`, or `S`;
- `<DOMAIN>` is the canonical domain code;
- `<SLUG>` is a stable, short English identifier in kebab-case.

Examples:

```text
CN-K-CLA-classical-content-word-meaning
CN-A-CLA-infer-contextual-word-sense
CN-A-MRD-evidence-to-character-trait
CN-S-META-evidence-explanation-conclusion
CN-A-WRT-select-typical-material
```

### 2.2 Identity rules

1. The ID is independent of Notion page IDs, titles, database names, grade labels and source materials.
2. Renaming the Chinese display name does **not** change the ID when the concept remains semantically identical.
3. If a node changes meaning materially, create a new ID and deprecate/redirect the old one.
4. IDs are never recycled.
5. A node moved to another navigation parent keeps its ID unless its semantic meaning changed.

### 2.3 Why IDs are English-slug based

The learner-facing system remains Chinese. The slug exists only to provide durable machine identity across Notion, GitHub and future code. It avoids identity changing when Chinese terminology is normalized.

---

## 3. Canonical domain codes

| code | domain | purpose |
| --- | --- | --- |
| `LAN` | 语言文字基础 | vocabulary, syntax, orthography, language precision |
| `MRD` | 现代文阅读 | modern literary/informational reading |
| `CLA` | 文言文 | classical-Chinese language and prose reading |
| `POE` | 古诗词 | poetry knowledge, reading and appreciation |
| `LIT` | 文学与文化 | genres, authors/works, literary/cultural knowledge, whole-book reading |
| `WRT` | 写作 | narrative, argumentative, source-based and revision abilities |
| `COM` | 语言运用与真实交流 | practical communication, transformation, audience-purpose tasks, cross-media |
| `META` | 跨域推理、评价与迁移 | cross-domain reasoning and transfer operations/strategies |

Domain is a **coverage/navigation classification**, not a progression level. A node may be cross-domain while retaining one primary domain and secondary tags/relations.

---

## 4. Required fields

```yaml
node_id: string
name_zh: string
node_type: knowledge | ability | strategy
domain: LAN | MRD | CLA | POE | LIT | WRT | COM | META
subdomain: string
definition: string
observable_success: string
scope_in: string
scope_out: string
parent_node_id: string | null
gaokao_relevance: core | supporting | enrichment
status: draft | active | deprecated
aliases_zh: string[]
source_basis: reference[]
```

### Field semantics

#### `node_id`
Permanent machine identity governed by Section 2.

#### `name_zh`
Short learner/teacher-facing canonical Chinese name. It should describe the semantic object, not one exam label.

#### `node_type`
The Knowledge / Ability / Strategy distinction. This is the most important semantic field.

#### `domain`
Primary coverage domain. It must not imply that the node can only transfer inside that domain.

#### `subdomain`
A stable navigation/coverage family such as `词义`, `人物与叙事`, `文言句法`, `论证`, `信息整合`.

#### `definition`
What the node means. Definition must be self-contained enough that two editors can make the same tagging decision.

#### `observable_success`
What successful use looks like in learner evidence.

For Knowledge, observable success may be recognition, recall, distinction or accurate explanation.

For Ability, it must describe an observable operation and output.

For Strategy, it must describe correct procedure selection/execution, not merely final-answer correctness.

#### `scope_in`
Explicit inclusions: what examples or operations belong to this node.

#### `scope_out`
Explicit boundaries: nearby concepts/tasks that do not belong to this node.

#### `parent_node_id`
Taxonomic decomposition only. It is **not** a prerequisite edge. Hard/soft prerequisites belong in the dependency-edge model.

#### `gaokao_relevance`
- `core`: repeatedly required directly or as a central prerequisite in Gaokao;
- `supporting`: materially supports core performance but is not consistently isolated as a tested endpoint;
- `enrichment`: useful for broader literacy but not required for baseline coverage.

It does not determine training order by itself.

#### `status`
- `draft`: semantics still under review;
- `active`: canonical and available for mapping/profile;
- `deprecated`: historical identity retained but new tagging should use successor nodes.

#### `aliases_zh`
Common textbook, school or exam terminology that resolves to the same canonical semantic node.

Aliases must not be used to merge genuinely distinct nodes.

#### `source_basis`
Evidence supporting why the node belongs in the graph, e.g. curriculum standards, authentic exams, current school materials, or repeated operational need. Source evidence justifies coverage; it is not learner evidence.

---

## 5. Optional fields

```yaml
short_description: string
secondary_domains: string[]
canonical_examples: string[]
common_confusions: string[]
deprecated_by: string | null
notes: string
```

These fields improve authoring and QA but are not required for identity.

---

## 6. Fields forbidden from canonical LearningNode

The following belong elsewhere and must not be stored as canonical meaning:

```text
mastery
current_mastery
student_score
automation
complexity_ceiling
recent_training
last_trained
needs_improvement
review_due
forgetting_risk
training_status
attempt_count
hint_level
source_grade
current_grade
school_unit
question_status
```

Correct homes:

- personal state -> `LearnerNodeState`;
- performance evidence -> `TrainingAttempt`;
- task/source metadata -> `Material`, `Question`, `TaskType`;
- scheduling -> `TrainingMove` / training queue.

---

## 7. Node-type decision rules

### 7.1 Knowledge

Use `knowledge` when the central semantic question is:

> **What must the learner know, recognize, distinguish, recall or understand?**

Knowledge names should normally be noun phrases or distinctions.

Good:

```text
比喻的构成与常见作用
宾语前置
古今异义
回忆性散文的文体特征
论据与论点的关系
```

Bad as Knowledge:

```text
分析比喻表达效果       # observable operation -> ability
准确翻译文言句子       # operation -> ability
辨-定-证-推-答-检      # procedure -> strategy
```

A knowledge node may have memory/recognition evidence, but forgetting state belongs in the learner profile.

### 7.2 Ability

Use `ability` when the central semantic question is:

> **What can the learner observably do with language, text, information or an artifact?**

Ability names should normally use an action verb plus an object/quality condition.

Good:

```text
定位与题目相关的关键信息
压缩信息且保持原意
建立有效比较维度
解释证据为何支持人物判断
根据语境推断文言实词义
准确翻译文言句子
用文本证据判断文体
选择能集中表现中心的典型材料
```

Avoid vague topics:

```text
人物形象          # topic, not observable operation
修辞              # knowledge family
开放题            # task type
作文               # domain
```

Composite abilities are allowed when they are stable, useful reporting outcomes, but they should connect to decomposed prerequisite abilities rather than hide all subskills inside one score.

### 7.3 Strategy

Use `strategy` when the central semantic question is:

> **What reusable ordered procedure helps the learner coordinate multiple knowledge/ability nodes?**

A strategy must satisfy all of:

1. contains at least two meaningful operations or decision steps;
2. applies to more than one concrete question/material, preferably more than one Task Type;
3. can be explicitly reminded, partially faded, independently invoked and automated;
4. is not merely an answer sentence template.

Good:

```text
证据 -> 解释 -> 结论
比较维度 -> 分别取证 -> 同异 -> 意义
内容 -> 位置 -> 上下文关系 -> 作用
圈关键词 -> 逐词落实 -> 调整句式 -> 补省略 -> 通读校验
```

Bad:

```text
“这句话运用了……生动形象地……”
```

The latter is an answer shell, not a reasoning strategy.

---

## 8. Split and merge rules

### Split when

- one current label contains both knowledge and operation (`修辞与表达效果`);
- one label contains multiple knowledge families with different prerequisites (`词类活用与特殊句式`);
- one label mixes a Task Type with underlying abilities (`标题含义与作用`);
- evidence requirements differ materially (`作文语言与修改`).

### Merge or generalize when

- the observable operation is semantically identical across text domains;
- separate v1 labels exist only because one occurs in modern prose and one in classical prose;
- shared abstraction improves transfer without losing important domain prerequisites.

Example:

```text
现代人物形象判断
文言人物形象判断
```

should share a cross-domain ability such as:

```text
CN-A-META-evidence-to-character-trait
由文本事实与情境推断人物稳定特征
```

while classical decoding remains a prerequisite for classical material.

### Do not merge when

- the operation has materially different knowledge/production demands;
- the same Chinese label hides different success criteria;
- merging would make profile diagnosis unable to locate the actual failure.

---

## 9. Parent hierarchy rules

`parent_node_id` describes decomposition/navigation, not learning order.

Example:

```text
文言句法知识
  ├─ 判断句
  ├─ 省略句
  ├─ 被动句
  └─ 倒装句
```

Do **not** encode:

```text
文言实词 -> parent -> 文言翻译
```

That is a prerequisite relationship and belongs to a graph edge such as `requires`.

A node has at most one canonical taxonomic parent in B1. Cross-cutting relationships use graph edges/tags instead of multiple parents.

---

## 10. Naming convention

### Knowledge

Prefer canonical concept names:

```text
古今异义
条件关系
小说叙事视角
论据类型
```

### Ability

Prefer `动词 + 对象 + 成功约束`:

```text
识别因果与条件关系
按同一维度比较两个文本
解释细节对人物塑造的作用
压缩多处信息且不遗漏核心条件
```

### Strategy

Prefer a short conceptual name plus the procedural chain in the definition:

```text
证据—解释—结论
比较分析链
结构作用链
文言翻译五步法
```

### Avoid

- grade labels: `初二人物形象`;
- source labels: `周亚夫人物分析`;
- score labels: `中考8分题`;
- broad containers as leaf nodes: `现代文阅读`;
- question wording as node identity: `为什么选这件事`;
- vague competence adjectives: `阅读理解能力`.

---

## 11. Representative canonical examples

### Modern reading

```yaml
node_id: CN-A-MRD-evidence-to-character-trait
name_zh: 由事实、情境与选择推断人物特征
node_type: ability
domain: MRD
subdomain: 人物与叙事
definition: 从文本中的事件、细节和特殊情境出发，解释人物选择为何能支持某项相对稳定的人物特征判断。
observable_success: 面对陌生叙事文本，能给出文本事实，并显性写出证据到人物判断之间的理由，不只贴品质标签。
scope_in: 人物形象、人物评价、典型事件中的人物选择解释。
scope_out: 仅识别描写方法；仅概括事件；无证据的价值判断。
parent_node_id: null
gaokao_relevance: core
status: active
aliases_zh: [人物形象分析]
source_basis: [现有学校人物叙事单元, 近年文学阅读]
```

### Classical Chinese

```yaml
node_id: CN-A-CLA-infer-contextual-word-sense
name_zh: 根据语境判断文言词语义项
node_type: ability
domain: CLA
subdomain: 词义与解码
definition: 在已知或部分已知词义网络基础上，根据句法、上下文事件和搭配选择当前语境中的合理义项。
observable_success: 在陌生短篇中能说明词义选择与句意相容，并避免只凭背诵机械套义。
scope_in: 实词义项、一词多义语境选择、部分活用后的语义判断。
scope_out: 单纯背诵词条；虚词语法功能；完整句子翻译。
parent_node_id: null
gaokao_relevance: core
status: active
aliases_zh: [文言实词语境义]
source_basis: [文言教材与课外迁移题]
```

### Poetry

```yaml
node_id: CN-A-POE-link-image-to-emotion
name_zh: 解释意象、画面与情感之间的关系
node_type: ability
domain: POE
subdomain: 意象与情感
definition: 从具体景物、事件和关键词出发，解释画面特征如何形成情绪方向并指向情感对象。
observable_success: 不只给出“思乡/悲伤”等标签，而能用诗中词句说明情绪为何成立。
scope_in: 意象、意境、景情关系、诗歌情感。
scope_out: 默写；单纯定义意象；只判断修辞名称。
parent_node_id: null
gaokao_relevance: core
status: active
aliases_zh: [诗词思想感情, 意象与意境分析]
source_basis: [古诗词真题与教材]
```

### Writing

```yaml
node_id: CN-A-WRT-select-typical-material
name_zh: 选择能集中表现中心的典型材料
node_type: ability
domain: WRT
subdomain: 选材
definition: 根据题目限制和中心问题，从候选经历/材料中选择具有情境压力、关键选择或辨识度的事件，使有限篇幅能够有效呈现中心。
observable_success: 能解释“为什么选这件而不选另一件”，且选择与文章中心存在清晰因果关系。
scope_in: 叙事作文选材、素材迁移、典型事件选择。
scope_out: 具体细节展开；全文语言润色；仅列素材清单。
parent_node_id: null
gaokao_relevance: core
status: active
aliases_zh: [作文选材与典型事件]
source_basis: [学校人物故事会写作任务, 高考写作构思要求]
```

### Language use

```yaml
node_id: CN-A-LAN-revise-sentence-logic
name_zh: 诊断并最小修改句子逻辑或语法问题
node_type: ability
domain: LAN
subdomain: 句子准确性
definition: 判断句子中的成分、搭配、指代或逻辑关系问题，在尽量保留原意的前提下作最小修改。
observable_success: 能指出问题类型，修改后语义、结构和逻辑均成立，且不过度改写。
scope_in: 病句修改、表达准确性、局部改写。
scope_out: 全段重写；文学语言审美；标点专项。
parent_node_id: null
gaokao_relevance: supporting
status: active
aliases_zh: [语病与句子表达]
source_basis: [语言运用题]
```

---

## 12. B1 invariants

A canonical registry is valid only if all invariants hold:

1. Every active node has a stable `CN-*` ID.
2. Every node has exactly one `node_type` and one primary domain.
3. No node contains personal mastery/review/training-state fields.
4. No node's meaning depends on a grade/year label.
5. `parent_node_id` never substitutes for prerequisite semantics.
6. Ability nodes have observable success criteria.
7. Strategy nodes define reusable procedures, not fixed answer wording.
8. Task labels are not promoted to abilities when their only meaning is “this exam asks this way”.
9. Mixed v1 labels must be explicitly split or mapped through multiple v2 entities.
10. Deprecated nodes retain identity and point to replacement guidance; IDs are never reused.
