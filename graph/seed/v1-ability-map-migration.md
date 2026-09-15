# v1 能力地图 → v2 LearningNode migration disposition

Status: **B1 migration design**

This document assigns every current v1 能力地图 row a semantic migration disposition. It does **not** create all final Gaokao nodes. Its purpose is to make the v1→v2 transition explicit and lossless.

## Disposition vocabulary

- **KEEP-A** — preserve the v1 concept as a reportable Ability node, although prerequisites may later be decomposed.
- **SPLIT K+A** — split knowledge/concept recognition from observable analysis/use ability.
- **SPLIT FAMILY** — one v1 row hides several distinct canonical nodes.
- **TASK+UNDERLYING** — keep the exam label mainly as Task Type; map underlying abilities separately.
- **GENERALIZE/MERGE** — the v1 domain-specific label should map to a shared cross-domain Ability/Strategy plus domain prerequisites.
- **TRANSFER-EVIDENCE** — treat the v1 row primarily as evidence dimension/transfer condition, not as one atomic ability.
- **PERFORMANCE-FAMILY** — keep as a family whose evidence/decay behavior differs from reasoning abilities.

---

# 1. 现代文阅读（13）

| v1 | disposition | canonical v2 direction | notes |
| --- | --- | --- | --- |
| 信息提取与概括 | KEEP-A, later decompose | `CN-A-MRD-locate-relevant-information`; `CN-A-MRD-select-relevant-information`; `CN-A-META-classify-information`; `CN-A-COM-compress-without-distortion` | v1 can remain a reporting aggregate; canonical leaves should expose where failure occurs. |
| 人物形象分析 | KEEP-A + shared reasoning | `CN-A-MRD-characterization-analysis` as composite outcome; shared `CN-A-META-evidence-to-character-judgment` | Keep learner-facing outcome, but do not let it hide evidence→reasoning gap. |
| 典型事件与选材 | SPLIT analysis/production | reading Ability `analyze-typical-event-value`; writing Ability `CN-A-WRT-select-typical-material`; Strategy seed `special-context-key-choice-significance` | Current school materials explicitly support reading→writing transfer. |
| 细节描写分析 | KEEP-A + transfer edge | `CN-A-MRD-explain-detail-function`; supports `CN-A-WRT-construct-effective-detail` | Analysis and production are related but not identical. |
| 词句赏析 | TASK+UNDERLYING | Task Type `词句赏析`; underlying lexical/context meaning + technique recognition + concrete-effect explanation | Avoid one vague “赏析能力” score as sole diagnosis. |
| 修辞与表达效果 | SPLIT K+A | Knowledge family `LAN.rhetoric.*`; Ability `CN-A-MRD-explain-rhetorical-effect-in-context` | Knowing the device ≠ explaining effect. |
| 表现手法 | SPLIT K+A | Knowledge `LIT/MRD.technique.*`; Ability `CN-A-MRD-explain-technique-function` | Named technique knowledge may later be organized by literary domain. |
| 对比与衬托 | SPLIT K+A + strategy | Knowledge `对比/衬托`; shared comparison Ability; Strategy `comparison-dimension-evidence-difference-significance` | Recognition alone is insufficient. |
| 结构与段落作用 | KEEP-A + strategy | `CN-A-MRD-explain-structural-function`; Strategy `content-position-context-function` | Strong seed for mother strategy. |
| 标题含义与作用 | TASK+UNDERLYING | Task Type `标题含义与作用`; abilities: semantic inference, structure/theme integration, naming-function analysis | The label is primarily an exam environment. |
| 作者情感与主旨 | SPLIT FAMILY | `infer-emotional-direction`; `infer-theme-from-evidence`; `integrate-evidence-across-text` | Emotion and theme can fail independently. |
| 文体辨析 | SPLIT K+A | Knowledge `LIT.genre.*`; Ability `CN-A-LIT-judge-genre-from-evidence` | Preserve school terminology as aliases. |
| 开放评价与迁移 | SPLIT FAMILY / TRANSFER-EVIDENCE | `CN-A-META-evaluate-claim-with-evidence`; `CN-A-META-transfer-rule-to-new-case`; `CN-A-META-bounded-judgment` | “开放题” is a Task family, not one ability. |

