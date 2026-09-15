# F3 amendment to C3 — verification refresh on hold

Status: **normative amendment**

F3 depends on axis-specific `*_verified_at` timestamps. C3 therefore needs an explicit rule for fresh evidence that **confirms the current state without changing its level**.

---

## 1. Hold can still be a meaningful Profile update

Example:

```text
before: mastery M2, mastery_verified_at = old date
today: independent representative M2-level review succeeds
after: mastery remains M2, mastery_verified_at = today
```

This is not a fake “upgrade”. It is a `hold + verification refresh` decision.

---

## 2. Axis-specific refresh rule

A `ProfileUpdateDecision` may refresh one axis verification timestamp when new evidence validly supports the **current claimed level/band** on that axis.

### Mastery
Refresh `mastery_verified_at` only when evidence independently supports the current M claim under appropriate diversity/familiarity conditions.

- M1: supported/familiar evidence may confirm M1, but should not be used as a substitute for planned M2 development.
- M2: representative independent performance is required.
- M3: the evidence must retain meaningful unfamiliar/diverse transfer characteristics; a familiar routine item cannot refresh an M3 transfer claim by itself.

### Automation
Refresh `automation_verified_at` only when the relevant trigger/execution fluency is actually observed.

- H2 strategy naming normally cannot refresh independent A2/A3 self-trigger claims;
- A3 requires relevant time/attention-pressure evidence when that is part of the construct.

### Complexity
Refresh `complexity_verified_at` only when the node is validly observed at a demand comparable to the currently verified band/vector.

A C1 success must not refresh a stale C4 claim.

---

## 3. Refresh is independent across axes

One Attempt may refresh:

- mastery only;
- mastery + automation;
- mastery + complexity;
- all three;
- none.

Do not update all timestamps because one answer was correct.

---

## 4. Guided success and refresh

A scaffolded review may teach or diagnose without refreshing the independent claim that was being checked.

Example:

```text
review intended H0
learner needs H4 location help
then reasons correctly
```

Possible result:

- downstream reasoning mastery may still be observed if F2 node-effects preserve its independence;
- location Ability independent verification is not refreshed;
- F1 review/move success criterion may remain unmet.

C3 follows node-specific evidence, not the global maximum H-level alone.

---

## 5. Contradiction

If fresh evidence contradicts the current state, do not refresh the verification timestamp merely because the node was “tested”. Route through ordinary C3 contradiction handling.

A test date is not a verification date.

---

## 6. F3 derived freshness

F3 review status/freshness confidence is recomputed from the accepted C3 verification timestamps. F3 does not directly write those timestamps.

---

## 7. Invariants

1. `hold` may refresh verification without changing M/A/C.
2. Verification means the current claim was supported, not merely attempted.
3. Refresh is per axis and per node.
4. M3 refresh retains transfer requirements.
5. Complexity refresh requires comparable demand.
6. Contradictory evidence does not refresh the clock.
7. C3 remains the authority for Profile verification timestamps.