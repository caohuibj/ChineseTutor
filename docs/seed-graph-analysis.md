# Seed graph analysis from existing ChineseTutor assets

## Purpose

This document turns the current workspace and uploaded learning materials into a concrete seed plan for the v2 Learning Graph. It is not yet the final Gaokao graph. Its purpose is to distinguish what can be preserved directly, what must be split semantically, and what major branches are still missing.

---

# 1. Existing 42-node inventory: migration interpretation

The v1 node set is a strong **training-topic inventory**, but several rows combine knowledge and ability semantics. The table below describes the intended v2 treatment.

## Modern reading

| v1 node | v2 interpretation | migration action |
| --- | --- | --- |
| 信息提取与概括 | ability | keep; later decompose into locate / select / classify / compress |
| 人物形象分析 | ability | keep as composite outcome; require evidence-to-trait subskills |
| 典型事件与选材 | ability | keep; link to writing transfer and narrative knowledge |
| 细节描写分析 | ability | keep; link to characterization and effect reasoning |
| 词句赏析 | ability | keep as task-facing composite; link to lexical precision + style/effect strategies |
| 修辞与表达效果 | mixed knowledge+ability | split: rhetorical-device knowledge + concrete-effect explanation ability |
| 表现手法 | mixed knowledge+ability | split into technique knowledge and effect-analysis ability |
| 对比与衬托 | mixed knowledge+ability | keep technique knowledge; create comparison/effect ability relation |
| 结构与段落作用 | ability | keep; decompose content-position-context-function reasoning |
| 标题含义与作用 | task-facing composite | keep mainly as Task Type; underlying abilities are semantic inference + structure/theme integration |
| 作者情感与主旨 | ability | keep; decompose evidence aggregation + value/theme inference |
| 文体辨析 | mixed knowledge+ability | split genre-feature knowledge + evidence-based genre judgment |
| 开放评价与迁移 | ability family | split into evidence evaluation / transfer / bounded judgment |

## Classical Chinese

| v1 node | v2 interpretation | migration action |
| --- | --- | --- |
| 文言实词 | mixed knowledge+ability | split accumulated meanings from contextual sense inference |
| 文言虚词 | mixed knowledge+ability | split function knowledge from contextual parsing |
| 古今异义与一词多义 | knowledge family | split into separate node families |
| 词类活用与特殊句式 | knowledge family | split; special sentence types need finer nodes |
| 文言断句 | ability | keep; prerequisites: meaning + syntax + markers + symmetry |
| 文言翻译 | ability | keep; link to translation strategy mother model |
| 文言内容理解 | ability | keep; prerequisites from word/sentence decoding |
| 文言人物形象 | ability | likely merge with cross-domain character reasoning plus classical decoding prerequisites |
| 文言写作手法 | mixed | map technique knowledge to general literary strategy; keep classical-specific evidence where needed |
| 文言主旨 | ability | likely share theme/evidence reasoning with modern reading |
| 文言课内外迁移 | meta ability | keep as transfer evidence dimension rather than a single isolated skill |

## Poetry

| v1 node | v2 interpretation | migration action |
| --- | --- | --- |
| 诗词画面还原 | ability | keep |
| 意象与意境 | mixed knowledge+ability | split image convention/knowledge from atmosphere construction ability |
| 炼字 | task-facing ability | keep Task Type; underlying lexical-semantic/effect abilities link to strategy |
| 诗词修辞 | mixed | split knowledge + effect reasoning |
| 诗词表现手法 | mixed | split knowledge + analysis ability |
| 写景角度与情景关系 | mixed | split technique knowledge + relation analysis |
| 诗词思想感情 | ability | keep; use evidence chain |
| 诗词比较阅读 | ability family | decompose comparison-dimension + separate evidence + difference significance |
| 背诵与默写 | knowledge retention/performance | keep as memory-verification node family, with forgetting model different from reasoning skills |

## Writing

| v1 node | v2 interpretation | migration action |
| --- | --- | --- |
| 作文审题与立意 | ability | keep; later separate constraint parsing from central-question construction |
| 作文选材与典型事件 | ability | keep; link strongly to reading-side typical-event analysis |
| 作文细节与人物塑造 | ability | keep; link to reading-side detail analysis |
| 作文结构与过渡 | ability | keep; expand to narrative and argumentative organization |
| 作文语言与修改 | ability family | split lexical precision / sentence revision / paragraph revision / style |
| 作文点题与成长关联 | narrative-specific ability | keep under narrative-writing branch, not universal writing rule |
| 作文素材迁移 | transfer ability | keep; evidence should include re-framing one material for different prompts |

