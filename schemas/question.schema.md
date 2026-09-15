# Question canonical schema

Status: **D1 normative draft**

A `Question` is one concrete learner-facing prompt attached to one canonical Material and one primary TaskType.

It is reusable across learners. Learner completion/failure/review state never belongs on Question.

---

## 1. Identity and structure

```yaml
question_id: Q-...
material_id: string
task_type_id: string
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

Source passage/content is stored once in Material.

### Multi-text Question

A Question still references one `material_id`. If several texts/charts are presented together, that Material is a `bundle` whose components preserve the individual source objects.

### Subquestions

Use `parent_question_id` when a source exam item contains dependent subparts. If subparts have different TaskTypes/target nodes and can be assessed separately, model each as a separate Question.

`question_group_id` can preserve exam/item grouping without implying semantic dependency.

`variant_group_id` groups near-equivalent or generated/adapted variants for later diversity accounting. C2 Attempt may snapshot this value.

---

## 2. Prompt fidelity and provenance

```yaml
question_origin: official | school_provided | authoritative_simulation | editorial | adapted | generated | unknown
prompt_fidelity: verbatim | lightly_normalized | adapted | generated | unknown
adaptation_note: string | null
question_source_url: string | null
question_source_file_ref: string | null
```

### Authenticity rule

A Question must not be presented as an original authentic exam question when the prompt was reconstructed, adapted, or generated.

- `verbatim`: wording verified against source;
- `lightly_normalized`: typography/layout normalized without semantic change;
- `adapted`: wording/content changed in a way that may alter task behavior;
- `generated`: created by tutor/editor rather than extracted from a source.

Authentic training may use adapted/generated variants, but they must be labeled honestly and should not substitute for actual transfer validation on real questions.

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

This distinction is required because a model-derived answer must never silently become “官方答案”.

When official scoring guidance is unavailable, a teacher/tutor rubric can still be useful, but its provenance remains explicit.

---

## 4. Canonical graph target mapping

```yaml
knowledge_node_ids: string[]
ability_node_ids: string[]
strategy_node_ids: string[]
primary_target_node_ids: string[]
```

### Mapping semantics

- `knowledge_node_ids`: Knowledge likely required/observed by the Question;
- `ability_node_ids`: observable operations exercised;
- `strategy_node_ids`: Strategies that the task may validly reveal/use;
- `primary_target_node_ids`: the nodes this Question is deliberately curated to train or diagnose.

Not every relevant prerequisite must be copied onto the Question if B2 graph traversal can derive it. Add explicit mappings when the Question directly tests or uniquely requires a node.

Question mapping is static editorial metadata; actual learner evidence is C2 `TrainingAttempt.node_evidence`.

---

## 5. Complexity placeholder

D1 carries complexity fields because Questions need a difficulty/context description, but Issue #6 defines the final complexity model.

```yaml
complexity_band: null | C0 | C1 | C2 | C3 | C4 | C5
complexity_vector: object | null
complexity_status: unreviewed | provisional | reviewed
```

Rules:

- D1 does not define the numerical/vector calculation;
- coarse legacy difficulty may migrate as `provisional` only;
- source grade never substitutes for complexity;
- C2 Attempts snapshot the complexity values used at the time.

---

## 6. Curation metadata

```yaml
curation_status: candidate | verified | active | retired
curation_note: string | null
```

This is shared editorial state only.

Allowed examples:

```text
candidate — source/question needs verification
verified — provenance and prompt checked
active — available for recommendation/training
retired — retained historically but not selected for new training
```

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

Those live in TrainingAttempt/Profile/TrainingMove/queue layers.

---

## 7. Transfer/familiarity is not a static Question property

A Question may be curated as useful for transfer testing, but whether it is actually **unfamiliar** is learner-specific.

Optional editorial field:

```yaml
transfer_probe_candidate: bool
```

This means the Question is suitable as a transfer probe when learner history makes it unfamiliar enough.

Do not store:

```text
是否陌生迁移 = true
```

as an absolute learner-independent fact.

C2 Attempt records actual `material_familiarity` and `transfer_probe` for that learner/event.

---

## 8. Question invariants

1. Every Question points to exactly one canonical Material (atomic or bundle).
2. Every Question has one primary TaskType.
3. Prompt provenance/fidelity is explicit.
4. Reference-answer/rubric provenance is explicit.
5. LearningNode mappings are editorial targets, not learner evidence.
6. Complexity metadata is independent of grade and remains provisional until Issue #6 semantics are applied.
7. Learner training status never lives on Question.
8. Transfer unfamiliarity is determined per learner Attempt, not statically.
9. One Material can own many Questions without source duplication.
10. Near-identical variants can be grouped so evidence diversity is not inflated.
