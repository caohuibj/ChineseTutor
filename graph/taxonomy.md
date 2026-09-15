# Canonical Learning Graph taxonomy

Status: **B1 normative draft**

This taxonomy supports complete Gaokao knowledge/capability coverage while keeping progression graph-driven rather than grade-driven.

## 1. Taxonomy principles

1. **Domain is navigation, not sequence.** A learner may train any node whose prerequisites and load are appropriate.
2. **Knowledge, Ability and Strategy are orthogonal semantic types.** Domain does not determine node type.
3. **Cross-domain abilities should be generalized when the observable operation is the same.** Domain-specific decoding remains as prerequisites.
4. **A task type is not a domain branch.** `人物形象分析题`, `标题作用题`, `文言翻译题` belong to the Task graph.
5. **Canonical leaves should be diagnosable.** If a node is too broad to explain why performance failed, decompose it.
6. **Do not atomize without value.** A distinction deserves its own node when it changes prerequisite, evidence, review, recommendation or intervention behavior.

---

# 2. Domain taxonomy

## LAN — 语言文字基础

Purpose: language form, meaning, syntax, precision and basic logical/cohesive knowledge/operations that support every other domain.

Recommended subdomains:

```text
LAN.lexicon             词汇与语义
LAN.orthography         字音字形与规范书写
LAN.syntax              句法与成分
LAN.logic               句间逻辑与连接
LAN.cohesion            指代、衔接与连贯
LAN.rhetoric            修辞知识
LAN.precision           准确、简明、得体
LAN.revision            句子诊断与局部修改
```

Representative Knowledge:

- 词语本义/语境义基本概念;
- 常见搭配;
- 句子成分;
- 因果、条件、转折、递进等关系;
- 常见修辞构成.

Representative Abilities:

- 根据语境判断词义;
- 识别句间逻辑关系;
- 诊断病句并最小修改;
- 压缩表达而不改变必要条件;
- 根据对象和目的调整语体.

---

## MRD — 现代文阅读

Purpose: comprehension, structure, inference, literary/informational interpretation for modern Chinese texts.

Recommended subdomains:

```text
MRD.information         信息定位、筛选、概括与整合
MRD.structure           段落/篇章关系与结构
MRD.narrative           事件、人物、叙事、视角、时间
MRD.detail              细节及其功能
MRD.language_effect     词句/修辞/表达效果
MRD.theme               情感、主旨、价值判断
MRD.genre               文体与体裁判断
MRD.informational       说明/信息类、多文本、模型重组
MRD.argument            观点、理由、证据、论证与评价
MRD.interpretation      开放解释、比较、评价
```

Representative cross-domain abilities may live in `META` rather than be duplicated here, e.g. evidence sufficiency or general comparison.

---

## CLA — 文言文

Purpose: classical-language decoding plus discourse, narrative, argument and cultural interpretation.

Recommended subdomains:

```text
CLA.lexicon             实词词义网络与语境义
CLA.function_words      虚词功能
CLA.word_change         古今异义、一词多义、词类活用
CLA.syntax              判断/省略/被动/倒装/固定结构
CLA.segmentation        断句
CLA.translation         翻译
CLA.comprehension       事件、关系、内容概括
CLA.narrative           史传/人物/叙事
CLA.argument            诸子/论说/观点与论证
CLA.culture             古代文化制度与常识
CLA.transfer            课内知识调用到陌生材料
```

Rule: `文言人物形象` is not automatically a separate reasoning ability from modern narrative. Classical decoding is domain-specific; evidence-to-character inference may be shared through `META`/MRD reasoning nodes.

---

## POE — 古诗词

Purpose: poetic language, imagery, structure, technique, emotion and comparison.

Recommended subdomains:

```text
POE.form                体式与基本结构
POE.language            炼字、词义、语言密度
POE.image               意象与文化联想
POE.scene               画面、意境、时空组织
POE.technique           修辞、表现手法、写景方式
POE.emotion             情感方向、对象与层次
POE.speaker             抒情主体/人物形象
POE.allusion             用典与文化背景
POE.structure           起承转合、照应、虚实等结构
POE.comparison          诗歌比较与同意象异功能
POE.memory              背诵与默写
```

Do not make every named technique an Ability. Technique identity is often Knowledge; explaining its concrete effect is Ability.

---

## LIT — 文学与文化

Purpose: literary/cultural knowledge that supports interpretation, whole-book reading and cultural understanding.

