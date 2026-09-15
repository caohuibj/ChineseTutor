# C3 semantic self-review summary

Issue: #17 — Define evidence-to-profile update policy

## Review scope

Reviewed C3 against B2 graph semantics, C1 Learner Profile, C2 TrainingAttempt, and D2 complexity separation.

The central question was:

> Can every Profile change be explained as a defensible claim about current learner state, without conflating correctness, hinting, novelty, grade, or graph topology?

## Findings and corrections

### 1. State levels were at risk of becoming sequential badges — fixed

An early reading of transition sections could imply that a node must chronologically pass M0→M1→M2→M3.

That would create artificial practice just to unlock state labels.

C3 now explicitly states:

- levels are evidence claims, not badges;
- unknown may initialize directly to M2/M3, A2/A3, or a higher verified complexity band if the existing evidence set already satisfies the semantics;
- transition tables describe evidence requirements, not gamified unlock order.

### 2. Incremental mutation could create irreversible profile drift — fixed

C1 says Profile is a projection, but C3 initially focused on local transitions.

The policy now requires replayability:

```text
current Profile = projection(evidence ledger, policy version, explicit overrides)
```

Incremental updates are permitted for performance only when they are replay-equivalent. Historical Attempts and old decisions are never rewritten to fit a new policy.

### 3. Short-question evidence topology would overburden writing — fixed

Rules such as “more than one independent success” can accidentally mean “write several full essays”.

C3 now permits independently assessable artifact versions, sections, and revision episodes to supply distinct evidence when they genuinely expose the same canonical node under varied conditions.

The system remains diversity-based, not item-count based.

### 4. Hint level could become a blanket score penalty — fixed

C2 H0-H7 is global Attempt context, but one hint may target only one node.

C3 now makes `node_evidence.independence` primary and uses hint/intervention type as a causal cross-check.

Example:

```text
H4 gives text location
-> location independence invalidated
-> downstream reasoning may still be independently observable
```

If hint relevance is ambiguous, evidence confidence is reduced rather than assuming independence.

### 5. A0 could be confused with low mastery — fixed

Failure to invoke a strategy/operation is only automation evidence when enough underlying competence exists for invocation to be observable.

If the learner cannot execute the operation at all, that is primarily mastery/knowledge evidence, not proof of A0.

### 6. M3 and C4 coupling remained a regression risk — guarded

D2 removed `C4 = unfamiliar transfer`.

C3 fixtures and QA now explicitly include:

- unfamiliar C2 H0 transfer supporting M3;
- familiar C4 not proving M3;
- one unfamiliar C4 failure not downgrading established M2/C2.

### 7. Profile updates needed their own audit object — added

C3 adds `ProfileUpdateDecision` containing:

- source evidence refs;
- policy version;
- before/after M/A/C state;
- per-axis action;
- confidence effect;
- excluded/discounted evidence reasons;
- human-readable rationale.

This keeps the current Profile lightweight while making state evolution auditable.

## Scenario review

Fixtures now cover:

1. existing Zhou Yafu legacy character reasoning;
2. H0→H3 guided repair;
3. M1→M2 independent routine stabilization;
4. near-duplicate repetition that must not create M3;
5. unfamiliar cross-domain transfer to M3;
6. high-complexity failure without lower-band downgrade;
7. repeated recent comparable regression and downgrade;
8. prerequisite-blocked downstream `not_observed`;
9. expression failure without reasoning downgrade;
10. untimed success that cannot establish A3.

## Acceptance judgment

**PASS for C3 semantic architecture.**

C3 is ready to stack on D2.

## Deliberately deferred

- numerical/empirical calibration from a large learner population;
- exact forgetting/spacing decay function;
- TrainingMove priority scoring;
- production Notion migration/backfill;
- automated assessor calibration across models/teachers.

These require real usage data and belong after the evidence/profile loop is stable.