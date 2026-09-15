# LearningNode canonical schema

Status: **B1 normative draft — reviewed**

This document defines the canonical unit of the ChineseTutor Learning Graph. It is a semantic contract, not a Notion-specific database design.

## 1. Core rule

A `LearningNode` describes a stable unit in the learning domain. It must not describe one learner's current state, one question, one source text, one school year, or one study session.

Exactly three semantic node types are canonical:

- `knowledge` — facts, concepts, distinctions, language/literary/cultural rules or conventions that can be known, recognized, recalled or understood;
- `ability` — observable operations that can be performed on language, text, information, an artifact or a communicative situation;
- `strategy` — reusable procedures that coordinate multiple operations and can be prompted, faded, independently invoked, automated and transferred.

If a proposed node mixes these meanings, split it before adding it to the canonical graph.

---

## 2. Stable Node ID

### 2.1 Format

```text
CN-<TYPE>-<SLUG>
```

where:

- `<TYPE>` is `K`, `A`, or `S`;
- `<SLUG>` is a globally unique, short English identifier in kebab-case within that type.

Examples:

```text
CN-K-classical-content-word-meaning
CN-A-infer-classical-contextual-word-sense
CN-A-evidence-to-character-judgment
CN-S-evidence-explanation-conclusion
CN-A-select-typical-writing-material
```

### 2.2 Identity rules

1. `node_id` is independent of Notion page IDs, titles, database names, domains, subdomains, grade labels and source materials.
2. Renaming the Chinese display name does **not** change the ID when semantics remain identical.
3. Moving a node between navigation domains/subdomains does **not** change the ID.
4. If semantic meaning changes materially, create a new ID and deprecate the old node.
5. IDs are never recycled.
6. IDs must remain resolvable after deprecation through `deprecated_by` or migration documentation.

### 2.3 Why domain is not encoded in the ID

`domain` is a mutable coverage/navigation classification. Cross-domain generalization and later taxonomy refinement must not invalidate permanent identity. Machine identity therefore encodes only semantic type plus stable slug.

---

## 3. Canonical domain codes

| code | domain | purpose |
| --- | --- | --- |
| `LAN` | 语言文字基础 | vocabulary, syntax, orthography, cohesion, language precision |
| `MRD` | 现代文阅读 | modern literary/informational text interpretation |
| `CLA` | 文言文 | classical-Chinese decoding, discourse and culture |
| `POE` | 古诗词 | poetic language, imagery, structure, technique and emotion |
| `LIT` | 文学与文化 | genre, authors/works, literary/cultural knowledge, whole-book reading |
| `WRT` | 写作 | narrative, argumentative, source-based and revision production |
| `COM` | 语言运用与真实交流 | practical, audience-purpose and cross-media communication |
| `META` | 跨域推理、评价与迁移 | reasoning operations reusable across content domains |

Domain is **coverage/navigation**, never progression.

### 3.1 Domain-assignment precedence

Assign by the semantic object, not by wording of the question that happens to test it.

1. **Cross-domain reasoning operation → `META`.** Example: explaining why evidence supports a conclusion, establishing a comparison dimension, judging evidence sufficiency.
2. **Language-form system reusable across tasks → `LAN`.** Example: syntax, cohesion, sentence logic, lexical precision.
3. **Extended composition/product construction → `WRT`.** Example: narrative material selection, argument paragraph construction, revision of an essay draft.
4. **Concrete audience-purpose/practical or cross-media act → `COM`.** Example: interview-question design, notice rewriting, chart-to-text conversion.
5. **Literary/cultural concept independent of one modern passage → `LIT`.** Example: memoir genre features, literary history, whole-book knowledge.
6. **Representation-specific reading/decoding → source domain (`MRD`, `CLA`, `POE`).** Example: classical contextual word sense, poetry scene reconstruction, modern narrative time structure.

If two domains remain plausible, choose the domain that best predicts prerequisites and intervention. Record cross-cutting relevance in `secondary_domains` or later graph edges; do not duplicate identity.

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
Short canonical Chinese display name. It describes the semantic unit, not an exam label.

#### `node_type`
Knowledge / Ability / Strategy. This is the primary semantic distinction.

#### `domain`
Primary coverage/navigation domain. It does not constrain transfer.

#### `subdomain`
Stable machine code using `<DOMAIN>.<family>` form, e.g.:

```text
META.evidence
MRD.narrative
CLA.lexicon
POE.emotion
WRT.material
COM.interview
```

Chinese labels and explanations belong in taxonomy documentation/views, not in the machine code.

#### `definition`
Self-contained semantic definition sufficient for consistent tagging by independent editors.

#### `observable_success`
What successful evidence looks like.

