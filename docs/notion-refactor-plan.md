# Notion refactor plan

## Objective

Refactor the current operational workspace without destroying working pedagogy or historical data.

The migration follows:

```text
add new structures
-> backfill from v1
-> dual-run selected workflows
-> validate profile/evidence behavior
-> switch source-of-truth references
-> deprecate redundant v1 fields/views
```

No destructive deletion should happen in the first migration cycle.

---

# 1. Target operational databases

## 1. Learning Graph Nodes

Replaces the role currently split across `能力地图` and parts of `知识与方法卡`.

### Properties

| Property | Type | Notes |
| --- | --- | --- |
| 节点 | title | Canonical name |
| Node ID | text | Stable ID, e.g. `CN-ABILITY-EVIDENCE-001` |
| 节点类型 | select | 知识 / 能力 / 策略 |
| 领域 | select | 语言文字 / 现代文 / 文言文 / 古诗词 / 文学文化 / 写作 / 语言运用 / 元认知 |
| 子领域 | text/select | More specific navigation |
| 定义 | text | What this node means |
| 可观察成功标准 | text | What performance proves the node |
| 高考相关性 | select | 核心 / 支撑 / 拓展 |
| Parent | self relation | decomposition |
| Prerequisites | self relation | hard requirements |
| Supports | self relation | soft relation if useful |
| 状态 | select | active/draft/deprecated |
| 来源/覆盖证据 | text or relation | exam/curriculum evidence |

### Migration

- existing capability nodes become `能力` or `知识` nodes after semantic review;
- reusable method cards become `策略` nodes when they represent stable algorithms;
- learner-specific properties are **not** copied into canonical node fields.

---

## 2. Learner Node State

New database. This is the core personal profile.

### Properties

| Property | Type |
| --- | --- |
| 状态记录 | title |
| Learner | text/relation |
| 节点 | relation -> Learning Graph Nodes |
| Mastery | select M0/M1/M2/M3 |
| Automation | select A0/A1/A2/A3 |
| Complexity Ceiling | number/select C0-C5 |
| 状态 | select Stable/Developing/Bottleneck/Review Due/Unknown |
| Evidence Strength | number |
| Attempt Count | number/rollup |
| Independent Successes | number/rollup |
| Transfer Successes | number/rollup |
| 最近训练 | date/rollup |
| 最近验证 | date |
| 遗忘风险 | select |
| 主要错误模式 | relation/text |
| Priority | number/formula/manual-v1 |
| 推荐解释 | text |

### Migration

The current `能力地图` learner-state fields are copied here:

```text
当前掌握度 -> Mastery initial value
自动化度 -> Automation initial value
最近训练 -> 最近训练
待加强 -> Developing/Bottleneck candidate, not direct truth
```

Initial `Evidence Strength` must remain low unless supported by several existing training records.

---

## 3. Materials

New database. Splits language material from individual questions.

### Properties

- Material ID
- 标题
- 类型/文体
- 作者
- 时代
- 来源类型
- 年份
- 地区/试卷
- 来源学段 (metadata only)
- 来源链接/file
- 来源可靠性
- 原文完整性
- 版权/使用备注
- 长度/阅读负荷

### Migration

- candidate pool records that refer to one passage/source become Materials;
- a full exam passage should exist once even if several questions use it;
- school uploaded texts remain source assets and can be linked without retyping copyrighted text into public systems.

---

## 4. Task Types

Evolution of current `题型地图`.

### Keep from v1

- 识别信号
- 核心问题
- 输出骨架
- 自检点
- 适用边界

### Refactor

- `3—5步算法` should primarily become relation(s) to Strategy nodes;
- task types link to default Knowledge/Ability targets;
- remove `初二定位` as an organizing dimension;
- optional fields may describe typical complexity range and common failure modes.

---

## 5. Questions

Evolution of `真实训练题库`.

### Static fields only

