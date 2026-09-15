# v1 能力地图 → v2 LearningNode migration disposition

Status: **B1 migration design — reviewed**

This document accounts for every current v1 能力地图 row. It does **not** imply a 42→42 conversion and does not yet create the complete Gaokao graph.

## Migration principles

- canonical IDs use `CN-<TYPE>-<SLUG>` and do not encode domain;
- `aliases_zh` is only for one-to-one semantic synonyms;
- mixed v1 labels that split into several nodes stay visible here as migration mappings rather than being attached as aliases to one child;
- grade/stage fields do not migrate into canonical LearningNode;
- domain-specific task wording does not justify duplicate cross-domain reasoning nodes.

## Disposition vocabulary

- **KEEP-A** — preserve as a reportable Ability, usually with decomposed prerequisites later.
- **SPLIT K+A** — split concept/knowledge from observable use/analysis.
- **SPLIT FAMILY** — split one broad v1 row into several canonical nodes.
- **TASK+UNDERLYING** — retain exam wording mainly as Task Type; tag underlying nodes separately.
- **GENERALIZE/MERGE** — map domain-specific labels to one shared operation plus domain prerequisites.
- **TRANSFER-EVIDENCE** — model mainly as a transfer condition/evidence dimension rather than one atomic skill.
- **PERFORMANCE-FAMILY** — memory/reproduction/performance family with distinct evidence/decay behavior.

---

# 1. 现代文阅读（13）

| v1 | disposition | canonical v2 direction | notes |
| --- | --- | --- | --- |
| 信息提取与概括 | KEEP-A, later decompose | `CN-A-locate-relevant-information`; `CN-A-select-relevant-information`; classify/compress subskills | reporting aggregate may remain; leaves should expose failure point |
| 人物形象分析 | GENERALIZE/MERGE + composite reporting | shared `CN-A-evidence-to-character-judgment`; optional modern-reading composite outcome | cross-domain reasoning belongs to `META`, not duplicated by text era |
| 典型事件与选材 | SPLIT analysis/production | reading `analyze-typical-event-value`; writing `CN-A-select-typical-writing-material`; Strategy `CN-S-special-context-key-choice-significance` | reading→writing transfer edge later |
| 细节描写分析 | KEEP-A + transfer | `CN-A-explain-detail-function`; supports writing-side detail construction | analysis ≠ production |
| 词句赏析 | TASK+UNDERLYING | Task Type `词句赏析`; contextual meaning + technique knowledge + effect reasoning | do not preserve one vague ability score as sole diagnosis |
| 修辞与表达效果 | SPLIT K+A | `LAN.rhetoric` Knowledge family + contextual effect Ability | device recognition ≠ explanation |
| 表现手法 | SPLIT K+A | technique Knowledge + effect/function Ability | organize named techniques separately from use |
| 对比与衬托 | SPLIT K+A + strategy | Knowledge of technique + shared comparison Ability + comparison Strategy | recognition alone insufficient |
| 结构与段落作用 | KEEP-A + strategy | structural-function Ability + content/position/context/function Strategy | strong mother-strategy seed |
| 标题含义与作用 | TASK+UNDERLYING | Task Type + semantic inference + structure/theme integration | task environment, not atomic ability |
| 作者情感与主旨 | SPLIT FAMILY | emotional inference; theme inference; across-text evidence integration | emotion/theme can fail independently |
| 文体辨析 | SPLIT K+A | `LIT.genre` Knowledge + `CN-A-judge-modern-genre-from-evidence` | mixed legacy label remains here, not child alias |
| 开放评价与迁移 | SPLIT FAMILY / TRANSFER-EVIDENCE | evidence-based evaluation; bounded judgment; transfer-to-new-case | “开放题” is Task family |

---

# 2. 文言文（11）

| v1 | disposition | canonical v2 direction | notes |
| --- | --- | --- | --- |
| 文言实词 | SPLIT K+A | `CN-K-classical-content-word-meaning-network` + `CN-A-infer-classical-contextual-word-sense` | separate retention from contextual selection |
| 文言虚词 | SPLIT K+A | function Knowledge + contextual parsing Ability | syntax/function-heavy |
| 古今异义与一词多义 | SPLIT FAMILY | separate Knowledge families; contextual-sense Ability links above | different review/evidence behavior |
| 词类活用与特殊句式 | SPLIT FAMILY | 活用 Knowledge; 判断/省略/被动/倒装/固定结构 Knowledge/recognition abilities | too heterogeneous for one leaf |
| 文言断句 | KEEP-A | `CN-A-segment-classical-sentence` | prerequisites added in B2 |
| 文言翻译 | KEEP-A + strategy | `CN-A-translate-classical-sentence-accurately`; `CN-S-classical-translation-five-step` | stable outcome + reusable procedure |
| 文言内容理解 | KEEP-A, later decompose | proposition/event reconstruction + content summarization | separate decoding from higher inference |
| 文言人物形象 | GENERALIZE/MERGE | shared `CN-A-evidence-to-character-judgment` + classical decoding prerequisites | no duplicate reasoning node by language era |
| 文言写作手法 | GENERALIZE/MERGE + K+A | shared technique Knowledge/Ability; keep classical-specific discourse knowledge only when representation differs | avoid era-based duplication |
| 文言主旨 | GENERALIZE/MERGE | shared theme-inference operation + classical decoding/discourse prerequisites | same evidence logic across domains |
| 文言课内外迁移 | TRANSFER-EVIDENCE | record what Knowledge/Ability transferred and under what novelty | not a monolithic skill |

