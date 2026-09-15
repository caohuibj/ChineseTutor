# v1 candidate/formal training records → Material / TaskType / Question migration

Status: **D1 migration design**

This document maps the current Notion candidate material pool, `真实训练题库`, and `题型地图` into D1 entities without losing source traceability or learner history.

---

## 1. Core migration rule

Do not migrate one old row into one new row mechanically.

A legacy record may contain several semantics:

```text
source passage/material
+ concrete question prompt
+ task type
+ target ability
+ source/provenance
+ editorial curation state
+ learner-specific training state
```

D1 separates them:

```text
Material
TaskType
Question
```

and routes learner-specific history to:

```text
TrainingAttempt / Learner Profile / future TrainingMove/queue
```

---

## 2. Candidate material pool

Current candidate fields include source type, year, region/paper, stage, difficulty, training value, training action, source link, reliability, recommendation-to-formal-bank, reason and notes.

### Candidate record with source only

Example shape:

```text
一篇北京区统考现代文 + 来源链接
没有完整题目文本
```

Migration:

```text
create/resolve Material
keep candidate-ingestion metadata in staging workflow
no Question until prompt is actually available/verified
```

Do not generate a Question silently merely to complete the schema.

### Candidate record with source + one authentic prompt

Migration:

```text
Material = source object
TaskType = resolve canonical task family
Question = exact prompt linked to Material
```

### Candidate record with source + several questions

Migration:

```text
one Material
many Questions
```

Do not duplicate the source text/provenance for every Question.

### Recommendation fields

`是否建议进入正式训练题库`, `推荐理由`, `训练价值` belong to candidate curation/staging, not canonical Material identity and not learner Profile.

On promotion, keep a traceable promotion note/reference if operationally useful, but do not carry staging flags as permanent semantic fields.

---

## 3. Current `真实训练题库`

Current fields include approximately:

```text
训练题
来源类型
年份
地区/试卷
学段
题型 relation
能力 relation
难度
训练状态
训练目标
是否陌生迁移
原题链接
最近训练
```

Migration:

| v1 field | D1 / later destination |
| --- | --- |
| `训练题` | split into Material title/source and Question prompt as applicable |
| `来源类型` | Material provenance |
| `年份` | Material provenance |
| `地区/试卷` | Material provenance |
| `学段` | Material `source_grade`/source metadata only |
| `题型` | TaskType relation |
| `能力` | Question canonical target-node mappings after B1 normalization |
| `难度` | provisional Question complexity metadata; normalized in Issue #6 |
| `训练状态` | **not Question**; migrate through Attempt/queue/history |
| `训练目标` | if learner-independent -> Question primary targets/curation note; if personalized -> TrainingMove |
| `是否陌生迁移` | **not static Question truth**; actual familiarity/transfer is C2 Attempt-specific |
| `原题链接` | Material/Question provenance |
| `最近训练` | TrainingAttempt/Profile, not Question |

### Current Zhou Yafu seed records

Existing formal records:

```text
《周亚夫军细柳》4.5｜为什么“劳军”一事足以塑造真将军
《周亚夫军细柳》4.6｜霸上、棘门与细柳营的对比有何作用
```

should migrate conceptually to:

```text
MAT-zhouyafu-junxiliu
  source: school/textbook material

Q-zhouyafu-45-character-evidence
  material = MAT-zhouyafu-junxiliu
  TaskType = 典型事件/人物形象相关 TaskType
  primary targets = evidence→character reasoning + event significance

Q-zhouyafu-46-comparison-function
  material = MAT-zhouyafu-junxiliu
  TaskType = 对比与衬托作用
  primary targets = comparison dimension + comparison significance
```

The learner's 2026-09-14 responses remain C2 Attempts. They do not alter Question records.

---

## 4. Current `题型地图`

The existing ~35 rows become TaskType seeds.

Do not create a new TaskType for every wording variation.

Examples likely remain stable TaskTypes:

```text
信息提取与概括题
多文本信息整合与比较题
人物形象分析题
典型事件与选材作用题
细节描写作用题
词句赏析题
对比与衬托作用题
段落与结构作用题
标题含义与作用题
开放评价与探究题
文言翻译题
诗词思想感情题
诗词比较阅读题
病句辨析与句子修改题
作文审题与立意
...
```

However, broad/high-school expansion should be judged semantically rather than preserving every v1 name forever.

`核心能力` relations must be remapped to B1 canonical nodes rather than old mixed ability rows.

`3—5步算法` is not automatically copied into a unique Strategy. It becomes:

- a recommended existing mother Strategy;
- or a candidate for G1 consolidation;
- or remains TaskType-specific guidance if it is not reusable enough to be canonical Strategy.

---

## 5. Deduplication rules

### Exact same source/version

If two old rows point to the same exact presented text/source version:

```text
one Material
multiple Questions
```

Use source identifiers/content hash/manual verification to deduplicate.

### Same underlying work, different excerpt/version

Example:

```text
same essay in textbook
same essay shortened in district exam
```

Create separate Material versions and optionally share `work_group_id`.

Do not merge if excerpting/annotations/context can change task evidence.

### Same prompt copied from multiple websites

Prefer one Question tied to the best traceable provenance. Record republication source separately if needed.

Do not create duplicate Questions merely because the same authentic prompt appears on multiple sites.

### Adapted prompt

If wording/content changes enough to affect the task:

```text
new Question
prompt_fidelity=adapted
variant_group_id=<shared family when appropriate>
```

Never label it verbatim official.

---

## 6. Answer/rubric migration

If the source provides an official answer/rubric:

```text
reference_answer_source=official
rubric_source=official
```

If the old row or teacher note contains an internally produced answer:

```text
teacher_authored / tutor_derived / authoritative_editorial
```

according to actual provenance.

Never upgrade an internal model answer to “official” during migration.

---

## 7. Learner-state extraction before canonical promotion

Before deprecating old formal-training rows, preserve:

- previous attempt history where reconstructable;
- recent training timestamp;
- status such as `需复练` as legacy scheduling evidence;
- any stable error/breakthrough notes.

Then map them into C1/C2/future queue layers.

Only after learner history has a new home should the canonical Question record stop carrying `训练状态` fields.

This keeps migration non-destructive.

---

## 8. Promotion flow after D1

```text
external/source discovery
        ↓
candidate staging record
        ↓
verify provenance/authenticity
        ↓
resolve/create Material
        ↓
extract/verify one or more Questions
        ↓
resolve TaskTypes + canonical target nodes
        ↓
provisional complexity tagging
        ↓
Question verified/active
        ↓
Recommendation selects it for a learner
        ↓
TrainingAttempt captures personal evidence
```

Candidate staging and canonical content should not be the same lifecycle object.

---

## 9. Migration invariants

1. No source traceability is lost.
2. No learner state remains embedded in canonical Material/Question.
3. No candidate source is turned into a fabricated Question just to fill a field.
4. One exact Material can serve many Questions.
5. Adapted/generated content remains explicitly labeled.
6. Grade remains provenance metadata only.
7. Old TaskType-to-ability mappings are normalized through B1 rather than copied blindly.
8. Existing learner history is preserved before old training-status fields are retired.
