# I1 semantic self-review — Gaokao coverage audit

## Scope reviewed

Reviewed the representative mapping, gap register, B1/B2/D2/G1 boundaries and the existing project source synthesis before opening the I1 PR.

## Findings and corrections

### 1. Do not overclaim exact question metadata

The project source synthesis exposes several demand descriptions without stable question numbers/stems in the fetched text.

Correction:

- fixtures use paraphrased `demand_label`;
- `question_no: unknown_in_current_extraction` is explicit;
- I1 is a coverage fixture, not a reconstructed question bank.

### 2. “Cleanly mapped” means no new semantic construct is needed

The production canonical graph is not yet exhaustively populated.

Therefore `cleanly_mapped` in I1 means:

> the demand can be represented by already-established node/strategy semantics without introducing a new recurring construct.

It does **not** claim every final Notion/GitHub row already exists.

Broad Knowledge population remains #35.

### 3. Representation transformation is not only a TaskType

Initial risk: treat text→table or prose→Q&A as merely output format.

Review judgment:

A learner may select all correct information yet fail to preserve hierarchy/relations during conversion. This creates a distinct diagnosis/intervention.

Result: keep candidate Ability gap #29 while output-specific conventions stay in TaskType.

### 4. Rule induction and rule application must remain separable

Initial risk: call both “迁移”.

Review judgment:

```text
cannot derive principle
!=
derives principle correctly but misapplies it to new conditions
```

Result: one follow-up issue (#30) with two candidate Ability nodes/operations, pending canonical naming/dependency work.

### 5. Evidence sufficiency is the highest-centrality uncovered Ability

It recurs in modern/information reading, classical interpretation, open evaluation and argumentative reasoning.

Result: #31 marked critical; do not hide it inside S5 Strategy or generic “思辨”.

### 6. Question formulation is distinct from generic expression

Reverse-question and interview design share an information-goal operation.

Result: retain #32 as an Ability candidate; TaskType controls interview/reverse-question output conventions.

### 7. Constraint-solution synthesis does not collapse into rule application

2026 Beijing-like multi-material planning may require satisfying several constraints even when there is no single abstract rule to transfer.

Result: retain separate #33.

### 8. TaskType gap was broadened rather than forcing unrelated tasks into COM

Initial draft grouped interview, Q&A transformation and literary micro-commentary under “real communication”.

Correction:

Issue #34 now covers **authentic higher-order TaskType inventory** while preserving domain distinctions:

- interview/Q&A -> COM;
- literary micro-commentary -> MRD;
- no single generic open-response TaskType.

### 9. No new mother Strategy was justified

Every representative demand could be expressed using G1 mother Strategies singly or in small composition.

Result:

- no `采访提问策略`;
- no `表格转换五步法`;
- no `牡丹方案法`;
- no `同意象比较模板`.

Novel surface form is handled through TaskType + Ability + existing Strategy composition.

### 10. D2 complexity vector remained sufficient

No recurring task required adding:

```text
Gaokao-level
novelty
transfer
source grade
```

as complexity dimensions.

Novelty remains learner-relative Attempt context.

### 11. Terminal coverage is not enough for adaptive training

A Gaokao exemplar proves a terminal demand exists but does not supply an instructional ladder.

Result: explicit material-coverage Issue #36 for authentic complexity ladders and variant diversity.

### 12. Architecture verdict is intentionally conservative

I1 found no top-level schema/entity gap **within the representative corpus**.

This is falsifiable, not a declaration that future exam innovation can never require revision. QA explicitly requires reopening architecture analysis if a future authentic task cannot map cleanly.

## Final self-review verdict

I1 is ready to open as a stacked PR on G1 when:

- all recurring candidate gaps have explicit issues (#29–#36);
- QA DoD marks self-review complete;
- the PR description states that question-level exactness is limited by the currently extracted source metadata.