---

# 3. 古诗词（9）

| v1 | disposition | canonical v2 direction | notes |
| --- | --- | --- | --- |
| 诗词画面还原 | KEEP-A | `CN-A-reconstruct-poetic-scene` | local evidence + integrated scene |
| 意象与意境 | SPLIT K+A | conventional image Knowledge + atmosphere/scene Ability | local text overrides memorized symbol |
| 炼字 | TASK+UNDERLYING | Task Type + contextual word sense + substitution comparison + concrete effect | not a unique magical faculty |
| 诗词修辞 | SPLIT K+A | rhetoric Knowledge + poetry-local effect Ability | share device knowledge where possible |
| 诗词表现手法 | SPLIT K+A | technique Knowledge + effect Ability | poetry representation remains domain-specific where needed |
| 写景角度与情景关系 | SPLIT FAMILY | perspective Knowledge + scene/emotion relation Ability | currently two families |
| 诗词思想感情 | KEEP-A, later decompose | emotion inference; layered/evolving emotion later if evidence requires | useful reporting outcome |
| 诗词比较阅读 | SPLIT FAMILY + strategy | shared comparison Ability/Strategy + poem-specific interpretation | method should transfer beyond poetry |
| 背诵与默写 | PERFORMANCE-FAMILY | memory nodes for required works/lines + accurate reproduction | decay differs from reasoning nodes |

---

# 4. 写作（7）

| v1 | disposition | canonical v2 direction | notes |
| --- | --- | --- | --- |
| 作文审题与立意 | SPLIT FAMILY | `CN-A-parse-writing-prompt-constraints`; form central question; form controlling idea | independently diagnosable |
| 作文选材与典型事件 | KEEP-A + transfer | `CN-A-select-typical-writing-material`; relation from reading-side event analysis | strong seed |
| 作文细节与人物塑造 | SPLIT FAMILY | construct scene; select characterizing detail; show change through detail | use artifact evidence |
| 作文结构与过渡 | SPLIT FAMILY | narrative/argument organization; sequence; transition/cohesion | narrative/argument branches later diverge |
| 作文语言与修改 | SPLIT FAMILY | lexical precision; sentence revision; paragraph revision; style/task adaptation | too broad for one diagnosis |
| 作文点题与成长关联 | KEEP-A under narrative branch | ending-to-central-change; reflective growth if warranted | not universal writing rule |
| 作文素材迁移 | TRANSFER-EVIDENCE + Ability | reframe material for genuinely different prompts | transfer requires real prompt variation |

---

# 5. 基础知识（2）

| v1 | disposition | canonical v2 direction | notes |
| --- | --- | --- | --- |
| 字音字形与书写 | SPLIT FAMILY / PERFORMANCE | phonological/character-form Knowledge; orthographic accuracy; optional handwriting performance | do not couple unrelated interventions |
| 语病与句子表达 | SPLIT K+A / FAMILY | syntax/logic Knowledge + diagnose sentence problem + `CN-A-revise-sentence-minimally` | detection and repair can diverge |

---

# 6. Strategy seeds from existing method cards

| v1 method card | v2 Strategy direction | semantic status |
| --- | --- | --- |
| 阅读答题总算法｜辨—定—证—推—答—检 | broad metacognitive orchestration Strategy | may later become wrapper strategy |
| 表达升级算法｜想法→逻辑→得分句 | normalize into evidence/explanation/conclusion family | avoid competing order labels |
| 典型事件为什么“典型” | `CN-S-special-context-key-choice-significance` | narrative seed with writing transfer |

B1 rule: do not proliferate a Strategy per Task Type. G1 will consolidate approximately 10–15 mother strategies.

---

# 7. v1 fields that do not migrate into LearningNode

```text
当前掌握度
自动化度
最近训练
待加强
阶段定位
初二达标标准（as grade progression field）
```

Later homes:

- personal state → `LearnerNodeState`;
- attempt evidence → `TrainingAttempt`;
- load/scaffold/current target → recommendation/training context;
- useful observable-success wording from old standards may be preserved after removing grade semantics.

---

# 8. Accounting result

All 42 v1 rows are explicitly accounted for. The migration intentionally changes table shape: mixed rows split, duplicate domain reasoning generalizes, and exam labels move toward Task Types. The objective is semantic correctness, diagnosis and transfer—not one-to-one preservation.