Recommended subdomains:

```text
LIT.genre               文体/体裁知识
LIT.author_work         作家作品
LIT.history             文学史与文学传统
LIT.culture             传统文化与重要概念
LIT.whole_book          整本书阅读
LIT.context             历史文化语境
```

This domain is currently underrepresented in v1 and will expand after B1.

---

## WRT — 写作

Purpose: construction, drafting, argumentation, revision and transfer in extended or short writing.

Recommended subdomains:

```text
WRT.prompt              题目/材料/任务约束解析
WRT.idea                中心、立意、核心问题
WRT.material            选材与素材迁移
WRT.narrative           事件、冲突、变化、视角
WRT.detail              场景、人物、细节
WRT.structure           段落/全文组织与过渡
WRT.language            句段表达与风格
WRT.argument            观点、理由、论据、论证
WRT.qualification       反例、边界、条件、复杂思辨
WRT.revision            诊断、重写与版本迭代
WRT.task_adaptation     对象、目的、文体、任务适配
```

Reading→writing transfer should be represented through graph edges, not duplicate names.

---

## COM — 语言运用与真实交流

Purpose: use language to solve concrete communicative, practical and cross-media tasks.

Recommended subdomains:

```text
COM.summarization       信息压缩与摘要
COM.transformation      改写、转换、仿写
COM.audience_purpose    对象与目的适配
COM.practical           通知/说明/应用表达
COM.interview           采访与提问设计
COM.speech              演讲/口头表达
COM.discussion          讨论、回应、协商
COM.chart_media         图表与跨媒介信息转换
COM.solution            方案综合与真实任务表达
```

---

## META — 跨域推理、评价与迁移

Purpose: reusable operations that should not be duplicated in each text domain.

Recommended subdomains:

```text
META.evidence           证据选择、相关性、充分性
META.reasoning          因果、条件、隐含前提、推断
META.comparison         比较维度与异同解释
META.transfer           原理/方法迁移到新对象
META.evaluation         判断解释/观点是否成立
META.modeling           分类、表格化、结构化、模型重组
META.metacognition      自检、策略选择、错误定位
```

Examples of candidate shared abilities:

- 解释证据为何支持结论;
- 判断证据是否充分;
- 建立共同比较维度;
- 把规则迁移到新材料;
- 从结果反推条件/问题;
- 在多个合理解释中依据证据作判断.

---

# 3. Knowledge vs Ability vs Strategy examples by domain

| domain | Knowledge | Ability | Strategy |
| --- | --- | --- | --- |
| LAN | 因果/条件关系概念 | 识别句间逻辑并修正错接 | 精准表达自检链 |
| MRD | 传记/回忆性散文特征 | 用文本证据判断文体 | 证据—解释—结论 |
| CLA | 宾语前置 | 识别并调整倒装句序 | 文言翻译五步法 |
| POE | 常见意象文化义 | 解释当前意象如何服务情感 | 景/事/人—关键词—情绪—对象 |
| WRT | 论据/论点概念 | 选择并解释有效论据 | 观点—理由—证据—边界 |
| COM | 语体/对象差异知识 | 按对象目的改写 | 对象—目的—信息—组织—校验 |

---

# 4. Cross-domain generalization rules

## Generalize when operation is the same

Example:

```text
现代文人物形象
文言人物形象
```

Both can use the shared operation:

```text
由事实 + 情境 + 选择 -> 推断人物稳定特征
```

Classical Chinese tasks additionally require decoding nodes.

## Keep domain-specific when representation differs

Example:

```text
根据语境判断现代词语义
根据语境判断文言实词义
```

They share semantic inference ideas but have materially different knowledge networks and error patterns; a premature merge would reduce diagnostic precision.

## Link reading and writing instead of cloning

Example:

```text
分析典型事件的表现力
  transfers_to/supports
选择能集中表现中心的典型材料
```

They are related but not the same operation: one is analysis of an existing artifact; the other is production choice.

---

# 5. Leaf-node granularity test

Before creating a leaf, ask:

1. Can it be described with one stable definition?
2. Would failure on it trigger a distinct intervention?
3. Does it have distinguishable evidence?
4. Does it have different prerequisites from neighboring nodes?
5. Would separating it improve recommendation or review scheduling?

If mostly **no**, keep it grouped.

If several are **yes**, split it.

This test prevents both giant vague skills and hundreds of useless micro-nodes.