## Language basics

| v1 node | v2 interpretation | migration action |
| --- | --- | --- |
| 字音字形与书写 | knowledge/performance family | split phonology/orthography/handwriting if needed operationally |
| 语病与句子表达 | mixed knowledge+ability | split syntax/logic knowledge from diagnosis and minimal-revision ability |

---

# 2. Strategy seeds already present in method cards

The current method-card database already contains the beginnings of the Strategy graph.

High-value seed strategies include:

```text
辨题 -> 定位 -> 取证 -> 推理 -> 作答 -> 自检

结论 -> 证据 -> 解释 -> 回扣

特殊情境 -> 关键选择 -> 与常态/他人差异 -> 稳定品质 -> 人物/主旨
```

These should not remain only as learner notes. Their canonical versions should become Strategy nodes and link to many Task Types.

Potential mother-strategy families to test during consolidation:

1. Locate-select-compress.
2. Evidence-explanation-conclusion.
3. Comparison-dimension / evidence A-B / difference / significance.
4. Content-position-context-function.
5. Word/phrase -> literal/context meaning -> technique -> concrete effect -> larger meaning.
6. Scene/event/person -> keywords -> emotional direction -> emotional object.
7. Classical keyword -> literal translation -> syntax adjustment -> ellipsis -> verification.
8. Viewpoint -> reason -> evidence -> boundary.
9. Constraint -> central question -> evidence/material -> organization -> output.
10. Draft -> diagnose -> minimal revision -> verify intended effect.

---

# 3. What the uploaded school learning design contributes

The人物叙事 unit is especially valuable because it is already graph-like rather than worksheet-like.

## 3.1 It links genre knowledge to observable tasks

The material distinguishes memoir and biography through:

- point of view;
- material selection;
- writing purpose;
- modes of expression;
- emphasis.

This implies a Knowledge branch for **genre features**, plus an Ability branch for **evidence-based genre judgment**.

## 3.2 It demonstrates reading-writing bidirectional transfer

Reading-side analysis of:

- typical events;
- details;
- point of view;
- structure;
- language revision;

is repeatedly converted into writing-side guidance.

The graph should therefore include explicit `transfers_to` edges rather than treating reading and writing as separate silos.

Examples:

```text
analyze typical event -> supports -> select effective writing material
analyze detail effect -> supports -> construct vivid scene
compare draft/revision -> supports -> revise own wording
analyze viewpoint -> supports -> choose narrative perspective
```

## 3.3 It contains rubric dimensions, not just a final score

The writing rubric separates:

- central idea;
- material/detail;
- dual perspective and personal change;
- structure/language.

This supports the v2 principle that composition evidence should be multidimensional. A single composition score is insufficient for profile updates.

## 3.4 It already moves through cognitive complexity

The unit includes:

```text
extract events
-> infer traits
-> analyze detail effects
-> compare genre/style
-> explain structure
-> evaluate interpretation
-> revise language
-> transfer into writing
```

This is a strong model for how Training Moves should sequence around one source domain without being grade-driven.

---

# 4. What the classical source pool contributes

## 4.1 Textbook classical prose + poetry

The existing third-unit source set provides a compact seed for:

- word/sentence decoding;
- translation;
- descriptive prose;
- scene/feeling relations;
- classical prose style comparison;
- poetry imagery/emotion;
- memorization evidence.

These sources are useful for early graph seeding but are too narrow to establish broad transfer by themselves.

## 4.2 `世说新语` extended corpus

The current reading arrangement spans many entries across categories such as:

- 德行;
- 言语;
- 政事;
- 文学;
- 方正;
- 雅量;
- 识鉴;
- 赏誉.

This is unusually valuable for a dependency/profile system because it offers **many short, stylistically related but semantically varied unfamiliar classical texts**.

Recommended use:

```text
same knowledge node + new passage
-> measure contextual transfer

same strategy + different category
-> measure semantic generalization

same vocabulary/virtual word across passages
-> strengthen lexical network

character/evaluation task across short passages
-> separate decoding failure from reasoning failure
```

The corpus should therefore be modeled as many Materials with shared source collection metadata, not as one giant monolithic task.

---

# 5. Major graph branches still missing or underrepresented

The current 42-node inventory is strong for junior-high literary reading, classical basics, poetry and narrative writing. It does **not yet provide complete Gaokao coverage**.