---

# 2. 文言文（11）

| v1 | disposition | canonical v2 direction | notes |
| --- | --- | --- | --- |
| 文言实词 | SPLIT K+A | Knowledge lexical entries/families; Ability `CN-A-CLA-infer-contextual-word-sense` | Profile for knowledge retention and contextual inference should be distinguishable. |
| 文言虚词 | SPLIT K+A | Knowledge of common functions; Ability `CN-A-CLA-parse-function-word-in-context` | Same principle as 实词 but syntax/function-heavy. |
| 古今异义与一词多义 | SPLIT FAMILY | Knowledge `古今异义`; Knowledge/lexical network `一词多义`; Ability to select contextual sense links above | Different review/evidence behavior. |
| 词类活用与特殊句式 | SPLIT FAMILY | Knowledge `词类活用`; sentence-pattern family: 判断/省略/被动/倒装/固定结构; recognition abilities as needed | Too heterogeneous to remain one canonical leaf. |
| 文言断句 | KEEP-A | `CN-A-CLA-segment-classical-sentence` | Prerequisites later include meaning, syntax, markers, rhythm/symmetry. |
| 文言翻译 | KEEP-A + strategy | `CN-A-CLA-translate-sentence-accurately`; Strategy `classical-translation-five-step` | Reportable stable outcome. |
| 文言内容理解 | KEEP-A, later decompose | `CN-A-CLA-reconstruct-proposition/event`; `CN-A-CLA-summarize-classical-content` | Must separate decoding failure from higher inference. |
| 文言人物形象 | GENERALIZE/MERGE | shared `CN-A-META-evidence-to-character-judgment` + classical decoding prerequisites; Task Type remains classical-character task | Do not duplicate identical reasoning by language era. |
| 文言写作手法 | GENERALIZE/MERGE + K+A | shared literary-technique Knowledge/Ability plus classical-specific discourse knowledge where needed | Only retain classical-specific nodes when representation differs. |
| 文言主旨 | GENERALIZE/MERGE | shared theme inference Ability + classical decoding/discourse prerequisites | Same inference operation across domains. |
| 文言课内外迁移 | TRANSFER-EVIDENCE | transfer dimension across Knowledge/Ability nodes; Task Type `课内外迁移` | Do not make “迁移” one monolithic skill; record what was transferred. |

---

# 3. 古诗词（9）

| v1 | disposition | canonical v2 direction | notes |
| --- | --- | --- | --- |
| 诗词画面还原 | KEEP-A | `CN-A-POE-reconstruct-scene` | Evidence includes accurate integration of image/action/time-space cues. |
| 意象与意境 | SPLIT K+A | Knowledge of conventional image associations; Ability `CN-A-POE-construct-atmosphere-from-images` | Conventional associations never override local textual evidence. |
| 炼字 | TASK+UNDERLYING | Task Type `炼字`; underlying contextual word sense + substitution comparison + concrete effect | Avoid treating “炼字” as a magical separate cognitive faculty. |
| 诗词修辞 | SPLIT K+A | rhetoric Knowledge + poetry-specific contextual effect Ability | Technique knowledge shared where possible. |
| 诗词表现手法 | SPLIT K+A | poetic technique Knowledge + `CN-A-POE-explain-technique-effect` | Later includes虚实、衬托、借景等. |
| 写景角度与情景关系 | SPLIT FAMILY | Knowledge: sensory/spatial/temporal perspectives; Ability: analyze scene-emotion relation | Two different semantic families currently combined. |
| 诗词思想感情 | KEEP-A, later decompose | `CN-A-POE-infer-emotion-from-evidence`; possibly layered emotion/evolving emotion nodes later | The current node remains a useful reporting outcome. |
| 诗词比较阅读 | SPLIT FAMILY + strategy | shared comparison Ability/Strategy + poem-specific evidence interpretation | Comparison method should transfer beyond poetry. |
| 背诵与默写 | PERFORMANCE-FAMILY | memory nodes for required works/lines + accurate written reproduction ability | Decay/review rules differ from inference skills. |

