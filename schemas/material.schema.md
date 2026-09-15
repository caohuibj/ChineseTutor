# Material canonical schema

Status: **D1 normative draft — semantic self-review complete**

A `Material` is the language/content object presented to the learner or used as the source stimulus for one or more Questions.

Examples include one essay/excerpt, one classical passage, one poem, a text set, chart/media packet, transcript, writing-source packet, or whole-book excerpt.

A Material is **not** a Question, Task Type, learner attempt, or training status.

---

## 1. Identity: presented source occurrence/version, not abstract work

```yaml
material_id: MAT-...
title: string
material_kind: atomic | bundle
work_group_id: string | null
content_group_id: string | null
```

Material identity represents the exact learner-facing **presentation occurrence/version** in a traceable source context.

Create different Materials when any of these can change evidence context:

- excerpt boundaries;
- edits/normalization beyond trivial formatting;
- annotations;
- paired media/texts;
- source/exam packet context;
- presentation order in a bundle.

Thus the same underlying essay may appear as separate Materials in a textbook and a district exam even if much of the text is identical. They may share grouping metadata and/or content references.

### Grouping

- `work_group_id` groups versions/excerpts of the same underlying literary/source work.
- `content_group_id` may group byte/text-equivalent or near-identical presented content across occurrences when useful for familiarity/deduplication analytics.

Grouping never erases source provenance.

Material IDs are independent of Notion page IDs, URLs and local file names.

---

## 2. Content and composition

```yaml
material_type: prose | novel_excerpt | biography | informational | argumentative | classical | poem | poem_set | multi_text | chart_media | transcript | writing_source_packet | whole_book_excerpt | other
language_form: modern_chinese | classical_chinese | poetry | mixed | other
content_ref: string | null
content_hash: string | null
content_complete: bool | null
content_fidelity: verbatim | excerpted | lightly_normalized | adapted | generated | unknown
adaptation_note: string | null
component_material_ids: string[]
```

### `content_ref`
Reference to the actual source content in an operational system: uploaded file, Notion page, permitted URL, file-library identifier, etc. Copyrighted text need not be committed into GitHub.

### `content_hash`
Optional fingerprint of exact presented content for provenance/dedup checks. Equal hashes do not automatically imply one Material because source occurrence/context may differ.

### Bundle composition

When `material_kind=bundle`, `component_material_ids` is an ordered list when presentation order matters.

Rules:

- components remain independently identifiable Materials;
- bundle composition is acyclic;
- atomic Materials have no components;
- a Question references the bundle rather than duplicating component content.

---

## 3. Descriptive literary metadata

Optional:

```yaml
author: string | null
era: string | null
genre_label: string | null
work_title: string | null
collection_title: string | null
```

These are descriptive metadata, not canonical LearningNodes.

---

## 4. Provenance

```yaml
source_type: textbook | school_material | school_exam | district_exam | zhongkao | gaokao | authoritative_simulation | authentic_external | editorial | generated | unknown
source_title: string | null
source_year: int | null
source_region_exam: string | null
source_grade: string | null
source_publisher_or_org: string | null
source_url: string | null
source_file_ref: string | null
source_reliability: official | authoritative | school | reputable_reprint | editorial | generated | unknown
reprint_source: string | null
retrieved_at: datetime | null
provenance_note: string | null
rights_note: string | null
```

### Grade is metadata only
`source_grade` never controls prerequisite, complexity or progression.

### Reprints
When an authentic exam/source is obtained from a republication:

- retain claimed original exam/source metadata;
- record the republication separately;
- do not silently label the retrieved copy `official` unless official provenance was actually verified.

### Authenticity discipline
Memory-reconstructed, adapted or generated content must be labeled accordingly. Source fidelity cannot be inferred from a familiar title alone.

---

## 5. Curation vs learner state

Allowed shared editorial lifecycle:

```yaml
curation_status: candidate | verified | active | retired
```

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

These belong to C1/C2/future TrainingMove layers.

---

## 6. D1 invariants

1. Material identity is a traceable presented source occurrence/version, not an abstract work title.
2. One Material may support many Questions without duplicating source content within that occurrence.
3. Equal/near-equal content across different source contexts may remain distinct Materials and be connected by grouping metadata.
4. Multi-text packets use acyclic ordered bundle composition.
5. Source grade is metadata only.
6. Adapted/generated/excerpted content is labeled honestly.
7. Candidate→formal promotion preserves provenance.
8. Storage location/copyright handling is separate from semantic identity.
9. Material contains no learner-specific training status.
10. Curation state is not learner mastery/training state.