- Knowledge: correct recognition, recall, distinction or explanation.
- Ability: an observable operation/output.
- Strategy: correct selection and execution of the procedure, not merely a correct final answer.

#### `scope_in` / `scope_out`
Explicit semantic boundary and nearby exclusions.

#### `parent_node_id`
Taxonomic decomposition only, never prerequisite semantics.

A taxonomic parent must:

- have the **same `node_type`**;
- normally have the **same primary domain**;
- represent a broader kind/family of the child.

Cross-type, cross-domain, prerequisite and transfer relations belong in B2 graph edges.

#### `gaokao_relevance`
- `core` — repeatedly required directly or is a central prerequisite;
- `supporting` — materially supports core performance;
- `enrichment` — broader literacy beyond baseline coverage.

This field never determines training order by itself.

#### `status`
- `draft` — semantics under review;
- `active` — canonical and available for mapping/profile;
- `deprecated` — identity retained; new tagging should use successor guidance.

#### `aliases_zh`
Only one-to-one semantic synonyms or common equivalent terminology.

Do **not** use an alias to absorb a mixed legacy label that splits into multiple canonical nodes. Those mappings belong in migration documentation.

#### `source_basis`
Coverage evidence such as curriculum standards, authentic exams, school materials or repeated operational need. This is not learner evidence.

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

---

## 6. Fields forbidden from canonical LearningNode

```text
mastery
automation
complexity_ceiling
student_score
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

- personal state → `LearnerNodeState`;
- performance evidence → `TrainingAttempt`;
- task/source metadata → `Material`, `Question`, `TaskType`;
- scheduling → `TrainingMove` / training queue.

---

## 7. Node-type decision rules

### 7.1 Knowledge

Use `knowledge` when the primary question is:

> What stable fact, concept, distinction, convention or rule must the learner know/recognize/understand?

Good:

```text
比喻的构成
宾语前置
古今异义
回忆性散文的文体特征
论据与论点的关系
```

Bad as Knowledge:

```text
分析比喻表达效果       # ability
准确翻译文言句子       # ability
辨-定-证-推-答-检      # strategy
```

### 7.2 Ability

Use `ability` when the primary question is:

> What can the learner observably do with language, text, information or an artifact?

Prefer `动词 + 对象 + 成功约束`.

Good:

```text
定位与题目相关的关键信息
压缩信息且保持必要条件
建立有效比较维度
解释证据为何支持人物判断
根据语境推断文言实词义
准确翻译文言句子
根据现代文本证据判断文体
选择能集中表现中心的典型材料
```

Composite abilities may exist as reporting outcomes, but canonical leaves must remain sufficiently diagnosable.

### 7.3 Strategy

Use `strategy` when the primary question is:

> What reusable ordered procedure coordinates multiple operations across questions/materials?

A Strategy must:

1. contain at least two meaningful operations/decisions;
2. apply to multiple concrete questions/materials;
3. be explicitly promptable and fadeable;
4. be independently invokable/automatable;
5. not be a fixed answer sentence template.

Good:

```text
证据 → 解释 → 结论
比较维度 → 分别取证 → 同异 → 意义
内容 → 位置 → 上下文关系 → 作用
圈关键词 → 逐词落实 → 调整句式 → 补省略 → 通读校验
```

---

## 8. Split, merge and generalization rules

### Split when

- one label mixes Knowledge and Ability (`修辞与表达效果`);
- one label hides different knowledge families/prerequisites (`词类活用与特殊句式`);
- one label is mainly a Task Type (`标题含义与作用`);
- evidence/intervention/review behavior differs materially (`作文语言与修改`).

### Generalize when

- the observable operation is semantically identical across source domains;
- shared identity improves transfer while source-specific prerequisites remain explicit.

Example:

```text
现代人物判断
文言人物判断
        ↓
