# Current-state audit

## Purpose

This document records the system that already exists before the v2 refactor. It is intentionally descriptive: the goal is to preserve what already works, identify structural debt, and avoid rebuilding proven teaching workflows from scratch.

## Existing operational layers

The current Notion workspace already contains a useful first-generation learning system:

| Component | Current role | Keep / change |
| --- | --- | --- |
| Project Instruction | Tutoring interaction rules, answer-scaffolding policy, recap policy | Keep; refactor references to new entities |
| 能力地图 | 42 trainable nodes, terminal capability mapping, mastery/automation fields | Preserve content; split static node definition from learner state |
| 题型地图 | Question-type recognition signals, core question, algorithm, output skeleton, self-check, boundaries | Keep; convert into Task Type layer linked to graph nodes and reusable strategies |
| 真实训练题库 | Formal training questions and training status | Split static Question from learner-specific Assignment/Attempt state |
| 课外真实题型与训练素材池 | Candidate-source intake and quality screening | Keep; split source Material from Question when multiple tasks share one text |
| 学习记录 | High-level recap of a learning topic | Keep as Session Summary, not as per-question event log |
| 错题与薄弱点 | Error type, reason, correct path, review flag | Keep only for stable/aggregated error patterns; raw errors belong to Attempt events |
| 知识与方法卡 | Reusable methods, knowledge notes, examples, boundaries | Separate canonical Knowledge/Strategy nodes from learner notes |
| 复习计划 | Human-created review tasks | Evolve into Recommendation / Training Queue; review becomes one recommendation reason |
| 高考核心参考 | Recent authentic examination corpus and capability evidence | Keep as coverage/evidence corpus and architecture validation source |

## Material inventory already available

The source pool is already heterogeneous, which is a strength rather than a problem:

- unit-level school learning designs that integrate reading, literary analysis, comparison, language revision, evaluation and writing;
- textbook classical prose and poetry with memorization requirements;
- a large extended reading list for `世说新语`, useful for repeated classical-language transfer instead of one-text memorization;
- recent Beijing and national Gaokao papers, answers and official/authoritative analyses;
- a candidate pool of 49 authentic or traceable items spanning modern reading, classical Chinese, poetry, language use, writing and high-school bridge tasks.

The key design lesson is that a source document is not the same thing as a question. One text can generate several tasks, and the same capability can be trained across many texts and task types.

## What the current teaching workflow already gets right

### 1. Think before model answer

The interaction flow distinguishes between:

- no idea -> one minimal guiding question;
- partial idea -> identify what is already correct, locate one missing link, ask for a second attempt;
- basically correct -> normalize into exam-ready written expression.

This should remain the core tutoring behavior.

### 2. Understanding and expression are diagnosed separately

A learner may understand the text but fail to convert understanding into:

`text evidence -> explicit reasoning -> precise terminology -> complete written answer`.

This distinction is central to the future profile model and must become measurable in Attempt events.

### 3. Question types are used as real training environments

The current system correctly rejects capability drills detached from authentic language tasks. A question type is a training environment, not the capability itself.

### 4. Algorithms are small and executable

Existing methods follow a useful form:

`recognition signal -> core question -> 3-5 step algorithm -> output skeleton -> self-check -> boundary`.

This structure should be preserved, but question-specific algorithms should gradually consolidate into a smaller Strategy/Mother-Model graph.

### 5. Transfer is already treated as the mastery criterion

The current mastery definition distinguishes prompted completion, independent routine performance and stable unfamiliar transfer. This is directionally correct and should be retained, but mastery must be calculated from multiple Attempt events rather than stored as an unsupported manual label.

## What existing school materials reveal that the current schema under-models

The unit learning design for人物叙事 demonstrates several relationships that the current database structure does not fully represent:

1. **Reading-to-writing transfer**: analysis of typical events and details feeds directly into composition selection and scene construction.
2. **Genre knowledge**: memoir and biography are compared through point of view, material selection, purpose, expression mode and emphasis.
3. **Revision as a skill**: comparing manuscript and revised wording is not merely language appreciation; it is also a writing-revision training task.
4. **Rubric dimensions**: writing quality is decomposed into central idea, material/detail, perspective/change, structure/language. These are evidence dimensions, not merely one overall composition score.
5. **Evaluation and interpretation**: tasks move from extraction to explanation to comparative judgment and value evaluation.
6. **One source, many capabilities**: a single text can train evidence location, character analysis, structure, style, comparison, evaluation and writing imitation.

These observations strongly support a graph-plus-event architecture rather than a grade/unit table architecture.

## Structural debt in v1

### A. Static graph definitions and learner state are mixed

Examples:

- mastery, automation, last-trained and needs-improvement are stored directly on Ability nodes;
- familiarity is stored directly on method cards;
- question training state is stored directly on Question records.

These properties describe the learner, not the canonical node or question. They must move into profile/attempt/assignment entities.

### B. Knowledge and ability are partially mixed

Items such as `文言实词`, `字音字形`, `人物形象分析`, `开放评价` currently live in one capability table although they represent different semantic types:

- knowledge to know;
- operations to perform;
- reusable strategies to invoke.

The v2 graph must distinguish these node types while still allowing dependencies across them.

### C. Grade/stage labels are overused as organizing fields

Fields such as `初二达标标准`, `初二定位`, `高中桥接`, and question `学段` are useful as source or load metadata, but they should not control progression.

Progression should be based on:

- prerequisites,
- learner state,
- complexity,
- transfer evidence,
- forgetting/review risk,
- current curriculum relevance when applicable.

### D. Question and material are conflated

A long passage, poem, classical text or source set should be a reusable Material entity. Individual prompts should be Question/Task entities linked to that material.

### E. Raw attempts and summaries are conflated

`学习记录` is valuable as a durable recap, but it is too heavy to serve as a per-question event store. A lightweight structured Attempt log is required.

### F. Error records duplicate raw performance data

Every wrong answer should not become a permanent error-note record. Stable repeated patterns should be promoted from Attempt events into an Error Pattern layer.

### G. Review planning is not yet recommendation-driven

The current review plan is mostly manually authored. The target system should generate explainable recommendations from profile state and graph structure.

## v2 design constraints derived from the audit

The refactor must satisfy all of the following:

1. Do not destroy existing teaching interaction quality.
2. Do not organize the learning path by school grade.
3. Preserve authentic-task training and source traceability.
4. Separate canonical/static entities from learner-specific/dynamic state.
5. Treat real learner attempts as the primary evidence source.
6. Make recommendation decisions explainable.
7. Support one learner now without blocking multiple learners later.
8. Make Gaokao coverage auditable: every stable Gaokao demand must map to graph nodes and task types.
9. Prefer migration by addition/backfill/verification over destructive schema replacement.
10. Keep the number of learner-facing strategies small even if the internal graph is large.

## Refactor conclusion

The current system is not a failed prototype. Its tutoring loop, authentic-task principle, method-card structure and early high-exam backcasting are strong foundations. The main problem is data modeling: learner state, static graph content and task instances are mixed together.

The v2 refactor therefore focuses on **normalization and evidence flow**, not on replacing the pedagogy.
