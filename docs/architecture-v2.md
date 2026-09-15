# ChineseTutor v2 architecture

## Product definition

ChineseTutor is an adaptive Chinese-language learning system whose coverage boundary is the full knowledge and capability space required for Gaokao.

It does **not** organize progression primarily by grade. Grade/year is retained only as source metadata, curriculum relevance and load-control context.

The product loop is:

```text
Gaokao coverage boundary
        |
Canonical Learning Graph
(Knowledge + Ability + Strategy + Dependencies)
        |
Learner Profile overlay
        |
Find highest-value current gap
        |
Choose a Training Move
(target + task type + complexity + scaffold)
        |
Choose authentic Material + Question
        |
First Attempt
        |
Failure diagnosis
        |
Minimal effective intervention
        |
Second Attempt / transfer task
        |
Attempt evidence
        |
Profile update
        +-----------------------> repeat
```

## Architectural principles

### P1. Graph-first, not grade-first

The next learning target is selected from dependency structure and learner state. Grade never acts as the primary prerequisite rule.

### P2. Canonical entities are separate from learner state

A capability such as `人物形象分析` has one definition. A learner's mastery of that capability is a separate state record.

Likewise, a question is a reusable static task. Whether it is pending, completed, failed or stable belongs to learner-specific training records.

### P3. Attempts are the source of truth for learning evidence

Mastery is not upgraded because an instructor feels that a learner "seems to understand". It is inferred from a sequence of attempt evidence across materials, task types, complexity and hint levels.

### P4. Authentic task types are training environments

A task type does not equal an ability. The same ability must transfer across task types, and one task may exercise multiple nodes.

### P5. Tutoring should minimize intervention

The tutor should provide the smallest scaffold that unlocks the missing reasoning step. Reduced hint dependence is itself evidence of learning.

### P6. Coverage must be auditable

Recent Gaokao questions should be mappable to canonical nodes, task types and complexity dimensions. Unmappable stable demands reveal graph gaps.

### P7. Learner-facing strategy count should stay small

The internal graph may contain many knowledge and ability nodes, but the learner should gradually automate a compact set of reusable reasoning strategies.

---

# 1. Canonical Learning Graph

## 1.1 Unified node registry

Use one canonical `LearningNode` registry rather than unrelated knowledge/ability tables. Nodes are distinguished by `node_type`.

### Node types

- `knowledge` - facts, concepts, language rules, literary/cultural knowledge to know;
- `ability` - observable operations a learner can perform;
- `strategy` - reusable reasoning/production algorithms that can be invoked across tasks.

This preserves conceptual separation while making dependencies and profile overlays uniform.

### Core node fields

```yaml
node_id: CN-...
name: string
node_type: knowledge | ability | strategy
domain: string
subdomain: string
parent_node: Node?
definition: string
observable_success: string
gaokao_relevance: core | supporting | enrichment
coverage_evidence: reference[]
status: active | draft | deprecated
```

### Example knowledge nodes

```text
文言实词常用义
古今异义
宾语前置
传记文体特征
回忆性散文视角
比喻
借景抒情
小说叙事视角
论据类型
```

### Example ability nodes

```text
定位关键信息
压缩信息且不失真
建立比较维度
解释证据为什么支持结论
判断证据充分性
由特殊情境和关键选择推人物品质
分析结构关系
准确翻译文言句子
解释意象与情感关系
构建完整论证段
```

### Example strategy nodes

```text
辨-定-证-推-答-检
证据-解释-结论
比较维度-分别取证-同异-意义
内容-位置-上下文-作用
景/事/人-关键词-情绪-对象
圈关键词-逐词落实-调句式-补省略-通读
观点-理由-证据-边界
```

## 1.2 Domains

Initial canonical domains:

```text
LANGUAGE          语言文字基础
MODERN_READING    现代文阅读
CLASSICAL         文言文
POETRY            古诗词
LITERATURE        文学与文化
WRITING           写作
COMMUNICATION     语言运用与真实交流
META              评价、迁移与反思
```

Domains are navigation views, not progression levels.

## 1.3 Dependency edges

The graph must support multiple edge semantics.

```text
requires       hard prerequisite
supports       soft prerequisite / useful support
part_of        decomposition hierarchy
strategy_for   strategy applies to ability/task
transfers_to   learned operation naturally transfers to another node
contrasts_with commonly confused distinction
```

A future implementation may store edges in a separate graph-edge table for analytics. In Notion v2, hard prerequisites should at minimum be represented explicitly rather than buried in prose.

### Example

```text
文言实词语境义
  -> requires -> 基础词义积累

准确翻译文言句子
  -> requires -> 文言实词语境义
  -> requires -> 特殊句式识别

文言内容概括
  -> requires -> 准确句意理解

文言人物评价
  -> requires -> 文言内容概括
  -> requires -> 证据-解释-结论
```

