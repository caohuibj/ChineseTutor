# Question canonical schema

Status: **D1 normative draft — semantic self-review complete**

A `Question` is one concrete learner-facing prompt attached to one canonical Material and indexed by one primary TaskType plus optional secondary TaskTypes.

It is reusable across learners. Learner completion/failure/review state never belongs on Question.

---

## 1. Identity and structure

```yaml
question_id: Q-...
material_id: string
primary_task_type_id: string
secondary_task_type_ids: string[]
prompt: string
parent_question_id: string | null
source_question_number: string | null
question_group_id: string | null
variant_group_id: string | null
```

### One Material, many Questions

```text
MAT-zhouyafu
  -> Q-character-evidence
  -> Q-comparison-function
  -> Q-classical-translation-1
  -> Q-content-summary
```

Source content is stored once in the Material occurrence/version.

### Integrated TaskTypes

Most Questions have one clear TaskType. Complex authentic tasks may combine operations, so D1 keeps one primary TaskType for indexing plus optional secondary TaskTypes.

Secondary TaskTypes must reflect genuinely present task semantics, not every incidental subskill.

### Multi-text Question

A Question references one `material_id`. If several texts/charts are presented together, that Material is a bundle whose components preserve the individual source objects.

### Subquestions

Use `parent_question_id` when a source exam item has dependent subparts. If subparts have different TaskTypes/target nodes and can be scored or attempted separately, model separate Questions.

`question_group_id` preserves exam/item grouping without implying prerequisite semantics.

`variant_group_id` groups near-equivalent/adapted/generated variants for later diversity accounting; C2 Attempt snapshots it.

---

## 2. Prompt fidelity and provenance

```yaml
question_origin: official | school_provided | authoritative_simulation | editorial | adapted | generated | unknown
prompt_fidelity: verbatim | lightly_normalized | adapted | generated | unknown
adaptation_note: string | null
question_source_url: string | null
question_source_file_ref: string | null
question_provenance_note: string | null
```

### Authenticity rule

A reconstructed/adapted/generated prompt must not be represented as an original authentic exam question.

- `verbatim`: wording verified against source;
- `lightly_normalized`: typography/layout normalized without semantic change;
- `adapted`: changes may alter task behavior;
- `generated`: created by tutor/editor rather than extracted.

Adapted/generated variants are useful practice but should not substitute for genuine authentic-transfer validation.

---

## 3. Answer and rubric provenance

```yaml
reference_answer: string | null
reference_answer_source: official | school_provided | authoritative_editorial | teacher_authored | tutor_derived | none
rubric: string | null
rubric_source: official | school_provided | authoritative_editorial | teacher_authored | tutor_derived | none
official_score: number | null
scoring_note: string | null
```

A model/teacher-derived answer must never silently become “官方答案”.

---

## 4. Canonical graph target mapping

```yaml
knowledge_node_ids: string[]
ability_node_ids: string[]
strategy_node_ids: string[]
primary_target_node_ids: string[]
```

- Knowledge/Ability/Strategy lists describe nodes directly relevant to the concrete Question.
- `primary_target_node_ids` identifies the constructs this Question is deliberately curated to train/diagnose.
- Do not copy every transitive B2 prerequisite into every Question.
- Question mapping is editorial metadata; actual learner evidence is C2 `TrainingAttempt.node_evidence`.

Concrete Question mappings override broad TaskType default mappings.

---

## 5. Complexity placeholder

D1 carries complexity metadata because Questions need it, but Issue #6 defines the final vector semantics.

```yaml
complexity_band: null | C0 | C1 | C2 | C3 | C4 | C5
complexity_vector: object | null
complexity_status: unreviewed | provisional | reviewed
```

Rules:

- D1 does not define the calculation;
- legacy difficulty may seed `provisional` only;
- source grade never substitutes for complexity;
- C2 Attempts snapshot the values in force at attempt time.

---

## 6. Curation metadata

```yaml
curation_status: candidate | verified | active | retired
curation_note: string | null
```

This is shared editorial state only.

Forbidden learner-specific status:

```text
待练
已练
需复练
已稳定
最近训练
学生得分
个人提示等级
```

Those belong to C1/C2/future TrainingMove/queue layers.

---

## 7. Transfer/familiarity is learner-relative

Optional editorial field:

```yaml
transfer_probe_candidate: bool
```

This means the Question is suitable as a transfer probe when the learner's history makes it sufficiently unfamiliar.

Do not store absolute `是否陌生迁移=true` on Question. C2 Attempt records actual `material_familiarity` and `transfer_probe`.

---

## 8. Question invariants

1. Every Question points to exactly one canonical Material occurrence/version (atomic or bundle).
2. Every Question has exactly one primary TaskType and zero or more secondary TaskTypes.
3. Secondary TaskTypes represent genuine integrated task semantics, not incidental subskills.
4. Prompt provenance/fidelity is explicit.
5. Reference-answer/rubric provenance is explicit.
6. LearningNode mappings are editorial targets, not learner evidence.
7. Complexity is grade-independent and provisional until Issue #6 semantics are applied.
8. Learner training status never lives on Question.
9. Transfer unfamiliarity is determined per learner Attempt.
10. One Material can own many Questions without duplicate source content within that source occurrence.
11. Near-identical variants can be grouped so evidence diversity is not inflated.
