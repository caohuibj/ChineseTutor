# Canonical Learning Graph taxonomy

Status: **B1 normative draft — reviewed**

This taxonomy supports complete Gaokao knowledge/capability coverage while keeping progression graph-driven rather than grade-driven.

## 1. Taxonomy principles

1. **Domain is navigation, not sequence.** Training order comes from dependencies, learner state, complexity and evidence.
2. **Knowledge, Ability and Strategy are semantic types.** Domain never determines type.
3. **Cross-domain operations generalize.** If the observable operation is the same across text domains, prefer one shared node plus domain-specific prerequisites.
4. **Task Type is not a branch of the Learning Graph.** `人物形象分析题`, `标题作用题`, `文言翻译题` live in the Task graph.
5. **Leaves must be diagnosable.** Split only when doing so changes prerequisite, evidence, intervention, review or recommendation behavior.
6. **Do not atomize for completeness alone.** Canonical graph size should be driven by instructional value.
7. **Taxonomic parent is same-type hierarchy.** Prerequisite and transfer semantics belong to B2 graph edges.
8. **Machine subdomain codes are stable.** Display labels may change without changing stored codes.

---

# 2. Domain assignment precedence

When a candidate could plausibly belong to multiple domains, classify by the semantic operation, not by the question wording.

1. `META` — reasoning operation genuinely reused across domains.
2. `LAN` — language-form/meaning system reused across many tasks.
3. `WRT` — extended written product construction/revision.
4. `COM` — practical, audience-purpose or cross-media communication act.
5. `LIT` — literary/cultural concept independent of one source passage.
6. `MRD` / `CLA` / `POE` — representation-specific reading/decoding.

Examples:

```text
解释证据为何支持人物判断 -> META.evidence
识别条件关系 -> LAN.logic
选择作文典型材料 -> WRT.material
设计采访问题 -> COM.interview
回忆性散文文体特征 -> LIT.genre
根据现代文本证据判断文体 -> MRD.genre
根据语境判断文言实词义 -> CLA.lexicon
诗歌画面还原 -> POE.scene
```

---

# 3. Domain and subdomain taxonomy

## LAN — 语言文字基础

Purpose: language form, meaning, syntax, cohesion and precision that support other domains.

Stable subdomain codes:

```text
LAN.lexicon       词汇与语义
LAN.orthography   字音字形与规范书写
LAN.syntax        句法与成分
LAN.logic         句间逻辑与连接
LAN.cohesion      指代、衔接与连贯
LAN.rhetoric      修辞知识
LAN.precision     准确、简明、得体
LAN.revision      句子诊断与局部修改
```

Representative Knowledge:
- 常见搭配与词义关系；
- 句子成分；
- 因果、条件、转折、递进等关系；
- 常见修辞构成。

Representative Abilities:
- 根据现代语境判断词义；
- 识别句间逻辑；
- 诊断病句并最小修改；
- 压缩表达而不改变必要条件。

---

## MRD — 现代文阅读

Purpose: modern-text comprehension, structure, narrative/literary/informational interpretation.

```text
MRD.information       信息定位、筛选、概括与整合
MRD.structure         段落/篇章关系与结构
MRD.narrative         事件、人物、叙事、视角、时间
MRD.detail            细节及其功能
MRD.language_effect   词句、修辞、表达效果
MRD.theme             情感、主旨、价值
MRD.genre             基于现代文本证据的文体判断
MRD.informational     说明/信息类、多文本、模型重组
MRD.argument          现代文本观点、理由、证据与论证
MRD.interpretation    开放解释、文学评价
```

Cross-domain comparison/evidence-sufficiency abilities belong to `META`, not duplicated here.

---

## CLA — 文言文

Purpose: classical-Chinese lexical/syntactic decoding, translation, discourse and culture.

```text
CLA.lexicon           实词词义网络与语境义
CLA.function_words    虚词功能
CLA.word_change       古今异义、一词多义、词类活用
CLA.syntax            判断/省略/被动/倒装/固定结构
CLA.segmentation      断句
CLA.translation       翻译
CLA.comprehension     事件、关系、内容概括
CLA.narrative         史传/人物/叙事
CLA.argument          诸子/论说/观点与论证
CLA.culture           古代文化制度与常识
CLA.transfer          文言知识迁移到陌生文本
```

`文言人物形象` is not automatically a separate reasoning ability; classical decoding is domain-specific while evidence→character judgment is shared in `META`.

---

## POE — 古诗词

Purpose: poetic language, imagery, structure, technique, emotion and comparison.

```text
POE.form          体式与基本结构
POE.language      炼字、词义、语言密度
POE.image         意象与文化联想
POE.scene         画面、意境、时空组织
POE.technique     修辞、表现手法、写景方式
POE.emotion       情感方向、对象与层次
POE.speaker       抒情主体/人物形象
POE.allusion      用典与文化背景
POE.structure     起承转合、照应、虚实等结构
POE.comparison    诗歌特有的比较解释
POE.memory        背诵与默写
```