---

# 2. Material and Task model

## 2.1 Material

A Material is the language object being read, heard, viewed or used.

Examples:

- one essay;
- one classical passage;
- one poem;
- a pair of poems;
- a multi-text source set;
- a chart plus explanatory text;
- a writing prompt/source packet;
- a whole-book chapter or excerpt.

### Fields

```yaml
material_id: MAT-...
title: string
material_type: prose | novel | biography | classical | poem | multi_text | chart | prompt | ...
author: string?
era: string?
source_type: textbook | school | district_exam | zhongkao | gaokao | authoritative_simulation | authentic_external
source_year: int?
source_region: string?
source_grade: string?      # metadata only
source_url: url?
source_reliability: official | authority | school | reputable_reprint | other
text_complete: boolean
rights_note: string?
```

## 2.2 Task Type

Task Type is the reusable authentic problem form.

Examples:

- 人物形象分析;
- 多文本信息整合;
- 文言翻译;
- 诗歌炼字;
- 证据充分性评价;
- 作文选材;
- 情境改写.

Existing 题型地图 content maps naturally here.

### Task Type fields

```yaml
task_type_id: TT-...
name: string
domain: string
recognition_signals: string
core_question: string
recommended_strategies: LearningNode[strategy][]
output_skeleton: string
self_checks: string
boundaries: string
default_target_nodes: LearningNode[]
```

## 2.3 Question

A Question is a concrete prompt attached to a Material.

```yaml
question_id: Q-...
material: Material
task_type: TaskType
prompt: string
reference_answer: string?
rubric: string?
official_score: number?
knowledge_nodes: LearningNode[]
ability_nodes: LearningNode[]
strategy_nodes: LearningNode[]
complexity: ComplexityVector
source_question_number: string?
```

A single Material may have many Questions. A single Question may target several nodes.

---

# 3. Complexity model

A single label such as `基础/常规/提升/迁移` is insufficient because difficulty can come from different sources.

Represent task complexity as a vector and optionally derive an overall band.

## Dimensions

Each dimension can initially use 0-3.

```text
text_load          length / syntax / conceptual density
information_hidden explicit -> implicit
reasoning_depth    direct -> multi-step -> evaluative
material_count     single -> multiple/cross-media
knowledge_distance directly learned -> remote transfer
response_openness  fixed -> bounded open -> multi-valid
expression_load    short phrase -> structured paragraph -> extended composition
time_pressure      untimed -> normal -> high-pressure
```

## Overall band

For user-facing simplicity:

```text
C0 recognition
C1 single-step application
C2 multi-step explanation
C3 integrated multi-node task
C4 unfamiliar transfer
C5 open evaluation / complex construction
```

The vector is canonical. `C0-C5` is a summary, not a replacement.

---

# 4. Learner Profile

## 4.1 LearnerNodeState

The profile is a dynamic overlay on every relevant canonical node.

```yaml
learner_id: string
node: LearningNode
mastery: M0 | M1 | M2 | M3
automation: A0 | A1 | A2 | A3
complexity_ceiling: C0..C5
status: stable | developing | bottleneck | review_due | unknown
evidence_strength: 0..1
attempt_count: int
independent_success_count: int
transfer_success_count: int
last_trained_at: datetime?
last_verified_at: datetime?
forgetting_risk: low | medium | high
primary_error_pattern: string?
priority_score: number?
```

### Mastery

```text
M0 not established
M1 completes with meaningful support
M2 independently completes routine/authentic tasks
M3 stable transfer across unfamiliar materials
```

### Automation

```text
A0 does not know what to invoke
A1 invokes after reminder
A2 independently recognizes and invokes
A3 invokes reliably under time pressure
```

## 4.2 Evidence strength

A profile state must expose confidence, not merely mastery.

Evidence strength increases with:

- number of independent attempts;
- diversity of materials;
- diversity of task types;
- unfamiliar transfer;
- recency;
- low hint dependence.

It decreases with:

- only one example;
- repeated near-identical questions;
- old evidence;
- heavy scaffolding;
- contradictory recent results.

This prevents one successful question from becoming false `M3` mastery.

---

# 5. Attempt event model

`TrainingAttempt` is the main learning telemetry event.

## Required fields

```yaml
attempt_id: ATT-...
learner_id: string
question: Question
training_move: TrainingMove?
attempt_number: 1 | 2 | 3...
started_at: datetime?
completed_at: datetime
hint_level: H0..H7
independent_task_type_recognition: 0..2
text_location: 0..2
evidence_selection: 0..2
reasoning: 0..2
terminology: 0..2
written_expression: 0..2
answer_correctness: 0..2
error_codes: [K,R,I,E,Q,M,C]
raw_answer: text?
feedback_summary: text?
next_attempt_delta: text?
```

