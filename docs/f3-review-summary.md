# F3 semantic self-review summary

Status: **self-review complete**

Scope: retention families, temporal freshness, review scheduling, C3 timestamp refresh, F1 integration, and review evidence boundaries.

The purpose of this review was to prevent F3 from quietly reintroducing a numeric decay score, creating a second recommendation engine, or turning stale evidence into fabricated learner failure.

---

## 1. Risk: time could become hidden mastery decay

### Problem
A naive spaced-repetition model might reduce M/A/C directly when time passes.

### Correction
F3 now treats time as changing:

- review status;
- freshness confidence;
- forgetting-risk estimate;
- value of a verification move.

Only new C2 evidence interpreted by C3 may alter M/A/C.

---

## 2. Risk: historical evidence strength and temporal freshness were conflated

### Problem
C1's early shorthand allowed “old” to contribute to weak evidence. If applied literally, a strong native evidence set could be rewritten as historically weak merely because it aged.

### Correction
F3 splits:

```text
evidence strength = quality/diversity of the historical evidence set
freshness confidence = how confidently the old claim can be treated as current now
```

A valid state is:

```text
M2 / evidence strong / freshness low / review overdue
```

This preserves history while representing uncertainty.

---

## 3. Risk: ReviewDecision identity contradicted its multi-axis schema

### Problem
The first draft described one ReviewDecision as belonging to one axis, but also allowed multiple axes.

### Correction
A ReviewDecision now belongs to one learner-node review need and may combine several compatible axes when one probe can validly verify them. Each axis still has independent clocks and refresh rules.

---

## 4. Risk: F3 lacked an explicit authority for refreshing `*_verified_at`

### Problem
A successful review that leaves M2 unchanged still needs to make the verification current. Without a rule, implementations might either fail to refresh the clock or let F3 mutate Profile directly.

### Correction
Added `profile/f3-verification-refresh-policy.md`:

```text
fresh confirming evidence
→ C3 hold + verification refresh
```

C3 remains the only authority that writes accepted verification timestamps. F3 never directly resets them.

---

## 5. Risk: M3 retention could collapse back into routine M2 checking

### Problem
A familiar routine item can confirm ordinary independent performance but cannot confirm continued unfamiliar transfer.

### Correction
M3 refresh explicitly requires meaningful unfamiliar/diverse transfer evidence. C-band and novelty remain separate.

---

## 6. Risk: unknown/M0 nodes could pollute the review queue

### Problem
A full Gaokao graph contains many untested nodes. If null states become “overdue”, maintenance would explode.

### Correction

- `null` + no verification history → review status `unknown`; F1 uses `diagnose` only when needed;
- `M0` / not established → `establish` or scaffolded practice, not maintenance review.

F3 primarily maintains previously evidenced claims.

---

## 7. Risk: retention family could become learner-specific canonical meaning

### Problem
Caching `retention_family` on LearnerNodeState could imply that the learner defines the node's semantics.

### Correction
Renamed it `retention_family_cache` and added an explicit family-resolution policy. Authority is policy classification derived from canonical semantics + optional approved override; learner-specific timing modifies parameters, not node identity.

---

## 8. Risk: post-repair follow-up was incorrectly forced into move type `review`

### Problem
After H6/H7 teaching, the learner may not yet have an established independent claim to “review”.

### Correction
F3 may create a temporal **post-repair recheck need**, but F1 chooses `independent_practice`, `diagnose`, or `review` according to current Profile state.

The Zhou Yafu fixture now uses a delayed new C2/H0 item rather than repeating the same repaired question.

---

## 9. Risk: default day values could be mistaken for scientific truth

### Problem
Operational scheduling needs numbers, but a fixed interval table can easily be overinterpreted.

### Correction
All horizons/multipliers/overdue thresholds are explicitly:

- configurable;
- policy-versioned;
- engineering defaults;
- subject to later learner-specific calibration.

They are not claims of universal optimal spacing.

---

## 10. Risk: ordinary authentic work might be ignored as review

### Problem
If every stale node creates a standalone worksheet, maintenance would become expensive and artificial.

### Correction
Normal TrainingAttempts may satisfy review when they genuinely re-observe the node/axis with suitable independence and demand. Passive exposure/co-occurrence does not count.

---

## 11. Risk: review could become a second global scheduler

### Problem
A separate maintenance queue could fight F1 and crowd out more valuable bottleneck work.

### Correction
F3 emits temporal need and verification shape; F1 remains the single global prioritizer. Time alone never creates P0.

---

## 12. Risk: review flood at Gaokao graph scale

### Problem
Hundreds of nodes with due dates could consume all learning time.

### Correction
Added:

- batching of compatible retrieval nodes;
- embedded review inside already-selected tasks;
- consequence-based ordering;
- configurable maintenance budget (~25% normal session as an engineering default);
- suppression after the evidence purpose is satisfied.

Critical regression/deadline cases can exceed the routine budget.

---

## 13. Risk: review pass/fail could bypass C2/C3

### Problem
A specialized review engine could silently promote/downgrade Profile state.

### Correction
Review execution remains:

```text
ReviewDecision
→ F1 TrainingMove
→ F2/C2 Attempt
→ C3 Profile update
```

`passed/failed` on ReviewDecision is audit linkage, not a hidden Profile mutation.

---

# Review conclusion

F3 is semantically coherent with B1/B2/C1/C2/C3/D2/F1/F2 after the corrections above.

The core invariant is now:

> **时间只改变“我们需不需要重新验证”的紧迫度；只有学生新的真实表现，才能改变“他现在到底会到什么程度”的证据判断。**

Remaining implementation/calibration work belongs later:

- empirical per-learner horizon tuning;
- actual scheduler/runtime code;
- Notion migration/backfill;
- notification/UI behavior;
- population-level statistical calibration.