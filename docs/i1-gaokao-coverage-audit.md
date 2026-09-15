# I1 Gaokao coverage audit — 2022–2026 Beijing + 2025–2026 National representative demands

Status: **semantic audit complete — representative, not exhaustive**

## 1. Executive verdict

The current ChineseTutor architecture is structurally capable of representing the representative recent Gaokao demands examined in I1.

The audit found **no new top-level entity type is required** beyond:

```text
Material
TaskType
LearningNode {Knowledge | Ability | Strategy}
LearningEdge
TaskComplexity
LearnerNodeState
TrainingMove
TrainingAttempt
InterventionDecision
ReviewDecision
```

The important gaps are not architectural collapse. They are concentrated in:

1. missing canonical Ability nodes for several high-order recurring operations;
2. incomplete high-school Knowledge population;
3. incomplete authentic TaskType coverage for real-communication/high-school forms;
4. insufficient authentic complexity ladders per high-value node.

This is a positive result: the graph-first model survives Gaokao stress-testing without reverting to grade progression or one-question-one-template design.

## 2. Corpus and source basis

Representative demand fixtures are derived from the project's existing imported corpus and synthesis:

- Beijing 2022–2026 papers/answers index;
- Beijing official/authoritative analyses;
- 2025–2026 National I/II papers/analyses;
- project synthesis `高考真题能力证据提取｜2022—2026北京＋2025—2026全国`.

The fixture file intentionally paraphrases demand semantics and avoids inventing exact question numbers where the current extraction does not expose them.

See:

- `coverage/gaokao/audit-protocol.md`
- `coverage/gaokao/representative-mapping.yaml`
- `coverage/gaokao/gap-register.yaml`

## 3. What mapped cleanly

### 3.1 Literary expression analysis

2026 Beijing's literary demand to explain the concrete effect of patterned/repeated sentence groups maps cleanly to:

```text
TaskType: 文学词句/句群赏析
Strategy: S9 关键词句—内容—写法—具体效果—意义
          + S1 证据—解释—结论
Knowledge: 修辞/句群/语境知识
Ability: contextual effect explanation
Complexity: bounded multi-step explanation (C2-like)
```

No exam-specific “排比反复题模板” is needed.

### 3.2 Literary attitude/understanding trajectory

The 2026 Beijing demand to trace changes in the author's understanding of a person can be expressed compositionally:

```text
S4 定位—分类—合并—压缩
+
S13 关系链
```

The key learner operations are locating phase evidence, ordering it, and explaining change relations. I1 found no evidence that a separate permanent Strategy is required.

### 3.3 Poetry comparison

The 2026 Beijing comparison of the same image across different poems maps cleanly to:

```text
S2 比较维度—分别取证—同异—意义
+
S6 景事人—关键词—情绪方向—对象—验证
+
S9 局部赏析（when effect/form matters）
```

This validates G1's decision that comparison is a reusable mother Strategy rather than separate “两首诗意象比较法”.

### 3.4 Textbook-to-unfamiliar comparison

2025 National I's comparison with `种树郭橐驼传` confirms the architecture's separation:

```text
canonical Knowledge retrieval
+ comparison/evidence Abilities
+ learner-relative transfer condition
```

“课内外迁移” remains an evidence condition, not a Strategy node or complexity band.

### 3.5 Full writing

Recent Beijing/National writing demands remain representable by the current writing/strategy architecture:

```text
S10 constraints/task fit
+
S5 argument construction
or
S11 narrative construction
```

The main uncovered problem is incomplete Knowledge/Ability population for high-school argument and literary/cultural resources, not a missing “Gaokao writing strategy”.

## 4. High-value recurring Ability gaps

I1 identified five high-value Ability areas that deserve explicit canonicalization because they recur, are independently diagnosable, and change tutoring actions.

### A. Representation / discourse transformation — #29

Evidence:

- 2023 Beijing: multi-text information reorganized into comparative table form;
- 2026 National II: prose/source content transformed into Q&A-style science summary.

Why S4 alone is insufficient:

A learner may correctly identify and compress information but fail to preserve semantic relations while changing representation. That produces a different intervention.

Candidate operation:

> Preserve required information/relations while converting them into another representation or discourse structure.

### B. Rule induction + rule application — #30

Evidence:

- 2024 Beijing: induce a rule/pattern and explain complexity;
- 2025 National I: extract cultivation principles and apply them to a new peony case;
- 2025 National II: use theory to evaluate a concrete language case.

Diagnostic split:

```text
Can the learner derive the principle?
vs
Can the learner apply a correctly understood principle to a changed case?
```

These should not be one opaque “迁移能力” label.

### C. Evidence sufficiency / claim validity — #31

This is the strongest uncovered Ability gap.

Evidence:

- competing classical interpretations supported with multiple evidence (2023 Beijing);
- rule complexity/boundary control (2024 Beijing);
- theory-based case evaluation (2025 National II);
- reasonableness of claims (2026 National II).

The existing S5 is procedural support. The learner outcome itself needs tracking:

> Is the evidence relevant, sufficient and appropriately bounded for the claim?

This Ability is highly central because it links reading, open evaluation, classical interpretation and argumentative writing.

### D. Purpose-driven question formulation — #32

Evidence:

- reverse from answer/information to an appropriate question (2025 National I);
- design interview questions from news and explain why (2026 National I).

This is not just “language expression.” It requires identifying an information goal/gap and generating an answerable, scoped question.

### E. Multi-constraint solution synthesis — #33

Evidence:

