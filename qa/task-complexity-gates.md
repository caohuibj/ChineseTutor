# QA gates — Task Complexity (D2)

## Gate 1 — Grade is not complexity

Two tasks from different grades may receive the same vector/band when their actual demands are comparable.

Fail if source grade/year directly sets C-band.

## Gate 2 — Learner novelty is separate

The same Question keeps the same canonical complexity whether the learner has seen the material before or not.

Fail if `unfamiliar => C4`.

## Gate 3 — All eight dimensions are inspectable

Reviewed Questions must retain:

```text
text_load
information_hiddenness
reasoning_depth
material_heterogeneity
knowledge_retrieval_distance
response_openness
expression_load
time_pressure
```

Fail if only a single C-band survives.

## Gate 4 — Expression and reasoning are independent

Pass:

- short answer requiring hidden-premise evaluation: reasoning high, expression low;
- full narrative writing: expression high even when local reasoning is moderate.

Fail if answer length is used as a proxy for reasoning depth.

## Gate 5 — Text length does not dominate band

A long direct-extraction task may remain C1/C2.

Fail if `text_load=3` automatically creates C4/C5.

## Gate 6 — Time pressure does not dominate semantic band

A C1 language-use item under severe timing remains semantically C1, while the Attempt becomes stronger automation evidence.

Fail if time pressure alone increases the C-band.

## Gate 7 — Material heterogeneity is integration, not count

Two adjacent homogeneous lines do not automatically mean multi-material integration.

Pass when a bundle of text/chart or multiple texts requires cross-source reconciliation.

## Gate 8 — Knowledge distance is canonical task demand

It describes required external knowledge not locally supplied, not distance from one learner's memory.

Learner mastery/familiarity belongs to Profile/Attempt.

## Gate 9 — Transfer is tested orthogonally

Pass scenario:

```text
same C2 Question family
familiar + H3 -> learning/scaffold evidence
unfamiliar + H0 -> transfer probe
```

Canonical C2 stays unchanged.

## Gate 10 — Band assignment uses semantic precedence

Review vector first, then apply `complexity/band-assignment.md` from C5 downward.

Fail if a naive average such as `sum(vector)/8` is the only band rule.

## Gate 11 — C5 requires open sustained construction

Dense or difficult does not automatically mean C5.

Pass examples:

- constrained multi-text evidence evaluation -> C4;
- full narrative/material writing -> C5.

## Gate 12 — Legacy labels are provisional only

`基础/常规/提升/迁移` are preserved as migration metadata but do not map one-to-one to bands.

Especially reject `迁移 -> C4`.

## Gate 13 — Cross-domain calibration exists

Representative fixtures must cover at least:

- modern reading;
- classical Chinese;
- poetry;
- language use;
- writing;
- high-integration information task.

## Gate 14 — Complexity can guide controlled training moves

The model must allow recommendations such as:

```text
hold reasoning_depth=2
raise expression_load 1->2
```

or:

```text
hold semantic vector roughly constant
reduce hints H3->H0
switch familiarity familiar->unfamiliar
```

Fail if the only training operation is “increase C-band”.

## Gate 15 — Reviewed complexity is auditable

Reviewed records include rationale, reviewer/provenance, and stable Question/Material version context.

## D2 definition of done

- [x] eight dimensions defined with anchors;
- [x] C0-C5 semantics independent of grade;
- [x] unfamiliar transfer removed from C-band semantics;
- [x] deterministic review precedence added;
- [x] legacy difficulty migration defined;
- [x] representative cross-domain fixtures scored;
- [x] controlled recommendation examples defined;
- [ ] C1 legacy shorthand corrected/superseded;
- [ ] semantic self-review summary complete;
- [ ] stacked PR opened.
