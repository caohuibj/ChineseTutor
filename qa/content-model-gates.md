# QA gates — Material / TaskType / Question (D1)

These gates are normative acceptance checks for Issue #5.

## Gate 1 — Source object and prompt are different entities

One passage with three questions must produce:

```text
1 Material
3 Questions
```

not three duplicated Materials containing the same source text.

---

## Gate 2 — Material identity represents a presented source occurrence/version

Two records may share the same underlying work or even the same content hash while still be different Materials when presentation/source context differs materially (different excerpting, annotations, paired media, exam packet, etc.).

Use grouping metadata to connect them; do not erase provenance distinctions for deduplication convenience.

---

## Gate 3 — Multi-text tasks use Material bundles

A comparison question over two poems should reference one bundle Material whose ordered components are the two atomic poem Materials.

Fail if the Question duplicates both poem texts or has unstructured free-text source references.

Bundle composition must be acyclic.

---

## Gate 4 — Copyright/storage is not semantic identity

Material may reference source content through file/Notion/URL identifiers. Full copyrighted text is not required in GitHub.

Fail if moving a source file changes the semantic Material ID.

---

## Gate 5 — Provenance survives promotion

Candidate → verified/active promotion must retain traceable source type, source title/organization, year/region when known, and original/reprint/file references.

Fail if promotion leaves only a copied prompt with no provenance.

---

## Gate 6 — Reprints are not silently official

If an exam item is obtained from a republication rather than an official authority, record republication provenance and appropriate reliability.

Fail if a third-party copy is labeled official merely because it claims an official exam name.

---

## Gate 7 — Adapted/generated content is labeled honestly

Fail if:

```text
prompt_fidelity=verbatim
```

for a reconstructed/adapted/generated prompt.

Likewise, materially adapted source text must not be labeled verbatim.

---

## Gate 8 — Answer/rubric provenance is explicit

A tutor-derived answer must never appear as `official`.

Pass:

```text
reference_answer_source=tutor_derived
```

when no official key is available.

---

## Gate 9 — TaskType is not Ability

Fail if `人物形象分析题` is inserted as a canonical LearningNode only to preserve an exam label.

TaskType may map to several underlying canonical abilities.

---

## Gate 10 — One Strategy per TaskType is not automatic

Existing `3—5步算法` may map to a reusable mother Strategy, remain TaskType guidance, or be merged in G1.

Fail if migrating 35 TaskTypes automatically creates 35 Strategy nodes.

---

## Gate 11 — Concrete Question mapping overrides TaskType defaults

TaskType defaults are broad hints. A specific Question's target nodes are authoritative for what it was curated to train/diagnose.

Fail if every Question of one TaskType is forced to inherit all default nodes as actual targets.

---

## Gate 12 — Integrated questions can have a primary and secondary TaskTypes

Complex authentic questions may combine operations. The model must support one primary TaskType for indexing plus optional secondary TaskTypes.

Fail if a multi-operation question must be duplicated solely to attach a second TaskType.

---

## Gate 13 — Question contains no learner status

Reject fields such as:

```text
待练
已练
需复练
已稳定
最近训练
个人得分
个人提示等级
```

from canonical Question.

---

## Gate 14 — Material contains no learner status

Curation lifecycle is allowed; learner training lifecycle is not.

Fail if `Material.status=已掌握`.

---

## Gate 15 — Static Question cannot declare learner unfamiliarity

`transfer_probe_candidate=true` is allowed.

Absolute `是否陌生迁移=true` is not, because familiarity depends on learner history.

Actual familiarity belongs to C2 Attempt.

---

## Gate 16 — Grade is provenance metadata only

Reject grade as TaskType identity, prerequisite, or Question complexity substitute.

Pass:

```text
source_grade=初三
complexity_status=provisional
```

without assuming a fixed C-band from grade.

---

## Gate 17 — Complexity remains provisional until Issue #6

Legacy `基础/常规/提升/迁移` may seed provisional metadata but cannot become the final vector without E1 review.

---

## Gate 18 — Subquestion decomposition preserves scoring/task semantics

If one exam number has subparts with different TaskTypes or target nodes, model separate Questions linked by parent/group IDs rather than one giant prompt with ambiguous targets.

---

## Gate 19 — Candidate source without a real prompt does not fabricate a Question

A useful source-only candidate may create/resolve a Material but no Question until an actual prompt is extracted/verified or an explicitly generated prompt is authored and labeled.

---

## Gate 20 — Same authentic prompt found on multiple websites is not automatically duplicated

Resolve against original exam/source identity and best provenance. Third-party mirrors do not create new semantic Questions by themselves.

---

## Gate 21 — Same underlying work in materially different presentation remains distinguishable

Textbook excerpt vs shortened exam excerpt may share `work_group_id` but remain different Materials if evidence context differs.

---

## Gate 22 — Existing Zhou Yafu records map without learner-state leakage

Expected:

```text
one Zhou Yafu Material
Question 4.5
Question 4.6
```

with 2026-09-14 responses remaining TrainingAttempt events, not Question state.

---

# D1 draft definition of done

- [x] Material schema defined;
- [x] TaskType schema defined;
- [x] Question schema defined;
- [x] exact-version/source provenance and bundle semantics defined;
- [x] prompt/answer/rubric provenance defined;
- [x] Question-to-canonical-node mapping defined;
- [x] learner-state exclusion explicit;
- [x] candidate/formal bank migration design defined;
- [x] current Zhou Yafu seed migration explained;
- [x] complexity explicitly deferred to Issue #6 while retaining provisional fields;
- [ ] semantic self-review fixes integrated;
- [ ] stacked PR opened.
