# Task complexity canonical schema

Status: **D2 normative draft**

Task complexity describes the **demands of a Question under stated conditions**, independent of source grade and independent of one learner's success/failure.

It is a vector, not a single difficulty label. A compact `C0-C5` band is a reviewed summary for routing/UI; recommendation logic should retain the full vector.

---

## 1. Core separation

Do not conflate:

```text
canonical task complexity
learner familiarity/novelty
learner mastery
empirical population difficulty
source grade/year
```

Examples:

- an eighth-grade source can contain a high-reasoning task;
- a Gaokao item can be straightforward on one dimension;
- an intrinsically C2 question can be an unfamiliar transfer probe for one learner and familiar practice for another;
- a learner can fail a C1 task because a prerequisite is missing without making the Question intrinsically C4.

---

## 2. Vector

Each dimension is `0..3` when reviewed and may be `null` while unreviewed/unknown.

```yaml
text_load: null | 0 | 1 | 2 | 3
information_hiddenness: null | 0 | 1 | 2 | 3
reasoning_depth: null | 0 | 1 | 2 | 3
material_heterogeneity: null | 0 | 1 | 2 | 3
knowledge_retrieval_distance: null | 0 | 1 | 2 | 3
response_openness: null | 0 | 1 | 2 | 3
expression_load: null | 0 | 1 | 2 | 3
time_pressure: null | 0 | 1 | 2 | 3
```

Question stores:

```yaml
complexity_vector: TaskComplexityVector
complexity_band: null | C0 | C1 | C2 | C3 | C4 | C5
complexity_status: unreviewed | provisional | reviewed
complexity_rationale: string | null
complexity_reviewed_by: string | null
complexity_reviewed_at: datetime | null
```

---

## 3. Dimension anchors

### 3.1 `text_load`
Amount/density/navigation burden of the presented language/material.

```text
0  phrase/sentence/very short item; trivial navigation
1  short passage or ordinary-length bounded material; ordinary syntax/density
2  long or dense passage / several sections; substantial navigation or complex syntax/concept density
3  very long/high-density source set; heavy navigation across many relevant regions
```

Long text alone does not imply deep reasoning.

### 3.2 `information_hiddenness`
How directly the needed information/relationship is available.

```text
0  explicit/local/directly stated
1  explicit but paraphrased, scattered, or requires simple selection
2  implicit relation requiring integration/inference across evidence
3  deeply implicit, competing, layered or ambiguous evidence requiring adjudication
```

### 3.3 `reasoning_depth`
Number/quality of inferential operations required for a valid response.

```text
0  recognition/recall/copy/direct extraction
1  one meaningful relation, transformation or rule application
2  multi-step reasoning such as evidence -> explanation -> conclusion, multi-condition synthesis, structured comparison
3  evaluation/modeling/counterfactual/hidden-premise/bounded judgment or sustained multi-layer reasoning
```

### 3.4 `material_heterogeneity`
Number and representational heterogeneity of source objects that must be coordinated.

```text
0  one local material/object
1  one extended material or multiple homogeneous parts requiring coordination
2  multiple texts/materials with meaningful cross-source integration
3  heterogeneous/cross-media source set, competing representations, or several materials requiring model reconciliation
```

This is not simply a raw count; two adjacent lines are not “multi-material”.

### 3.5 `knowledge_retrieval_distance`
How far the task requires the solver to retrieve/use knowledge not directly supplied by the immediate prompt/material.

```text
0  no external canonical knowledge needed beyond directly supplied information
1  one directly cued/common concept/rule/procedure
2  several prerequisites or an uncued but nearby canonical knowledge/strategy family
3  remote/cross-domain/abstract theory/cultural-system knowledge or a rule that must be recognized and transferred without local cue
```

This is a **task-demand property**, not distance from this particular learner's memory. Actual learner familiarity/mastery remains C1/C2 state.

### 3.6 `response_openness`
Number of acceptable solution/interpretation pathways and degree of judgment required.

```text
0  fixed/closed answer or tightly objective transformation
1  bounded constructed response with narrow scoring space
2  several defensible interpretations/organizations within clear constraints
3  open evaluation/design/argumentation/construction requiring justified choices and boundary control
```

### 3.7 `expression_load`
Amount and organization of output required after thinking is complete.

```text
0  choice/word/phrase/minimal edit
1  one or a few sentences / concise answer
2  structured paragraph, multi-part explanation, or sustained local reasoning
3  extended composition / multi-paragraph construction / substantial revision artifact
```

Expression load is separate from reasoning depth. A long answer can contain shallow reasoning; a short answer can require deep reasoning.

### 3.8 `time_pressure`
Constraint imposed by expected time relative to input/output demand.

```text
0  untimed or ample time; speed not meaningfully tested
1  ordinary classroom/assignment timing
2  standardized constrained timing where fluent execution matters
3  unusually tight timing relative to load; high automation demand
```