---

# 4. 写作（7）

| v1 | disposition | canonical v2 direction | notes |
| --- | --- | --- | --- |
| 作文审题与立意 | SPLIT FAMILY | `CN-A-WRT-parse-prompt-constraints`; `CN-A-WRT-form-central-question`; `CN-A-WRT-form-controlling-idea` | These can fail separately and trigger different interventions. |
| 作文选材与典型事件 | KEEP-A + transfer | `CN-A-WRT-select-typical-material`; relation from reading-side typical-event analysis | Strong v1 seed. |
| 作文细节与人物塑造 | SPLIT FAMILY | `construct-scene`; `select-characterizing-detail`; `show-change-through-detail` | Production evidence should use actual drafts/versions. |
| 作文结构与过渡 | SPLIT FAMILY | narrative/argument organization; paragraph sequence; transition/cohesion | Narrative and argumentative organization must later diverge. |
| 作文语言与修改 | SPLIT FAMILY | lexical precision; sentence revision; paragraph revision; style/task adaptation; revision strategy | Current row is too broad for diagnosis. |
| 作文点题与成长关联 | KEEP-A under narrative branch | `CN-A-WRT-link-ending-to-central-change`; possible separate reflective-growth node | Not a universal writing rule; keep within narrative writing. |
| 作文素材迁移 | TRANSFER-EVIDENCE + Ability | `CN-A-WRT-reframe-material-for-new-prompt` | Requires evidence across genuinely different prompts, not paraphrased repeats. |

---

# 5. 基础知识（2）

| v1 | disposition | canonical v2 direction | notes |
| --- | --- | --- | --- |
| 字音字形与书写 | SPLIT FAMILY / PERFORMANCE | phonological/character-form Knowledge; orthographic accuracy; handwriting as optional operational performance node | Do not couple handwriting quality to lexical knowledge if interventions differ. |
| 语病与句子表达 | SPLIT K+A / FAMILY | syntax/logical-relation Knowledge + `CN-A-LAN-diagnose-sentence-problem` + `CN-A-LAN-revise-sentence-logic` | Diagnosis and minimal revision should be separately observable if useful. |

---

# 6. Cross-cutting Strategy seeds from existing method cards

The following existing cards should migrate to `strategy` nodes rather than remain only human-readable notes.

| v1 method card | v2 Strategy direction | semantic status |
| --- | --- | --- |
| 阅读答题总算法｜辨—定—证—推—答—检 | `CN-S-META-read-locate-evidence-reason-answer-check` | broad orchestration strategy; useful early, may become metacognitive wrapper rather than task-specific method |
| 表达升级算法｜想法→逻辑→得分句 | `CN-S-META-conclusion-evidence-explanation-return` or normalized `evidence-explanation-conclusion` family | needs consolidation so learner does not memorize competing order labels |
| 典型事件为什么“典型” | `CN-S-MRD-special-context-key-choice-significance` | domain-seeded strategy with writing transfer |

B1 decision: **do not proliferate Strategy nodes from every current Task Type algorithm yet.** PR G1 will cluster them into approximately 10–15 mother strategies.

---

# 7. v1 personal fields that do not migrate into LearningNode

Current fields such as:

```text
当前掌握度
自动化度
最近训练
待加强
阶段定位
```

must be backfilled into `LearnerNodeState` or replaced by recommendation metadata in later PRs. They are intentionally absent from the canonical node schema.

`初二达标标准` is also **not** a canonical progression field in v2. Its useful content should be reinterpreted as one of:

- observable-success examples;
- current load/scaffold guidance;
- learner-specific training target;
- source/curriculum relevance metadata.

No grade label is allowed to become a prerequisite edge.

---

# 8. Summary disposition count

The 42 v1 rows are all accounted for.

The migration does **not** imply a 42→42 one-to-one conversion. Several rows split, several domain-specific reasoning rows generalize, and several exam labels become Task Types with underlying canonical nodes.

This is deliberate: v2 optimizes for **semantic correctness, diagnosis and transfer**, not preservation of the old table shape.
