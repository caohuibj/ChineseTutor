# QA gates — I1 Gaokao coverage mapping and graph-gap audit

## Gate 1 — Gaokao is the coverage boundary, not the progression axis
Fail if year/grade controls prerequisites or learner progression.

## Gate 2 — Authentic demand is mapped through canonical entities
Each fixture must identify Material/source, TaskType semantics, Knowledge/Ability demands, Strategies and D2 complexity.

## Gate 3 — Do not invent exact question metadata
If the current source extraction lacks question number or exact stem, preserve that uncertainty explicitly.

## Gate 4 — Surface novelty is not a Strategy
Novel tasks such as interview design or Q&A transformation must first be expressed using existing mother Strategies before any new Strategy is proposed.

## Gate 5 — TaskType is not Ability
An authentic task environment may expose several underlying Abilities; do not create a permanent Ability named only after the exam task.

## Gate 6 — Knowledge is not Ability
Missing genre/lexical/cultural/argument knowledge must not be hidden inside an Ability definition.

## Gate 7 — Missing row is not architecture failure
A candidate canonical node gap is distinct from needing a new top-level schema/entity.

## Gate 8 — Recurring unmapped demand gets an explicit issue
No recurring gap remains only as prose TODO or new answer template.

## Gate 9 — Evidence sufficiency is independently diagnosable
Claims/evidence evaluation cannot be reduced to “find evidence” when authentic demands require judging relevance/sufficiency/boundary.

## Gate 10 — Rule induction and application are not collapsed automatically
The audit must be able to represent “understands rule but cannot apply” and “cannot derive rule in the first place”.

## Gate 11 — Representation transformation preserves semantic relations
Text→table / prose→Q&A is more than shortening text; mapping must expose representation/discourse transformation when recurrent.

## Gate 12 — Question formulation is not generic expression
Reverse-question/interview tasks must identify the information-goal/gap operation.

## Gate 13 — Constraint synthesis is not a list of conditions
A valid plan/solution must jointly satisfy relevant constraints rather than merely repeat them.

## Gate 14 — Transfer remains learner-relative
Authentic cross-context or textbook-to-new-material tasks do not turn “transfer” into a permanent complexity band or Strategy.

## Gate 15 — D2 vector is used instead of source grade
Complexity rationale must rely on hiddenness, reasoning, material heterogeneity, retrieval distance, openness, expression load, etc.

## Gate 16 — Strategy composition remains small
Normal mapping should use one primary and at most a few secondary mother Strategies; a long list signals poor decomposition.

## Gate 17 — Literature mapping requires concrete effect
Technique labels alone are insufficient coverage for literary appreciation demands.

## Gate 18 — Poetry comparison begins with shared dimension
Same-image comparison must not become two unrelated mini-answers.

## Gate 19 — Classical interpretation keeps decoding and evaluation separate
Lexical/syntactic decoding Knowledge/Abilities are distinct from interpretation/evidence evaluation.

## Gate 20 — Writing does not become one monolithic Ability
Prompt parsing, material selection, argument/narrative construction and expression remain separable.

## Gate 21 — Real-communication tasks stay stable across years
TaskType expansion cannot create `2026采访题` or other year-specific identities.

## Gate 22 — Knowledge coverage remains reusable
High-school Knowledge population should avoid trivia that has no stable authentic task role.

## Gate 23 — Authentic material supply is audited separately
A node can have correct semantics while lacking enough authentic questions across complexity; this is a material-coverage gap.

## Gate 24 — Variant diversity matters
Multiple superficial variants cannot be counted as a full complexity/transfer ladder.

## Gate 25 — Representative audit includes all major domains
At minimum: information/modern reading, literary reading, classical Chinese, poetry, language/communication and writing.

## Gate 26 — Beijing and national evidence both appear
The audit cannot generalize from one paper family only.

## Gate 27 — Architecture verdict is explicit
The PR must state whether any current representative demand requires a new top-level entity or schema change.

## Gate 28 — No hidden grade-first backslide
Phrases such as “high-school-only node” cannot be used as semantic justification; source stage is provenance.

## Gate 29 — Gap issues specify observable constructs
A gap issue must say what independent learner operation is missing and how success/failure would be observed.

## Gate 30 — Existing mother Strategy IDs remain semantically stable
I1 may compose strategies but may not silently redefine G1 Strategy identity.

## Gate 31 — Existing seeded Ability IDs remain stable
Coverage fixtures may reference known canonical IDs; candidate gaps use temporary gap labels, not invented permanent node IDs.

## Gate 32 — Source claims stay within project evidence
Do not silently add exact paper content beyond imported papers/synthesis/official analyses unless separately verified.

## Gate 33 — Coverage audit does not become a question bank
Fixtures encode terminal demands and mappings; D1 authentic Question ingestion remains a separate content operation.

## Gate 34 — Follow-up order follows graph value
High-centrality shared abilities/knowledge should precede bulk material migration.

## Gate 35 — I1 conclusion is falsifiable
Future authentic questions that cannot be represented should reopen architecture/gap analysis rather than be forced into existing categories.

---

# Definition of done

- [x] audit protocol defined;
- [x] representative Beijing + national mapping fixtures added;
- [x] modern/information, literary, classical, poetry, language/communication and writing covered;
- [x] D2 complexity included in mappings;
- [x] current mother Strategies stress-tested;
- [x] recurring Ability/TaskType/Knowledge/material gaps registered;
- [x] each recurring unmapped gap linked to an explicit issue;
- [x] architecture-level verdict recorded;
- [x] semantic self-review complete;
- [ ] stacked PR opened.
