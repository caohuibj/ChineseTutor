# TrainingMove → Question selection contract

Status: **F1 normative draft**

F1 recommends an evidence-producing training action. It must not collapse into “pick the highest-scoring question”.

Question selection is a second stage:

```text
TrainingMove constraints
        ↓
D1 Question/Material inventory
        ↓
eligibility filter
        ↓
quality + fit ranking
        ↓
selected Question
```

---

## 1. Hard eligibility checks

A Question is ineligible for the move when any required condition fails:

- primary target cannot be validly observed by the Question;
- TaskType is incompatible with the move's preferred/allowed TaskTypes;
- D2 vector exceeds an explicit max constraint or misses a required minimum;
- material is too familiar for a required transfer probe;
- Question/variant is explicitly excluded by recent-history constraints;
- provenance/fidelity is insufficient for an evidence-validation move;
- response mode makes the target construct unobservable;
- a hard prerequisite blocker makes downstream independent evidence invalid;
- the Question is retired or otherwise unavailable.

Do not “score up” an ineligible Question.

---

## 2. Fit ranking after eligibility

Among eligible Questions, prefer in this order when relevant:

1. target-node fidelity;
2. desired D2 vector fit;
3. hint/scaffold compatibility;
4. diversity from recent variant/material history;
5. provenance/fidelity appropriate to evidence purpose;
6. efficient response load;
7. school-material relevance as tie-breaker;
8. source-priority preference.

---

## 3. Authentic vs generated/editorial probes

Use authentic traceable Questions preferentially when:

- validating transfer/generalization;
- calibrating high-stakes exam behavior;
- claiming stable performance against real task conventions.

Short generated/editorial probes are appropriate when:

- diagnosing a narrow prerequisite;
- verifying a single knowledge distinction cheaply;
- fading scaffold with controlled complexity;
- isolating one expression/reasoning failure.

Generated probes must remain labeled as generated/adapted under D1 and cannot silently substitute for all authentic-transfer evidence.

---

## 4. Familiarity determination

Question/Material is not intrinsically “unfamiliar”. Determine learner-relative familiarity from history:

```text
same exact Question seen recently -> familiar
same Material, different Question -> partially familiar depending on target
same variant group -> likely too familiar for transfer diversity
same TaskType but meaningfully new Material -> may be unfamiliar
new domain representation exposing same canonical Ability -> strong transfer diversity candidate
```

C2 Attempt records final `material_familiarity` and `transfer_probe` status.

---

## 5. Controlled variable changes

For efficient training, prefer changing only the dimensions relevant to the move.

Examples:

### Fade scaffold
Keep:

```text
reasoning_depth ~= current
text_load ~= current
TaskType similar
```

Change:

```text
H3 -> H1/H0
```

### Expression training
Keep reasoning demand stable; raise `expression_load` or require explicit written warrant.

### Complexity extension
Raise one or two intended D2 dimensions, not all dimensions accidentally.

### Transfer probe
Keep intrinsic demand near established range; change Material/domain/task surface and enforce unfamiliarity.

---

## 6. No suitable Question

If no eligible Question exists:

```text
TrainingMove need remains valid
Question selection returns no_match
```

Then:

- emit a content-gap request for candidate-material search/authoring;
- optionally choose the next-best executable TrainingMove;
- never lower/alter Profile need merely because inventory is missing.

---

## 7. Selection result record

Suggested operational output:

```yaml
training_move_id: TM-...
selected_question_id: Q-... | null
selection_status: selected | no_match | manually_selected
fit_reasons: string[]
tradeoffs: string[]
selection_policy_version: string
```

This object is scheduling/execution metadata, not Profile evidence.

---

## 8. Contract invariants

1. Eligibility precedes ranking.
2. Question abundance never creates learner need.
3. Transfer validation must account for learner-relative familiarity.
4. Generated probes are valid for efficient diagnosis but do not masquerade as authentic questions.
5. Controlled training changes relevant dimensions deliberately.
6. Missing inventory creates a content-supply gap, not a false learner-state change.
7. The concrete Question remains replaceable while the TrainingMove semantics stay stable.