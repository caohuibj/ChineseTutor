# QA gates — Evidence → Learner Profile updates (C3)

These gates are normative acceptance checks for Issue #17.

## Gate 1 — Update from node evidence, not overall correctness

Fail if:

```text
answer_correctness=0 -> every target node loses mastery
```

Pass when prerequisite-blocked downstream nodes remain `not_observed`.

## Gate 2 — Unknown is not weakness

No observation must preserve `null`, not initialize M0/A0.

## Gate 3 — M/A/C update independently

Pass:

```text
mastery hold M2
automation A1 -> A2
complexity hold C2
```

Fail if one state transition forces the others.

## Gate 4 — Hint meaning is causal, not a global penalty score

H4 supplying location should weaken independent location evidence but may leave downstream reasoning observable.

Fail if all dimensions are mechanically reduced because one H4 hint occurred.

## Gate 5 — H6/H7 success cannot masquerade as independent target mastery

When partial reasoning/near answer supplies the target operation, positive final correctness is learning evidence, not M2 evidence for that operation.

## Gate 6 — H2/H3 may support mastery while limiting automation

A learner may execute an Ability correctly after a strategy reminder. This can strengthen mastery while showing self-trigger is still A1.

Fail if every prompted success is treated identically.

## Gate 7 — Hold + confidence increase is first-class

Repeated valid evidence at the current level need not promote state.

Fail if every additional success must cause a level increase.

## Gate 8 — One contradiction can lower confidence without downgrading

A single meaningful failure normally creates verification need before state downgrade.

## Gate 9 — High-complexity failure does not erase lower-band mastery

Given M2/C2 and one valid C4 failure, default is hold M2/C2 and record ceiling/transfer gap.

## Gate 10 — Comparable repeated contradiction can downgrade

Recent valid failures at the same/lower previously verified complexity, across distinct contexts, may downgrade M/A/C.

Fail if profile is monotonic upward.

## Gate 11 — Staleness alone does not automatically lower mastery

Stale evidence should first lower confidence / set review due. Actual contradictory performance is needed for state downgrade under normal policy.

## Gate 12 — Legacy evidence is conserved but capped

Legacy reconstruction can seed weak estimates.

Fail if legacy-only evidence establishes strong confidence, M3, or A3.

## Gate 13 — M2 requires representative independent performance

Near-identical repeated items do not satisfy M2 merely through count.

## Gate 14 — M3 requires semantic transfer diversity

At least two meaningfully distinct unfamiliar transfer contexts (or equivalent stronger diversity evidence) are required as a guardrail.

Fail if one unseen item or familiar high-complexity repetition creates M3.

## Gate 15 — Intrinsic complexity and transfer remain orthogonal

Fail if C4 is used as a synonym for unfamiliar transfer.

A C2 unfamiliar H0 attempt may be strong M3 evidence when it validly tests transfer.

## Gate 16 — A3 needs actual pressure evidence

Untimed H0 accuracy can strengthen A2 confidence but cannot by itself establish A3.

## Gate 17 — Automation is not inferred from mastery

M3/A1 and M1/A2 remain representable when evidence supports them.

## Gate 18 — State levels are evidence claims, not mandatory sequential badges

If a batch of evidence already satisfies M2 semantics, a previously unknown node may initialize directly to M2.

Fail if the engine requires artificial M0→M1→M2 transitions over separate runs.

The same applies to A and C.

## Gate 19 — Per-axis verification timestamps are only updated by relevant evidence

Untimed evidence may update mastery verification while leaving automation verification unchanged.

## Gate 20 — Variant diversity is discounted

Five attempts in one near-identical variant family cannot be counted as five independent transfer contexts.

## Gate 21 — Assessment confidence matters

Low-confidence automated or reconstructed tagging cannot dominate high-confidence native observations.

## Gate 22 — Error pattern is not created from one mistake

One `E` Attempt may create a candidate note; stable Profile error patterns require repetition/causal consistency.

## Gate 23 — Expression failure does not automatically downgrade reasoning

Pass when:

```text
reasoning=2
written_expression=0
```

updates expression-related Profile state while preserving reasoning evidence.

## Gate 24 — Graph edges never grant state

Reject:

```text
prerequisite M3 -> dependent M1
transfer source M3 -> target M2
```

Edges guide what evidence to seek and whether a downstream failure is validly observable.

## Gate 25 — Every material state change is auditable

A `ProfileUpdateDecision` must contain:

- evidence references;
- before/after axis state;
- policy version;
- human-readable rationale.

## Gate 26 — No opaque score is canonical truth

Internal features are allowed, but the state transition must be reproducible through semantic evidence rules and explainable without revealing a hidden weighted learner score.

## Gate 27 — Recompute does not erase history

A newer policy version may produce a different current projection, but old Attempt evidence and prior decisions remain traceable.

---

# Representative regression tests

### A. Guided repair
H0 mixed -> H3 positive: M1 confidence rises; A1 remains; next test should reduce scaffold.

### B. Routine independence
Distinct H0/H1 representative successes: M2/A2 become eligible.

### C. Near duplicate repetition
Multiple same-variant H0 successes: confidence may rise; M3 remains blocked.

### D. Unfamiliar cross-domain transfer
Established M2 plus distinct unfamiliar H0 successes in modern/classical character reasoning: M3 becomes eligible without requiring C4 Questions.

### E. High-complexity miss
M2/C2 plus one C4 miss: preserve M2/C2.

### F. Recent regression
Repeated recent C1/C2 valid failures across distinct contexts may downgrade stale M2/C2.

### G. Prerequisite block
Word-sense failure with translation `not_observed`: no negative translation mastery update.

---

# C3 definition of done

- [x] evidence unit and validity rules defined;
- [x] hint/independence semantics defined;
- [x] M0-M3 update gates defined;
- [x] A0-A3 update gates defined;
- [x] complexity verification/downgrade rules defined;
- [x] confidence-only update supported;
- [x] contradiction/downgrade policy defined;
- [x] legacy/recency/diversity rules defined;
- [x] error-pattern promotion defined;
- [x] audit decision schema defined;
- [x] representative scenarios added;
- [ ] semantic self-review completed;
- [ ] stacked PR opened.