Named technique identity is usually Knowledge; explaining concrete local effect is Ability.

---

## LIT — 文学与文化

Purpose: literary/cultural knowledge supporting interpretation, whole-book reading and cultural understanding.

```text
LIT.genre         文体/体裁知识
LIT.author_work   作家作品
LIT.history       文学史与文学传统
LIT.culture       传统文化与重要概念
LIT.whole_book    整本书阅读知识网络
LIT.context       历史文化语境
```

Genre **knowledge** belongs here. Applying that knowledge to judge a modern passage belongs in `MRD.genre`.

---

## WRT — 写作

Purpose: construction, drafting, argumentation, revision and transfer in extended writing.

```text
WRT.prompt            题目/材料/任务约束解析
WRT.idea              中心、立意、核心问题
WRT.material          选材与素材迁移
WRT.narrative         事件、冲突、变化、视角
WRT.detail            场景、人物、细节
WRT.structure         段落/全文组织与过渡
WRT.language          句段表达与风格
WRT.argument          观点、理由、论据、论证
WRT.qualification     反例、边界、条件、复杂思辨
WRT.revision          诊断、重写与版本迭代
WRT.task_adaptation   对象、目的、文体、任务适配
```

Reading→writing relations are graph edges, not duplicated node identities.

---

## COM — 语言运用与真实交流

Purpose: language used to solve concrete communicative, practical and cross-media tasks.

```text
COM.summarization      信息压缩与摘要
COM.transformation     改写、转换、仿写
COM.audience_purpose   对象与目的适配
COM.practical          通知/说明/应用表达
COM.interview          采访与提问设计
COM.speech             演讲/口头表达
COM.discussion         讨论、回应、协商
COM.chart_media        图表与跨媒介信息转换
COM.solution           方案综合与真实任务表达
```

Boundary with `LAN`: LAN owns language-system precision; COM owns accomplishing a concrete communication task.

Boundary with `WRT`: WRT owns extended constructed texts/essays; COM owns practical/situational output where audience/purpose/task constraints dominate.

---

## META — 跨域推理、评价与迁移

Purpose: reusable reasoning operations that should not be cloned inside each content domain.

```text
META.evidence        证据选择、相关性、充分性、证据→结论
META.reasoning       因果、条件、隐含前提、推断
META.comparison      比较维度与异同解释
META.transfer        原理/方法迁移到新对象
META.evaluation      判断解释/观点是否成立
META.modeling        分类、表格化、结构化、模型重组
META.metacognition   自检、策略选择、错误定位
```

Representative shared abilities:

- 解释证据为何支持结论；
- 由事实、情境与选择推断人物特征；
- 判断证据相关性/充分性；
- 建立共同比较维度；
- 把规则迁移到新材料；
- 在多个合理解释中依据证据作判断。

---

# 4. Knowledge vs Ability vs Strategy examples

| domain | Knowledge | Ability | Strategy |
| --- | --- | --- | --- |
| LAN | 因果/条件关系概念 | 识别句间逻辑并修正错接 | 精准表达自检链 |
| MRD/LIT | 回忆性散文特征（LIT） | 根据现代文本证据判断文体（MRD） | 内容—位置—上下文—作用 |
| CLA | 宾语前置 | 识别并调整倒装句序 | 文言翻译五步法 |
| POE | 常见意象文化义 | 解释当前意象如何服务情感 | 景/事/人—关键词—情绪—对象 |
| WRT | 论据/论点概念 | 选择并解释有效论据 | 观点—理由—证据—边界 |
| COM | 语体/对象差异知识 | 按对象目的改写 | 对象—目的—信息—组织—校验 |
| META | 证据相关性概念 | 解释证据为何支持结论 | 证据—解释—结论 |

---

# 5. Cross-domain generalization rules

## Generalize when the observable operation is the same

```text
现代文本人物判断
文言人物判断
          ↓
CN-A-evidence-to-character-judgment
primary domain = META
```

Classical tasks additionally require classical decoding nodes.

## Keep domain-specific when representation differs

```text
根据现代语境判断词义
根据文言语境判断实词义
```

The semantic-inference idea is related, but knowledge networks/error patterns differ enough to justify separate nodes.

## Link reading and writing instead of cloning

```text
分析典型事件的表现力
  supports/transfers_to
选择能集中表现中心的典型材料
```

These operations are related but not identical.

---

# 6. Taxonomic parent rules

A `parent_node_id` is valid only when:

1. parent and child have the same `node_type`;
2. parent and child normally share the same primary domain;
3. the parent is a genuine broader kind/family of the child;
4. the relation does not mean “must master before”.

Cross-domain or prerequisite relations are B2 edges.

---

# 7. Leaf granularity test

Before creating a canonical leaf, ask:

1. Can it be defined stably?
2. Would failure trigger a distinct intervention?
3. Does it need distinct prerequisites?
4. Does it have distinguishable evidence?
5. Does it decay/review differently?
6. Would separating it improve recommendation?

Split only when one or more answers materially change system behavior. This prevents both giant vague skills and hundreds of useless micro-nodes.