The dimensions are deliberately small. The objective is useful evidence with low recording cost.

## Hint scale

```text
H0 no hint
H1 task-type reminder
H2 strategy reminder
H3 one key guiding question
H4 text range/location supplied
H5 critical evidence supplied
H6 partial reasoning supplied
H7 near-answer / model answer
```

A learner moving from H5 to H2 on comparable complexity is meaningful improvement even if both final answers are correct.

---

# 6. Training Move and Recommendation model

The recommendation engine should recommend more than a question. It recommends a **Training Move**.

```yaml
move_id: MOVE-...
target_nodes: LearningNode[]
move_type: learn | scaffolded_practice | fade_scaffold | timed_automation | unfamiliar_transfer | review
preferred_task_types: TaskType[]
complexity_target: ComplexityVector or band
max_hint_level: H0..H7
success_criterion: string
selection_reason: string
priority_score: number
```

Examples:

```text
Target: 对比衬托
State: M1/A1/C2
Observed gap: sees difference but cannot explain how it highlights the main character
Move: fade scaffold
Task: unfamiliar narrative text, comparison prompt
Constraint: do not provide the effect formula initially
Success: two independent answers containing comparison dimension -> difference -> highlighted trait
```

## Recommendation priority

First implementation should be rule-based and explainable.

Candidate priority factors:

```text
mastery gap
x node importance / Gaokao centrality
x dependency unlock value
x evidence uncertainty
x transfer gap
x forgetting risk
x current-material relevance
x suitable-question availability
```

The engine should not always select the lowest-scoring node. A moderately weak high-centrality prerequisite may be the highest-value target.

---

# 7. Tutoring policy

## 7.1 Default interaction

```text
First attempt
  -> identify the earliest failing layer
  -> acknowledge what is already valid
  -> give one minimal intervention
  -> second attempt
  -> normalize expression only after reasoning is valid
```

## 7.2 Failure-layer order

A useful diagnostic order is:

```text
Q: understood the question?
R: located relevant text?
K: has required knowledge?
I: reasoning relation valid?
M: knows which strategy to invoke?
E: can convert into written answer?
C: execution/carelessness issue?
```

The tutor should repair the earliest causal failure rather than polishing downstream wording.

## 7.3 Minimal Effective Intervention

Tutor quality should be evaluated partly by how little support is needed to create a correct second attempt.

The system should avoid:

- explaining an entire question when only one inferential link is missing;
- repeating a strategy already automated;
- continuing near-identical tasks after transfer is demonstrated;
- treating expression failure as comprehension failure.

---

# 8. Summary and derived layers

## Session Summary

A durable recap promoted from multiple attempts or a meaningful learning event.

Keep:

- original reasoning;
- breakthrough;
- stable gap;
- reusable strategy;
- transfer result;
- recommendation.

Do not use it as raw event storage.

## Error Pattern

A repeated failure mode promoted from multiple attempts.

Examples:

- `E: conclusion/evidence present but inferential sentence omitted`;
- `R: searches whole text instead of task-specified range`;
- `K: repeatedly confuses 之 as pronoun/structural particle in similar contexts`.

One-off wrong answers stay in Attempt events.

## Learning Note / Method Card

Human-readable notes generated from canonical nodes and actual learning breakthroughs.

They are pedagogical artifacts, not the canonical graph source of truth.

---

# 9. Gaokao coverage validation

The architecture is complete only if recent authentic examinations can be mapped into it.

For every selected Gaokao question, record:

```text
source exam
material type
task type
knowledge nodes
ability nodes
strategy nodes
complexity vector
expected evidence chain
```

Coverage tests:

1. **Node coverage** - every stable examination demand maps to existing nodes.
2. **Task coverage** - every recurring task environment maps to a Task Type.
3. **Dependency plausibility** - required subskills exist as prerequisites.
4. **Training availability** - important nodes have tasks across increasing complexity.
5. **Transfer coverage** - central abilities appear in more than one task family.

Any recurring unmapped demand becomes a graph-design issue, not an ad-hoc new answer template.

---

# 10. Source-of-truth boundaries

## GitHub

Canonical specifications, schemas, graph definitions, architecture decisions, migration history and QA rules.

## Notion

Operational learning workspace: live graph mirror, materials/questions, learner profile, attempts, recommendations, summaries and human-readable views.

## Authentic source files

Primary learning content and evidence. They should remain traceable to the source and should not be silently rewritten.

The same semantic entity should not have two competing authoritative definitions.