- multi-material comprehensive plan (2026 Beijing);
- rule-constrained peony plan (2025 National I).

The learner must satisfy several constraints jointly, not merely list them. This is a distinct observable operation from extraction and single-rule application.

## 5. TaskType gap, not Strategy gap

Recent Gaokao authentic communication introduces stable task environments that are underrepresented in the current TaskType inventory:

- interview-question design;
- Q&A/discourse transformation;
- literary micro-commentary / 评点;
- audience/purpose constrained micro-expression.

I1 classifies this as **TaskType inventory expansion** (#34), not new Strategies.

The underlying procedures already compose from S1/S4/S5/S9/S10/S13.

## 6. Knowledge graph remains the largest population gap

The architecture has a correct Knowledge type, but the actual canonical Knowledge inventory is still intentionally sparse.

Gaokao coverage requires systematic population of:

### Language

- normative characters;
- contextual lexical meaning;
- collocation/grammar/syntax;
- register and discourse appropriateness;
- emerging/new-word contextual interpretation.

### Argument and reasoning knowledge

- claim/reason/evidence/warrant;
- condition/cause/analogy/comparison;
- qualification/boundary;
- common argument structures and evidence types.

### Classical Chinese

- lexical networks;
- function words;
- syntax/fixed constructions;
- genre/discourse structures;
- historical/cultural/institutional knowledge when recurrently task-relevant.

### Literature/poetry

- genre/form;
- imagery/allusion;
- narrative concepts;
- literary-history/culture knowledge where it supports authentic reading.

### Writing

- narrative construction knowledge;
- argumentative structure;
- concept definition;
- evidence/argument types;
- task/genre conventions.

This becomes follow-up Issue #35.

## 7. Complexity model survived the audit

D2's eight dimensions were sufficient for all representative demands.

I1 did **not** find a need to add “Gaokao level” or “unfamiliar transfer” as a complexity dimension.

Examples:

```text
2026 literary effect analysis -> bounded C2-like multi-step explanation
2023 multi-text table comparison -> C3 integrated representation task
2026 interview design -> C4 open constrained design/evaluation
2026 Beijing full essay -> C5 extended construction
```

The useful distinction is still:

```text
intrinsic task complexity
!= learner familiarity/transfer
!= source grade/year
```

## 8. Mother Strategy catalog survived the audit

G1's 13-strategy catalog covered all representative demands through single-strategy use or small composition.

I1 found **no compelling new mother Strategy**.

This matters because several recent high-profile tasks look novel at the surface level:

```text
表格比较
牡丹方案
反推问题
采访提问
问答式科普摘要
```

but their reasoning is expressible through existing structures plus missing Abilities/TaskTypes.

Therefore the correct response is graph completion, not template proliferation.

## 9. Material coverage remains incomplete

A terminal Gaokao example proves that a demand exists. It does not by itself provide a good adaptive learning ladder.

For important nodes, ChineseTutor still needs authentic tasks that span:

```text
routine bounded task
-> multi-step integrated task
-> unfamiliar transfer
-> evaluation/open construction when appropriate
```

while controlling variant similarity and prerequisites.

This is explicitly tracked as material-coverage Issue #36.

## 10. Dependency implications

The audit suggests likely dependency chains to test in follow-up graph population.

### Rule transfer

```text
information extraction
-> relation modeling
-> rule induction
-> rule application
-> constraint synthesis / evaluation
```

Not every arrow should automatically become B2 `requires`; each must pass the hard-prerequisite test.

### Claim evaluation

```text
identify claim
+ identify evidence/reasons
-> explain evidence-claim relation
-> evaluate relevance/sufficiency/boundary
```

### Question formulation

```text
understand goal/context
-> identify information gap
-> formulate scoped answerable question
-> justify fit
```

These chains are hypotheses for graph work, not yet canonical edges.

## 11. Coverage-gap issue register

I1 opened explicit follow-up issues:

- #29 representation/discourse transformation Ability;
- #30 rule induction + novel-case application Ability chain;
- #31 evidence sufficiency / claim validity Ability;
- #32 purpose-driven question formulation Ability;
- #33 multi-constraint solution synthesis Ability;
- #34 real-communication/high-school TaskType expansion;
- #35 Gaokao-supporting Knowledge graph completion;
- #36 authentic complexity ladders for high-value nodes.

No recurring unmapped demand in the representative fixture set is left as an unnamed TODO.

## 12. Architectural conclusion

The strongest conclusion of I1 is:

> **The system does not need more top-level architecture to cover Gaokao. It now needs disciplined graph population and authentic evidence supply.**

The current model can express:

```text
what the task is
what knowledge it requires
what operation the learner must perform
what reusable strategy may help
how complex the task is
whether the learner performed independently
how tutoring changed support
how evidence updates personal state
when it should be re-verified
```

That is enough structural machinery for the representative recent Gaokao demands.

The next risk is no longer conceptual under-design. It is **content-model incompleteness**: missing canonical nodes, incomplete TaskTypes, and insufficient authentic task ladders.

## 13. Recommended next implementation phase

Do not start by bulk-copying every exam item into Notion.

Recommended order:

```text
1. Resolve high-centrality Ability gaps (#31, #30, #29/#32/#33)
2. Populate priority Knowledge families (#35)
3. Expand canonical TaskTypes (#34)
4. Build authentic complexity ladders (#36)
5. Then perform production Notion v2 backfill/migration
6. Run live learner sessions and compare predicted vs observed diagnosis/recommendation
```

This preserves the architecture's main principle: **graph semantics first, inventory second, runtime evidence third**.
