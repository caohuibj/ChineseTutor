# Material canonical schema

Status: **D1 normative draft**

A `Material` is the language/content object presented to the learner or used as the source stimulus for one or more Questions.

Examples:

- one modern essay or excerpt;
- one classical passage;
- one poem;
- a pair/set of texts presented together;
- a chart plus explanatory text;
- an interview transcript;
- a writing prompt source packet;
- a whole-book chapter/excerpt.

A Material is **not** a Question, Task Type, learner attempt, or training status.

---

## 1. Identity: exact presented object, not abstract work

```yaml
material_id: MAT-...
title: string
material_kind: atomic | bundle
```

The identity represents the exact learner-facing version.

If the same literary work appears with materially different excerpting, edits, annotations, charts, or paired texts, those are different Material records.

Optional grouping:

```yaml
work_group_id: string | null
```

`work_group_id` may group versions/excerpts of the same underlying work without pretending they are identical evidence contexts.

Material IDs are stable and independent of Notion page IDs or file names.

---

## 2. Content and composition

```yaml
material_type: prose | novel_excerpt | biography | informational | argumentative | classical | poem | poem_set | multi_text | chart_media | transcript | writing_source_packet | whole_book_excerpt | other
language_form: modern_chinese | classical_chinese | poetry | mixed | other
content_ref: string | null
content_hash: string | null
content_complete: bool | null
content_fidelity: verbatim | excerpted | lightly_normalized | adapted | unknown
adaptation_note: string | null
component_material_ids: string[]
```

### `content_ref`
Reference to the actual source content in an operational system: uploaded file, Notion page, permitted URL, file-library identifier, etc.

The canonical schema does not require copyrighted source text to be committed into GitHub.

### `content_hash`
Optional hash/fingerprint of the exact presented content/version for deduplication and provenance checks.

### `component_material_ids`
Used when `material_kind=bundle`.

Example:

```text
MAT-poetry-comparison-packet
  components:
  - MAT-poem-a
  - MAT-poem-b
```

Bundle rules:

- components remain independently identifiable Materials;
- bundle composition is ordered when presentation order matters;
- bundle graph must be acyclic;
- an atomic Material has no components.

A Question can therefore reference one Material while still supporting multi-text tasks through a bundle.

---

## 3. Authorship / literary metadata

Optional fields:

```yaml
author: string | null
era: string | null
genre_label: string | null
work_title: string | null
collection_title: string | null
```

These are descriptive metadata, not canonical LearningNodes. Literary/cultural knowledge is mapped separately through B1 nodes when relevant.

---

## 4. Provenance

Material provenance must remain traceable.

```yaml
source_type: textbook | school_material | school_exam | district_exam | zhongkao | gaokao | authoritative_simulation | authentic_external | editorial | generated | unknown
source_title: string | null
source_year: int | null
source_region_exam: string | null
source_grade: string | null
source_publisher_or_org: string | null
source_url: string | null
source_file_ref: string | null
source_reliability: official | authoritative | school | reputable_reprint | editorial | unknown
reprint_source: string | null
retrieved_at: datetime | null
provenance_note: string | null
rights_note: string | null
```

### Grade is metadata only

`source_grade` says where the material came from. It never controls prerequisite or learner progression.

### Reprint distinction

When an authentic exam is obtained through a republication rather than the original authority:

- retain the claimed original exam metadata;
- identify the republication source separately;
- do not silently upgrade `source_reliability` to official.

### Authenticity discipline

A Material labeled as official/authentic must have traceable source evidence. Memory-reconstructed or materially adapted text must not be presented as verbatim original.

---

## 5. Curation state vs learner state

Allowed editorial lifecycle:

```yaml
curation_status: candidate | verified | active | retired
```

This is **content curation state**, shared across learners.

Forbidden learner-specific fields:

```text
待练
已练
需复练
已稳定
最近训练
学生正确率
个人难度
是否陌生迁移（相对某个学生）
```

Those belong to TrainingAttempt/Profile/TrainingMove layers.

---

## 6. Material invariants

1. One exact presented source/version has one stable Material identity.
2. One Material may support many Questions without duplicating source content.
3. Multi-text packets use bundle composition rather than embedding duplicate texts in every Question.
4. Source grade is metadata only.
5. Adapted/excerpted content is labeled explicitly.
6. Traceability survives candidate→formal promotion.
7. Copyright/storage location is separate from semantic identity; GitHub need not contain full source text.
8. Material contains no learner-specific training status.
9. Bundle composition is acyclic.
10. Curation status is not learner mastery/training status.
