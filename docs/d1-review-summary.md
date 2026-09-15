# D1 review summary

Issue: #5 — Split authentic Material from concrete Question/Task instances

## Implemented

- canonical `Material` schema with exact presentation/source-occurrence identity;
- atomic/bundle Material composition for multi-text and cross-media tasks;
- source provenance, reliability, reprint, fidelity and copyright/storage separation;
- work/content grouping without collapsing distinct source occurrences;
- canonical `TaskType` schema separate from LearningNode;
- migration of existing 题型地图 fields into TaskType semantics;
- canonical `Question` schema with primary + optional secondary TaskTypes;
- prompt fidelity/authenticity provenance;
- reference-answer/rubric provenance;
- static Question→Knowledge/Ability/Strategy target mappings;
- provisional complexity fields pending Issue #6;
- learner-relative transfer/familiarity semantics;
- non-destructive migration design for candidate pool and current 真实训练题库;
- current 《周亚夫军细柳》 Material/Question fixtures plus multi-text bundle example;
- QA gates and ADR-009.

## Semantic self-review findings

### 1. Material cannot be identified only by abstract work or content hash — fixed

The same underlying work can appear as different excerpts/presentations/source occurrences. D1 now defines Material as a learner-facing **presentation occurrence/version** with traceable provenance.

Grouping fields preserve relations without erasing distinctions:

```text
work_group_id
content_group_id
```

### 2. Exactly one TaskType per Question was too restrictive — fixed

Authentic integrated questions can combine task forms. Final model:

```text
primary_task_type_id: exactly one
secondary_task_type_ids: zero or more
```

Secondary TaskTypes describe genuine integrated demands, not every incidental subskill.

### 3. Multi-text sources needed a canonical composition model — fixed

D1 uses acyclic ordered bundle Materials. The Question still references one Material while the bundle preserves the identities of component texts/charts.

### 4. Authenticity needed two separate fidelity layers — fixed

Material fidelity and Question-prompt fidelity are recorded separately. Adapted/generated practice remains useful but cannot masquerade as a verbatim original.

### 5. Reference answers/rubrics need provenance — fixed

Official/school/editorial/teacher/tutor-derived guidance remains distinguishable. This prevents an internal model answer from silently becoming “官方答案”.

### 6. `是否陌生迁移` cannot be static Question truth — fixed

Question may be a transfer-probe candidate, but actual unfamiliarity belongs to C2 Attempt because it depends on learner history.

### 7. Candidate curation and learner training state were conflated in v1 — separated

Shared content states such as `candidate/verified/active/retired` remain on content. Personal states such as `待练/需复练/已稳定/最近训练` move to C1/C2/future TrainingMove.

## Migration judgment

The current pool can be migrated without loss using one of five patterns:

1. **source only** → Material only, no fabricated Question;
2. **source + one prompt** → one Material + one Question;
3. **source + many prompts** → one Material + many Questions;
4. **same work, different presentation/source occurrence** → separate Materials + grouping;
5. **task/status mixed row** → TaskType/Question + learner history extraction into C1/C2.

The current Zhou Yafu 4.5/4.6 records demonstrate the intended split:

```text
one school Material occurrence
→ Q 4.5 character/evidence task
→ Q 4.6 comparison/foil task
```

while the student's actual 2026-09-14 responses remain TrainingAttempt evidence.

## Acceptance judgment

**PASS for D1 semantic architecture.**

The next dependency is Issue #6: define the task-complexity vector so Question complexity and C1 `verified_complexity_band` are no longer provisional labels.