## 5.1 Language and expression knowledge

Need explicit nodes for:

- word meaning and contextual meaning;
- collocation;
- sentence components and syntax;
- reference/cohesion;
- conjunction and logical relations;
- punctuation where operationally relevant;
- register, concision and appropriateness;
- paragraph coherence;
- information transformation.

## 5.2 Information / expository / argumentative reading

Need stronger branches for:

- multi-text information integration;
- table/model reconstruction;
- causal/conditional relation extraction;
- thesis/reason/evidence structure;
- implicit premise;
- evidence relevance;
- evidence sufficiency;
- inference from rules;
- applying a theory/rule to a new case;
- evaluating competing claims;
- reverse inference from answer/result to question/cause;
- solution synthesis.

## 5.3 Literary reading at higher complexity

Need nodes for:

- narrator / focalization / perspective;
- narrative order and time structure;
- characterization system beyond trait labeling;
- setting and atmosphere;
- symbol and recurring motif;
- irony/ambiguity/multiple interpretation;
- structural repetition and variation;
- literary evaluation with evidence;
- genre-specific reading of novel, prose, drama and biography.

## 5.4 Classical Chinese advanced branch

Need to expand:

- fixed structures;
- ellipsis;
- inversion/passive/other sentence patterns;
- discourse/argument structure in classical prose;
- historiographical narrative;
- thinkers/argumentative prose;
- cultural institutions and conventional terms;
- cross-text viewpoint comparison.

## 5.5 Poetry advanced branch

Need:

- poetic form and structure;
- allusion;
- speaker/persona;
- temporal/spatial organization;
- style/tone;
- ambiguity and layered emotion;
- same-image different-function comparison;
- poem-to-prose / poem-to-poem intertextual reading.

## 5.6 Literature and culture

Currently underrepresented as a canonical graph:

- author/work knowledge;
- literary history;
- genres and traditions;
- whole-book reading;
- cultural concepts and historical context;
- Beijing-specific whole-book requirements where relevant;
- connection between cultural knowledge and textual interpretation.

## 5.7 Writing beyond narrative

Need full argumentation branch:

```text
prompt constraint parsing
-> concept definition
-> claim
-> reasons
-> evidence
-> warrant/explanation
-> counterexample / qualification
-> multi-angle reasoning
-> paragraph structure
-> whole-essay organization
-> task/audience adaptation
```

Also need:

- micro-writing;
- source-based writing;
- practical writing;
- revision evidence across versions.

## 5.8 Communication and cross-media

Need:

- audience-purpose analysis;
- interview question design;
- speech/discussion;
- chart/text conversion;
- summarization and rewriting;
- cross-media information selection;
- real-task solution communication.

---

# 6. Profile implications of different node families

Not all nodes should decay or be verified in the same way.

## Memory-sensitive nodes

Examples:

- character forms;
- classical meanings;
- poetry memorization;
- literature/cultural facts.

These need stronger spaced-review and recency weighting.

## Strategy/automation-sensitive nodes

Examples:

- translation procedure;
- evidence-explanation-conclusion;
- comparison strategy;
- structural-effect analysis.

These need hint-level and timed-invocation evidence.

## Transfer-sensitive abilities

Examples:

- evidence sufficiency;
- multi-text integration;
- character reasoning;
- theme inference;
- material transfer in writing.

These need diverse Materials and Task Types before M3.

## Production-heavy abilities

Examples:

- written expression;
- composition detail;
- argumentation;
- revision.

These need artifact/version evidence, not only short-answer scores.

This distinction should influence profile update and recommendation rules.

---

# 7. Recommended seed order

The graph should not be populated breadth-first with hundreds of nodes immediately.

Recommended order:

1. **Preserve and normalize current 42 nodes** so existing tutoring continues.
2. **Promote existing method cards into Strategy seeds**.
3. **Add missing cross-domain foundation nodes**: evidence, comparison, logic relations, information integration, expression precision.
4. **Expand information/argumentative reading** from recent Gaokao evidence.
5. **Expand classical and poetry knowledge branches** using current textbook + extended corpus.
6. **Build literature/culture and whole-book branches**.
7. **Expand writing into full narrative + argumentation + revision graph**.
8. **Add communication/cross-media branch**.
9. **Run Gaokao coverage mapping**, adding only evidence-backed gaps.

This sequence protects current utility while systematically moving toward full coverage.
