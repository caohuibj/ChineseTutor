# C3 transition gates

This document is an operational companion to `profile/evidence-to-profile-policy.md`. It provides minimum semantic gates for state transitions without reducing evidence to one weighted score.

---

## 1. Mastery gates

| transition | minimum semantic gate | evidence that does **not** suffice |
| --- | --- | --- |
| `unknown -> M0` | valid direct negative evidence that target is actually observable; preferably repeated or one high-confidence diagnostic | no evidence; downstream failure blocked by prerequisite; one over-complex item |
| `unknown/M0 -> M1` | learner performs meaningful portion of target with scaffold or in tightly constrained familiar context | H7 answer copying; tutor supplied the target reasoning entirely |
| `M1 -> M2` | multiple valid independent successes on representative, non-trivial, non-near-duplicate contexts | repeated same variant; final correctness after meaningful target prompt |
| `M2 -> M3` | established M2 + stable independent unfamiliar transfer across at least two meaningfully distinct contexts | familiar repetition; one lucky unseen item; higher intrinsic C-band alone |

### M2 representation test

Before promoting to M2, ask:

> If the tutor stopped reminding the learner how to perform the target operation, is there credible evidence that the learner can still execute it correctly on a representative task?

If no, remain M1 even if the polished answer is correct.

### M3 transfer test

Before promoting to M3, ask:

> Has the same canonical operation survived meaningful changes in text/material/task surface without being re-taught or prompted?

If no, remain M2.

---

## 2. Automation gates

| transition | minimum semantic gate | non-sufficient evidence |
| --- | --- | --- |
| `unknown -> A0` | target should have been invoked but repeatedly was not, despite enough underlying competence to make invocation observable | inability caused by missing knowledge/mastery |
| `unknown/A0 -> A1` | explicit cue/reminder reliably activates the construct | tutor executes the operation for learner |
| `A1 -> A2` | self-trigger under normal conditions across distinct contexts; usually H0/H1 | repeated H2/H3 reminders; one self-trigger event |
| `A2 -> A3` | self-trigger + stable quality under meaningful time/attention pressure across repeated contexts | untimed fluency only; fast but error-prone performance |

Automation is about access/trigger/low-friction execution. It must not be inferred solely from mastery.

---

## 3. Complexity verification gates

For each candidate band `Cx`:

1. Question must have reviewed/provisional D2 complexity metadata appropriate to evidence use.
2. The target node must be validly observed at that band.
3. Success must contain enough learner-independent execution to support the claimed axis.
4. One maximum-band success normally yields weak/moderate evidence, not automatic strong verification.
5. Multiple distinct successes can strengthen confidence without changing band.

### Complexity downgrade

Do not downgrade because of:

- one failure above the verified ceiling;
- unfamiliarity;
- source grade;
- one failure where prerequisite breakdown blocked observation.

Downgrade becomes eligible only when valid recent failures occur at the same or lower previously verified band under comparable conditions and meaningfully contradict old evidence.

---

## 4. Hint interpretation matrix

Hint levels are contextual; node-specific independence remains authoritative. Default interpretation:

| hint | default implication |
| --- | --- |
| H0 | strongest evidence for independent execution and automation |
| H1 task-type reminder | may still support mastery; slightly weaker automation evidence |
| H2 strategy/model reminder | can support execution mastery; usually caps self-trigger automation evidence at A1 |
| H3 guiding question | supports M1/repair; target independence must be examined carefully |
| H4 text location supplied | invalidates independent location evidence; downstream reasoning may remain observable |
| H5 critical evidence supplied | invalidates independent evidence selection for supplied evidence; downstream reasoning may still be partly observable |
| H6 partial reasoning supplied | target reasoning usually guided/partial, not independent |
| H7 near answer/model answer | almost never direct mastery evidence for the supplied target operation |

This matrix is a default causal interpretation, not a global score deduction.

---

## 5. Contradiction handling gates

### Level 0 — noise / non-comparable
No state effect. Examples: unrelated modality, over-complex item, prerequisite-blocked failure.

### Level 1 — meaningful contradiction
Hold state; lower confidence or create verification need.

### Level 2 — repeated comparable contradiction
Eligible for state downgrade when recent valid evidence across distinct contexts contradicts the previously verified claim.

### Level 3 — stable regression / forgetting demonstrated
Downgrade state and update verification timestamp to the new lower estimate. Keep historical evidence in the ledger.

No downgrade deletes prior achievements; it updates the current estimate.

---

## 6. Evidence confidence semantics

### `none`
No usable evidence.

### `weak`
One/few observations, legacy reconstruction, heavy scaffold, stale evidence, low assessor confidence, or highly repetitive contexts.

### `moderate`
Several valid observations with reasonable independence and some diversity; no major unresolved contradiction.

### `strong`
Recent, diverse, high-confidence, independent evidence appropriate to the axis; transfer/pressure evidence included where the claimed level requires it.

Important:

```text
M2 + weak confidence
```

is valid and should usually trigger cheap verification rather than immediate remediation.

---

## 7. Error-pattern gates

| state | evidence pattern |
| --- | --- |
| none | no repeated causal error |
| weak/candidate | one or two occurrences that may matter |
| moderate | repeated same causal failure across distinct representative contexts |
| strong | persistent pattern across variation and/or despite targeted intervention |

Clear or replace an error pattern only after varied recent evidence shows the old pattern no longer dominates.

---

## 8. No hidden aggregate score

Implementations may compute internal helper features, but no single weighted scalar may be treated as canonical evidence truth.

Any automated transition must still be able to render a human-readable rationale such as:

> M1→M2 because three recent independent successes occurred across two distinct material contexts and two variant groups at C2, with no comparable recent contradiction; H0/H1 support conditions did not supply the target reasoning.

If the system cannot explain the transition in this form, the update logic is too opaque.