`time_pressure` is an operational modifier. It **does not independently raise semantic C-band**; it primarily matters to C1 Automation and recommendation.

---

## 4. Learner-relative transfer is separate

The original architecture shorthand used `C4 = unfamiliar transfer`. D2 deliberately corrects that coupling.

Final separation:

```text
Complexity band = intrinsic task-demand summary
Transfer evidence = learner-relative Attempt condition/outcome
```

C2 already records:

```text
material_familiarity
transfer_probe
question_variant_group_id
```

C1 M3 uses stable transfer/generalization evidence, not merely a C4 label.

A C2 task can be an unfamiliar transfer probe. A C4 task can be familiar to the learner. Both are valid.

---

## 5. C0-C5 summary band

The band is an interpretable anchor, not a weighted score. Review the full vector first, then assign the **lowest band whose description adequately captures the dominant semantic demand**.

`time_pressure` may affect recommendation/automation but cannot by itself promote a band.

### C0 — recognition / direct retrieval

Typical profile:

- reasoning depth 0;
- hiddenness 0-1;
- response openness 0;
- expression load 0-1;
- little integration.

Examples: direct word recognition, explicit fact selection, simple memorized reproduction.

### C1 — single-step application

Typical profile:

- one meaningful rule/relation/transformation;
- bounded material;
- response mostly closed/narrow;
- no sustained multi-step inferential chain.

Examples: simple sentence correction, direct contextual choice, one-rule language use.

### C2 — multi-step explanation

Typical profile includes at least one of:

- reasoning depth 2;
- hiddenness 2 with explicit explanation;
- structured comparison/evidence-warrant chain;
- moderate knowledge retrieval with bounded response.

Usually single/bounded material and limited openness.

Examples:人物证据→品质解释、常规诗歌情感证据链、文言翻译中多要素落实.

### C3 — integrated multi-node / multi-source task

Typical profile:

- several dimensions at level 2;
- coordinated use of multiple nodes/material sections;
- multi-text integration or structured synthesis;
- response remains meaningfully constrained.

Examples:多文本同异整合、较复杂结构作用、综合文言内容与观点解释.

### C4 — high integration / remote application / constrained evaluation

Typical profile includes:

- reasoning depth 3 with additional hiddenness/material/knowledge demand >=2; or
- several level-2/3 dimensions requiring high integration;
- remote rule/theory application, implicit-premise reasoning, constrained evidence evaluation, complex model reconstruction.

C4 is often a good transfer-probe candidate, but **unfamiliarity is not part of the band definition**.

### C5 — open evaluation / complex construction

Typical profile includes:

- response openness 3; and
- reasoning depth 3 and/or expression load 3 with substantial organization;
- multiple valid paths requiring justified choices, boundary control, sustained construction or complex revision.

Examples:开放评价 with competing interpretations; full complex argumentative/material writing; high-level solution/design communication.

---

## 6. Band assignment guardrails

### Guardrail A — long != complex
`text_load=3` with direct extraction can remain C1/C2.

### Guardrail B — time != semantic complexity
`time_pressure=3` can turn a C1 task into a demanding automation check while semantic band remains C1.

### Guardrail C — one high dimension does not always dominate
`knowledge_distance=3` on a fixed recall item may still be low semantic reasoning complexity, though knowledge burden is high. Preserve the vector.

### Guardrail D — C5 requires openness/construction, not merely many dimensions
A dense multi-text inference item can be C4 without becoming C5.

### Guardrail E — transfer remains separate
Do not assign C4 simply because the learner has never seen the material.

---

## 7. Recommendation semantics

Future TrainingMove selection should use the vector, not only C-band.

Examples:

- weak reading navigation -> avoid jumping `text_load` from 1 to 3 even if reasoning band stays C2;
- strong reasoning but weak writing expression -> hold reasoning depth steady while raising `expression_load` deliberately;
- automate a method -> hold semantic band constant and increase `time_pressure` / reduce hints;
- transfer validation -> keep semantic demand comparable while switch material familiarity to unfamiliar and vary TaskType/material family.

This enables controlled training moves instead of generic “harder question”.

---

## 8. D2 invariants

1. Complexity describes Question demands, not learner performance.
2. Grade/year is not a vector dimension or band rule.
3. Learner familiarity/transfer is not encoded in the canonical C-band.
4. All eight dimensions remain visible even when a band is assigned.
5. Time pressure does not independently promote semantic band.
6. Expression load and reasoning depth remain separate.
7. Material count/heterogeneity is semantic integration, not raw number of pages.
8. Legacy difficulty labels migrate as provisional only.
9. Reviewed complexity includes rationale/provenance.
10. Recommendation logic may manipulate dimensions independently rather than only escalating C0→C5.