- Question ID
- Material relation
- Task Type relation
- prompt
- reference answer / rubric
- source question number
- Knowledge nodes
- Ability nodes
- Strategy nodes
- Complexity vector / band
- source authenticity metadata

### Fields to remove from static Question source-of-truth

```text
训练状态
最近训练
是否陌生迁移 (learner-specific result)
```

These move to Training Move / Attempt evidence.

---

## 6. Training Attempts

New event database.

### Minimum viable properties

- Attempt ID
- date/time
- Question
- Training Move (optional initially)
- attempt number
- H0-H7 hint level
- task-type recognition 0/1/2
- text location 0/1/2
- evidence 0/1/2
- reasoning 0/1/2
- terminology 0/1/2
- expression 0/1/2
- correctness 0/1/2
- error codes multi-select K/R/I/E/Q/M/C
- first-answer summary/raw answer
- intervention
- second-answer result
- delta / breakthrough

### Why this database is mandatory

Without Attempt events, the personal profile is only manual annotation. With Attempt events, profile changes can be explained and audited.

---

## 7. Training Queue / Recommendations

Replaces most operational use of `复习计划` and question-level training status.

### Properties

- recommendation title
- target nodes
- move type: learn / scaffold / fade / timed / transfer / review
- preferred Task Types
- selected Question (optional until assigned)
- target complexity
- max hint level
- reason
- priority
- status: proposed / queued / active / completed / cancelled
- created date
- due/review date if relevant
- success criterion

### Views

- 下一步最值得练
- Bottleneck
- Review Due
- Transfer Needed
- Timed Automation

---

## 8. Session Summaries

Current `学习记录` is retained but semantics change.

Use for:

- meaningful learning sessions;
- newly formed reusable strategy;
- stable error-pattern discovery;
- transfer milestone;
- periodic profile summary.

Do not create one record per simple question.

Recommended additional relations:

- Attempts
- Graph Nodes
- Strategy Nodes
- Questions/Materials

---

## 9. Error Patterns

Current `错题与薄弱点` becomes a promoted-pattern database.

### Promotion rule

Create/update a stable error pattern only when one of these is true:

- same causal error repeats across multiple attempts;
- error affects several task types;
- error is a high-centrality bottleneck;
- the learner needs a dedicated remediation sequence.

Single mistakes stay only in Training Attempts.

Recommended fields:

- pattern name
- error code
- affected nodes
- evidence attempts
- causal description
- repair strategy
- status
- last observed
- verified resolved date

---

## 10. Learning Notes

Current `知识与方法卡` should remain as learner-facing notes, but should not duplicate canonical definitions.

Recommended relation:

```text
Learning Note -> canonical Graph Node(s)
```

Use it to store:

- memorable formulation;
- personal example;
- common mistake;
- learner-specific cue;
- source example.

A strategy that is globally reusable belongs first in the canonical Strategy graph, then may have a personalized note.

---

# 2. Fields to deprecate gradually

## 能力地图

Candidate learner-specific fields for migration out:

- 当前掌握度
- 自动化度
- 最近训练
- 待加强

Candidate grade-first fields to replace:

- 初二达标标准 -> canonical `可观察成功标准` plus complexity-specific evidence definitions;
- 阶段定位 -> replaced by profile + dependency + recommendation;
- 真实题型训练场 -> relations to Task Types rather than free text.

`高考终局能力(L1)` may be retained temporarily as a navigation view during migration, but should eventually be represented as higher-level graph nodes/relations rather than a fixed select taxonomy if graph coverage evolves.

## 题型地图

- 初二定位 -> remove as progression control;
- 3—5步算法 -> relation to Strategy nodes, with optional task-specific adaptation text.

## 真实训练题库

- 学段 -> rename `来源学段` and treat only as metadata;
- 训练状态 -> Training Queue / Attempts;
- 最近训练 -> Attempts/Profile;
- 是否陌生迁移 -> computed from Training Move + attempt result.

## 候选训练素材

