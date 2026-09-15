# QA gates — F3 retention, review and spaced verification

These are normative acceptance checks for Issue #23.

## Gate 1 — Time does not downgrade mastery
Fail if `M2 -> M1` occurs only because days elapsed.

## Gate 2 — Staleness and regression are separate
A node may be M2 with overdue review. Demonstrated regression requires new learner evidence.

## Gate 3 — Review is axis-specific
A node can be current on mastery and due on automation without forcing both axes to the same status.

## Gate 4 — Unknown is not overdue
Missing verification history must remain `unknown`, not fabricated lateness.

## Gate 5 — Retention family is semantic, not grade-based
Exact retrieval, reasoning, strategy and writing may use different horizons regardless of school year.

## Gate 6 — Default horizons are configurable
Fail if day values are presented as universal scientific truths or hard-coded curriculum stages.

## Gate 7 — Evidence strength changes review horizon
Weak evidence should normally be rechecked sooner than strong evidence.

## Gate 8 — Passive exposure does not refresh clocks
Rereading, teacher explanation or model-answer viewing alone cannot count as independent verification.

## Gate 9 — Ordinary training may count as implicit review
Pass when a normal TrainingAttempt genuinely re-observes the due construct under valid evidence conditions.

## Gate 10 — Incidental presence is insufficient
A word/strategy merely appearing in a task does not refresh its clock unless the learner operation was actually observed.

## Gate 11 — Cheapest valid verification first
Stale M2 knowledge receives a short probe before full reteaching.

## Gate 12 — Review failure enters C2/C3
Fail if review code secretly downgrades the Profile outside the normal evidence pipeline.

## Gate 13 — One failure need not erase stable history
A single independent miss may lower confidence/request verification before a downgrade, consistent with C3.

## Gate 14 — Repeated recent contradiction can matter
F3 must allow multiple comparable failures to feed C3 regression decisions.

## Gate 15 — Automation review observes self-trigger
A2/A3 review cannot be refreshed by a task where H2 explicitly reminds the strategy being tested.

## Gate 16 — Complexity review matches demand
A C1 success cannot refresh a stale C4 verification claim.

## Gate 17 — Transfer is still not C4
Retention of M3/generalization should use unfamiliar/diverse evidence; complexity band and novelty remain separate.

## Gate 18 — Recitation uses exact production evidence
Rereading or recognition alone does not fully verify exact 默写/背诵 production.

## Gate 19 — Writing review respects artifact cost
A narrow writing skill should normally be rechecked through micro-writing/revision rather than a full essay.

## Gate 20 — Post-repair review is earlier
Heavy scaffold/model exposure creates a delayed independent re-probe obligation before the normal long retention horizon.

## Gate 21 — Same-item independence cannot be restored after H7
Post-repair independent review requires a sufficiently distinct new probe for supplied content.

## Gate 22 — Review priority stays in F1
F3 produces temporal needs; F1 remains the global prioritizer across maintenance, bottleneck repair and development.

## Gate 23 — Time alone does not create P0
A stale node becomes P0/P1 only through active requirement/causal consequence/regression evidence, not elapsed days alone.

## Gate 24 — Active requirement can pull review earlier
A near-term assessment may justify fresh validation without turning grade/unit into the global progression axis.

## Gate 25 — Review success stops repetition
After sufficient fresh evidence, duplicate same-purpose review should be suppressed until the next horizon.

## Gate 26 — Inventory abundance does not create review need
Having many flashcards/questions available cannot make a current node due.

## Gate 27 — Review flood is controlled
The policy supports batching, embedded review, consequence-based ordering and a configurable maintenance budget.

## Gate 28 — Exact retrieval can batch without losing per-node evidence
One short block may review several facts, but each node must retain its own observation/result.

## Gate 29 — Reasoning review uses representative task evidence
Do not replace reasoning retention with terminology recall.

## Gate 30 — Strategy mastery and automation can have different clocks
The learner may still execute a strategy correctly when prompted while self-trigger has become stale.

## Gate 31 — Verification timestamps remain per axis
F3 must not collapse mastery/automation/complexity into one generic `last_verified_at`.

## Gate 32 — ReviewDecision is auditable
It records family, source verification, horizon, due status, risk, reason, and the cheapest valid verification shape.

## Gate 33 — Review history is replayable
Given the evidence ledger and policy version, the derived temporal state should be reconstructable.

## Gate 34 — Current Zhou Yafu-like reasoning can be implicitly reviewed
A later independent unfamiliar character-reasoning item may refresh the shared reasoning node without a dedicated “review worksheet”.

## Gate 35 — Lexical regression can block translation after evidence
If fresh independent failures show word-sense regression and B2 marks it required for translation, F1 may escalate the prerequisite work.

## Gate 36 — Stable does not equal temporally current
`readiness_status=stable` may coexist with `review_status=due`; the UI/logic must preserve both meanings.

---

# Definition of done

- [x] ReviewDecision schema defined;
- [x] retention families defined;
- [x] configurable default horizons defined;
- [x] axis-specific review clocks/debt defined;
- [x] implicit-review credit rules defined;
- [x] cheapest-valid-verification policy defined;
- [x] failed review routes through C2/C3;
- [x] F1 integration defined;
- [x] post-repair verification defined;
- [x] review-flood controls defined;
- [x] cross-domain regression scenarios added;
- [ ] semantic self-review complete;
- [ ] stacked PR opened.