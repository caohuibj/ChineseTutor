# ADR-009 — Separate Material, TaskType and Question

Status: **proposed in D1; semantic self-review complete**

## Context

The current candidate pool and formal training bank sometimes use one row to represent source text, question wording, task type, ability targets, provenance, curation state and learner training status.

That shape creates several problems:

- the same source passage is duplicated across questions;
- source provenance drifts between copies;
- Task Type labels masquerade as Abilities;
- learner-specific `待练/已练/需复练` becomes attached to reusable content;
- adapted/generated prompts can lose authenticity labels;
- one final “是否陌生迁移” flag ignores learner familiarity;
- answer keys may appear authoritative without provenance.

## Decision

ChineseTutor separates three reusable canonical entities:

```text
Material
TaskType
Question
```

### Material
The exact learner-facing source occurrence/version, including provenance and presentation composition.

### TaskType
The reusable authentic problem form — how the learner is asked to operate — separate from underlying Knowledge/Ability/Strategy.

### Question
One concrete prompt attached to one Material, indexed by one primary TaskType plus optional secondary TaskTypes, with explicit target nodes and answer/rubric provenance.

Learner-specific history remains in C1/C2/future TrainingMove layers.

## Material identity is source-occurrence aware

A Material is not merely “the abstract essay”. The same underlying work may produce different Materials when source/presentation context can change evidence meaning.

Examples:

```text
textbook excerpt
vs
shortened district-exam excerpt
```

may share `work_group_id` but remain separate Materials.

Even identical text appearing in different source occurrences may remain distinct and share `content_group_id`; provenance is never erased for deduplication convenience.

## Multi-text is represented by bundle Material

Questions continue to reference one Material. A poem comparison or cross-media packet is modeled as a bundle with ordered component Materials.

This keeps source objects reusable and traceable without duplicating them in Question records.

## Integrated Questions support secondary TaskTypes

The first D1 draft assumed exactly one TaskType per Question. Review rejected that as too restrictive for authentic integrated tasks.

Final model:

```text
primary TaskType: exactly one
secondary TaskTypes: zero or more
```

Primary TaskType drives indexing; secondary types describe genuinely integrated task demands, not incidental subskills.

## Authenticity and fidelity are explicit

Material content and Question prompt separately record whether they are:

```text
verbatim
excerpted/lightly normalized
adapted
generated
unknown
```

An adapted/generated item can still be useful training, but cannot be silently presented as an original authentic question.

## Answer/rubric provenance is explicit

Official, school-provided, teacher-authored, editorial and tutor-derived answers/rubrics remain distinguishable.

A useful tutor-derived answer is not “官方答案”.

## Transfer unfamiliarity is learner-relative

Question may be tagged as a `transfer_probe_candidate`, but actual unfamiliarity/transfer status belongs to C2 Attempt because it depends on learner history.

## Candidate staging is not learner state

Candidate/verified/active/retired are shared curation states. `待练/需复练/已稳定` are personal states and must not appear on canonical Material/Question.

## Complexity is carried but not finalized

Question retains provisional complexity metadata so the content model can operate, but Issue #6 defines the final vector semantics. Source grade cannot substitute for complexity.

## Consequences

### Positive

- source passages are reusable across many questions;
- provenance/authenticity becomes auditable;
- candidate promotion can preserve source identity;
- TaskType analytics and node analytics stay conceptually separate;
- C2 evidence can reference stable Questions while retaining learner-specific state;
- generated/adapted practice can coexist honestly with authentic transfer questions;
- copyright storage can remain external to GitHub schemas.

### Costs

- migration is one-to-many rather than row-copying;
- bundles/grouping require some additional identifiers;
- old learner status must be migrated before retiring mixed v1 rows;
- final complexity tagging waits on Issue #6.

## Rejected alternatives

### One record = passage + question + learner status
Rejected because reusable content and personal history have different lifecycles.

### Material identity = abstract work title only
Rejected because excerpt/presentation/source context affects evidence and provenance.

### Material identity = content hash only
Rejected because identical text can occur in different traceable source contexts; equal content can be grouped without erasing occurrence provenance.

### Exactly one TaskType per complex Question
Rejected during self-review because authentic integrated tasks can combine task forms. One primary plus optional secondary types preserves indexing and fidelity.

### Static `是否陌生迁移`
Rejected because familiarity is learner-specific.

### Assume every reference answer is official
Rejected because answer provenance materially affects trust and grading interpretation.