CN-A-evidence-to-character-judgment   domain=META
```

Modern/classical decoding remains separate prerequisite context.

### Do not merge when

- representation-specific knowledge differs materially;
- error patterns/interventions differ;
- one merged score would hide the actionable failure.

---

## 9. Parent hierarchy rules

`parent_node_id` answers:

> This node is a kind/part of what broader node of the same semantic type?

Valid:

```text
Knowledge: 宾语前置 → 文言倒装句知识
Ability: 选择语境义 → 文言词义解码能力族
```

Invalid:

```text
文言翻译 parent=文言实词
```

That is a prerequisite relation and belongs in B2.

B1 uses at most one canonical taxonomic parent. Cross-cutting relations use graph edges.

---

## 10. Naming convention

### Knowledge
Prefer stable concept/distinction nouns.

### Ability
Prefer action + object + success constraint.

### Strategy
Prefer a short conceptual name; keep procedural steps in `definition`.

Avoid:

- grade labels (`初二人物形象`);
- source labels (`周亚夫人物分析`);
- score labels (`中考8分题`);
- broad containers as leaf nodes (`现代文阅读`);
- question wording as identity (`为什么选这件事`);
- vague labels (`阅读理解能力`).

---

## 11. Representative reviewed examples

### Cross-domain character reasoning

```yaml
node_id: CN-A-evidence-to-character-judgment
name_zh: 由事实、情境与选择推断人物特征
node_type: ability
domain: META
subdomain: META.evidence
definition: 从人物言行、事件与所处情境出发，解释关键选择为何足以支持某项相对稳定的人物特征判断。
observable_success: 在现代文或文言陌生材料中，能给出事实证据并显性完成证据到人物判断之间的推理。
scope_in: 人物形象、人物评价、典型事件中的人格判断。
scope_out: 文言词句解码；仅概括事件；无证据价值判断。
parent_node_id: null
gaokao_relevance: core
status: draft
aliases_zh: []
source_basis: [现有学校人物叙事单元, 周亚夫军细柳训练记录, 文学阅读真题]
```

### Genre knowledge

```yaml
node_id: CN-K-memoir-genre-features
name_zh: 回忆性散文的文体特征
node_type: knowledge
domain: LIT
subdomain: LIT.genre
definition: 回忆性散文在视角、材料组织、叙事与抒情关系、写作目的等方面的稳定体裁特征。
observable_success: 能说明主要特征并与传记、小说等相邻体裁作概念区分。
scope_in: 体裁稳定特征。
scope_out: 对某篇现代文本作最终文体判断。
parent_node_id: null
gaokao_relevance: supporting
status: draft
aliases_zh: [回忆性散文特征]
source_basis: [现有人物叙事学习设计]
```

### Evidence-based modern genre judgment

```yaml
node_id: CN-A-judge-modern-genre-from-evidence
name_zh: 根据现代文本证据判断文体
node_type: ability
domain: MRD
subdomain: MRD.genre
definition: 调用体裁知识，从叙事视角、材料组织、表达方式与写作目的等证据判断现代文本的文体属性并说明理由。
observable_success: 面对陌生现代文本时，能给出相关证据并解释其与文体特征的对应关系。
scope_in: 回忆性散文、传记、小说等现代文本辨析。
scope_out: 仅背体裁定义；文学史记忆。
parent_node_id: null
gaokao_relevance: supporting
status: draft
aliases_zh: [文体辨析]
source_basis: [现有人物叙事学习设计]
```

### Classical contextual word sense

```yaml
node_id: CN-A-infer-classical-contextual-word-sense
name_zh: 根据语境判断文言词语义项
node_type: ability
domain: CLA
subdomain: CLA.lexicon
definition: 在词义知识网络基础上，根据搭配、句法和上下文事件关系选择或推断当前文言语境中的合理义项。
observable_success: 在陌生短篇中能给出与句意一致的词义判断，并在需要时说明语境依据。
scope_in: 实词语境义、一词多义义项选择、部分活用后语义理解。
scope_out: 单纯背诵词条；虚词功能；完整句子翻译。
parent_node_id: null
gaokao_relevance: core
status: draft
aliases_zh: [文言实词语境义]
source_basis: [现有文言教材与课外迁移设计]
```

### Writing material selection

```yaml
node_id: CN-A-select-typical-writing-material
name_zh: 选择能集中表现中心的典型材料
node_type: ability
domain: WRT
subdomain: WRT.material
definition: 根据题目限制和中心问题，在候选经历或材料中选择最能通过情境压力、关键选择或辨识度服务中心的内容。
observable_success: 能比较多个候选素材并解释最终选择为什么更能服务中心。
scope_in: 叙事作文选材、素材迁移、典型事件选择。
scope_out: 细节展开；全文结构；单纯列素材。
parent_node_id: null
gaokao_relevance: core
status: draft
aliases_zh: []
source_basis: [人物故事会写作任务, 周亚夫典型事件训练]
```

---

## 12. B1 invariants

A canonical registry is valid only if:

1. every active node has stable `CN-<TYPE>-<SLUG>` identity;
2. domain/subdomain changes do not change identity;
3. every node has exactly one semantic type and one primary domain;
4. `subdomain` uses a stable machine code;
5. no personal learning state leaks into canonical nodes;
6. no node meaning depends on grade/year;
7. taxonomic parent has the same node type and normally the same primary domain;
8. Ability nodes have observable success criteria;
9. Strategy nodes are reusable procedures, not answer wording;
10. aliases are one-to-one synonyms, never a shortcut for split legacy labels;
11. Task Type labels are not promoted to Abilities merely because exams use them;
12. mixed v1 labels have explicit split/merge/migration decisions;
13. deprecated IDs are never reused.