- 年级/学段 -> 来源学段;
- 建议训练阶段 is useful and should be renamed to `建议训练动作` or mapped to Training Move type;
- split Material and Question before promotion when a source contains multiple prompts.

---

# 3. Migration sequence

## Migration M0 - freeze semantics

- document every v1 database and field;
- no destructive edits;
- establish stable IDs in GitHub specs.

## Migration M1 - create canonical graph + profile

- create Learning Graph Nodes;
- create Learner Node State;
- backfill current 42 nodes;
- classify each as Knowledge/Ability/Strategy candidate;
- preserve old Ability Map unchanged during dual run.

Acceptance:

- every old node maps to exactly one canonical node or an explicit merge/split decision;
- learner-state fields exist separately;
- no loss of current mastery metadata.

## Migration M2 - create Attempt events

- add Training Attempts;
- backfill the existing Zhou Yafu session as sample attempt evidence;
- use new event logging for all new training sessions.

Acceptance:

- one real session can be reconstructed from Attempt records;
- profile update can be justified from recorded evidence.

## Migration M3 - normalize material/question

- create Materials;
- refactor formal Question bank;
- promote candidate pool records through Material + Question separation;
- preserve original links and authenticity information.

## Migration M4 - recommendation queue

- create Training Queue;
- convert open review-plan items into queue entries;
- stop using Question training state as learner state.

## Migration M5 - strategy consolidation

- identify duplicate algorithms;
- promote 10-15 mother strategies;
- relate Task Types to strategies;
- retain task-specific boundaries.

## Migration M6 - coverage QA

- map selected recent Gaokao questions to nodes/task types/complexity;
- identify graph gaps;
- add missing nodes only with evidence.

## Migration M7 - deprecate v1 fields

Only after several weeks of successful dual-run:

- hide obsolete views/fields;
- do not delete historical content until export/backup and migration audit are complete.

---

# 4. Profile update policy v1

The first implementation should be rule-based, not opaque.

## Suggested promotion evidence

### M0 -> M1

Meaningful guided completion on at least one authentic task, with the missing reasoning step understood in a second attempt.

### M1 -> M2

Multiple independent successes on non-identical authentic tasks, with no critical H4+ support.

### M2 -> M3

Repeated unfamiliar transfer across at least two distinct materials and preferably more than one task family when the ability is cross-task.

## Automation

### A0 -> A1

Can invoke strategy after explicit reminder.

### A1 -> A2

Recognizes task signal and invokes strategy independently.

### A2 -> A3

Maintains correct invocation under realistic time pressure.

## Downgrade / review

A state can become `Review Due` or reduce effective confidence when:

- recent evidence conflicts with old mastery;
- the node has not been used for a long interval and is memory-sensitive;
- transfer attempts fail repeatedly;
- performance only holds with rising hints.

Do not automatically downgrade durable reasoning skills solely because time has passed; forgetting risk depends on node type.

---

# 5. Recommendation policy v1

For each candidate target node, calculate or manually estimate:

```text
Need = mastery gap + automation gap + transfer gap + forgetting risk + evidence uncertainty
Value = Gaokao importance + dependency unlock value + current task relevance
Feasibility = suitable material availability + prerequisite readiness
Priority ~ Need x Value x Feasibility
```

Rules:

1. Block a target if a hard prerequisite is materially missing.
2. Prefer a high-centrality prerequisite over a low-value isolated weak node.
3. Do not repeat near-identical tasks after independent success; increase complexity or vary task family.
4. If understanding is strong but expression is weak, target expression rather than reteaching content.
5. Use source-grade only to regulate load/context, never as the main prerequisite.
6. Every recommendation must include a human-readable reason.

---

# 6. Minimal data-entry policy

The operational system succeeds only if logging cost stays low.

For a routine question, record only:

- question;
- hint level;
- 0/1/2 diagnostic dimensions;
- error code(s);
- short intervention;
- second-attempt result.

Long prose recap is created only when something durable changes.

This keeps evidence dense without turning learning into database maintenance.
