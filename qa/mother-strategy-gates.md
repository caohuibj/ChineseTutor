# QA gates — G1 mother Strategy consolidation

These gates protect the project from strategy/template proliferation.

## Gate 1 — Strategy is not TaskType
Fail if a node exists only because an exam label exists.

## Gate 2 — Strategy coordinates multiple operations
Atomic acts such as “找关键词” remain Ability, not Strategy.

## Gate 3 — Strategy is promptable and fadeable
A valid Strategy can be named/reminded at H2, then later self-invoked without the reminder.

## Gate 4 — Strategy has observable execution
A final correct answer alone is insufficient; the procedure itself must be observable when Strategy evidence is claimed.

## Gate 5 — Output skeleton is not Strategy
Sentence stems belong to TaskType/output normalization, not canonical Strategy identity.

## Gate 6 — No one-TaskType-one-Strategy proliferation
High-frequency TaskTypes should reuse the 10–15 mother catalog wherever semantics permit.

## Gate 7 — Same Strategy crosses TaskTypes when procedure is the same
Evidence→explanation→conclusion should not be cloned separately for modern text, classical text and poetry.

## Gate 8 — Representation-specific procedure may remain domain-specific
Classical translation and poetry-emotion procedures can remain distinct when their operations/evidence are genuinely specialized.

## Gate 9 — Comparison starts from a shared dimension
A comparison Strategy that merely lists A then B fails.

## Gate 10 — Information compression preserves necessary conditions
S4 cannot reward shorter answers that delete key qualifiers.

## Gate 11 — Structure/function avoids empty terminology
S3 must require “承什么/启什么/作用于什么”, not only labels such as 承上启下.

## Gate 12 — Argument Strategy includes warrant
S5 must distinguish evidence from explanation of why evidence supports the claim.

## Gate 13 — Poetry Strategy avoids fixed-symbol lookup
S6 cannot turn “月=思乡” into a universal answer shortcut.

## Gate 14 — Translation Strategy does not replace Knowledge
S7 may coordinate translation, but missing lexical/syntactic knowledge remains separately diagnosable.

## Gate 15 — Typical-event Strategy is not universal
S8 applies when special context/key choice is meaningful; ordinary event summary need not use it.

## Gate 16 — Appreciation Strategy requires concrete effect
S9 cannot stop at “运用了比喻，生动形象”.

## Gate 17 — Constraint parsing does not become writing itself
S10 handles task restrictions/fit; S5/S11 handle argument/narrative construction.

## Gate 18 — Narrative and argumentative writing remain distinct
Do not merge S11 into S5 simply because both produce essays.

## Gate 19 — Minimal revision preserves higher-level diagnosis
S12 cannot cosmetically edit a paragraph whose central logic/structure is wrong.

## Gate 20 — Relation-chain Strategy differs from evidence-warrant
S13 models causal/conditional relations among facts; S1 justifies a conclusion with evidence. They may compose but should not collapse automatically.

## Gate 21 — Default TaskType mapping stays small
Normal TaskType: ~1 primary and at most 0–2 secondary mother Strategies.

## Gate 22 — Recommended Strategy is not hard prerequisite
TaskType→Strategy mapping must not be converted into B2 `requires`.

## Gate 23 — Correct answer via another valid method does not fabricate Strategy evidence
Ability can be positive while recommended Strategy remains `not_observed`.

## Gate 24 — Strategy Mastery and Automation are independent
A learner may execute a named Strategy correctly (M2) but still need H2 to trigger it (A1).

## Gate 25 — F2 hint semantics remain causal
H2 strategy reminder weakens self-trigger/automation evidence but need not erase independently produced downstream reasoning.

## Gate 26 — Current Zhou Yafu case maps cleanly
人物/典型事件 work should be expressible through S8 + S1 without inventing a 周亚夫-specific strategy.

## Gate 27 — Title/structure does not create a title-only strategy
标题作用 maps to S3 (+S1 when justification is needed).

## Gate 28 — Poetry 炼字 composes existing strategies
Use S9 + S6 rather than a new “炼字五步法” unless future evidence proves a distinct reusable procedure is necessary.

## Gate 29 — Classical 原因概括 composes existing strategies
Use S13 + S4 instead of a dedicated new strategy.

## Gate 30 — Strategy count remains disciplined
Any proposed strategy beyond the catalog must document why existing composition cannot express it without diagnostic loss.

## Gate 31 — Knowledge-centric tasks may have no default mother Strategy
Do not force 文言实词、虚词/句式、断句等 tasks into a Strategy merely to fill the mapping table.

## Gate 32 — Strategy seed records conform to B1 LearningNode schema
Required fields such as `parent_node_id`, `aliases_zh`, and `source_basis` must be present.

## Gate 33 — Existing Strategy IDs remain semantically consistent
If an ID already appears in B1 examples, G1 may refine display wording but cannot silently change its semantic identity.

## Gate 34 — TaskType IDs are not fabricated
Until canonical TaskType-row backfill exists, G1 may map stable TaskType semantics/names but must not invent fake permanent IDs.

## Gate 35 — Domain is navigation, not Strategy-use boundary
A Strategy may transfer to another domain if its procedure remains valid; do not clone only because the source domain changed.

## Gate 36 — Strategy transfer is flexible, not rigid
Forcing irrelevant steps in a new task is not M3 transfer even if the learner reproduces the memorized template.

## Gate 37 — Ability/Strategy near-neighbors stay separate
Examples such as prompt parsing, sentence revision, poetic emotion and character judgment must preserve outcome-vs-procedure distinction.

---

# Definition of done

- [x] 10–15 mother Strategy catalog defined;
- [x] high-frequency modern/classical/poetry/writing/language TaskTypes mapped;
- [x] Strategy vs TaskType/Ability/Knowledge/output-skeleton boundaries defined;
- [x] Strategy composition rules defined;
- [x] canonical Strategy seed fixture added;
- [x] TaskType mapping schema refined;
- [x] automation/hint evidence boundary documented;
- [x] F1 Strategy-target integration documented;
- [x] semantic self-review complete;
- [ ] stacked PR opened.