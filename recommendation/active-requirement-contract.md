# ActiveRequirement contract for recommendation

Status: **F1 normative input contract**

A recommendation engine cannot decide whether a learner has “met the need” without an explicit requirement to compare against.

C1 stores observed learner state. F1 consumes **ActiveRequirements** that state what level currently matters.

This is not grade progression.

---

## 1. Requirement shape

Suggested contract:

```yaml
requirement_id: REQ-...
node_id: CN-...
requirement_scope: long_term_gaokao | current_training | school_sync | review_maintenance | diagnostic
source: gaokao_coverage | teacher | tutor_policy | school_material | manual

desired_mastery: M0 | M1 | M2 | M3 | null
desired_automation: A0 | A1 | A2 | A3 | null
desired_complexity_band: C0 | C1 | C2 | C3 | C4 | C5 | null
minimum_evidence_strength: none | weak | moderate | strong | null
transfer_required: bool | null

importance: core | supporting | optional
active_from: datetime | null
active_until: datetime | null
reason: string
```

The full D2 vector may later be added when one band is too coarse.

---

## 2. Long-term vs current requirement

A learner may simultaneously have:

```text
long-term Gaokao requirement: M3 / A2 / C4
current training requirement: M2 / A1 / C2
```

Observed Profile may be:

```text
M2 / A1 / C2
```

Then the node can be:

- **stable for the current training requirement**;
- still below the long-term terminal target.

This resolves the ambiguity in the word `stable` without returning to grade-based stages.

---

## 3. Requirement precedence

Requirements do not automatically overwrite one another.

Recommendation interprets them by purpose:

1. hard long-term core coverage defines eventual destination;
2. current-training requirement defines the next reachable evidence target;
3. school-sync requirement raises short-term relevance but does not redefine canonical terminal capability;
4. review-maintenance requirement preserves already established state;
5. diagnostic requirements may temporarily supersede training to resolve uncertainty.

If requirements conflict, F1 must expose the conflict rather than silently selecting the highest number.

---

## 4. Current training targets should be reachable

A current TrainingMove should normally target the **next meaningful evidence frontier**, not the final Gaokao state in one jump.

Example:

```text
Profile: M1 / A1 / C2
Long-term: M3 / A2 / C4
```

Reasonable current requirement:

```text
M2 / A1-A2 / C2
```

Then after representative H0 independence is established, recommendation can shift to transfer/complexity expansion.

This is dependency/evidence-based staging, not grade staging.

---

## 5. Unknown requirement vs unknown learner state

These are different:

- learner state unknown -> evidence uncertainty;
- requirement unknown -> system does not yet know what level matters.

Do not invent a mastery gap when there is no relevant requirement.

For canonical `gaokao_relevance=core`, the system may still flag coverage calibration work, but it should not fabricate exact M/A/C targets without a defined policy.

---

## 6. School sync

School-sync requirements may identify nodes exercised by the currently taught material.

They can:

- elevate tie-break relevance;
- create an efficient opportunity to train a canonical node;
- impose a near-term deadline for a task.

They must not:

- redefine LearningNode identity;
- make grade the progression axis;
- declare unrelated higher-value bottlenecks irrelevant.

---

## 7. Requirement invariants

1. Profile describes observed state; Requirement describes desired state.
2. `stable` is always relative to a Requirement.
3. Long-term Gaokao targets and current reachable targets may coexist.
4. Current targets advance by evidence frontier, not grade.
5. School-sync is a relevance signal, not canonical progression.
6. Unknown learner state and unknown target requirement remain distinct.
7. Recommendation rationale must name the requirement/gap it is acting on when one exists.