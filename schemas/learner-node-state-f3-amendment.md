# LearnerNodeState F3 amendment — temporal review cache

Status: **F3 amendment to C1**

C1 already defines `forgetting_risk` and a coarse `review_status`. F3 refines these into axis-specific derived/cache fields while preserving the canonical M/A/C estimates.

This is a non-breaking semantic extension. Source-of-truth remains TrainingAttempt evidence + C3 projection + F3 policy.

---

## 1. Added derived/cache fields

```yaml
mastery_review_status: unknown | current | due | overdue
automation_review_status: unknown | current | due | overdue
complexity_review_status: unknown | current | due | overdue

mastery_review_due_at: datetime | null
automation_review_due_at: datetime | null
complexity_review_due_at: datetime | null

mastery_review_debt: unknown | none | light | material | urgent
automation_review_debt: unknown | none | light | material | urgent
complexity_review_debt: unknown | none | light | material | urgent

retention_family: exact_retrieval | recitation | conceptual_knowledge | reasoning_ability | strategy | writing_production | other | null
forgetting_risk: unknown | low | medium | high
last_review_decision_id: string | null
```

These may be materialized for dashboards/query speed, but they are reproducible from evidence + F3 policy and are not primary learner-state evidence.

---

## 2. Existing verification timestamps remain authoritative inputs

C1 fields:

```text
mastery_verified_at
automation_verified_at
complexity_verified_at
```

remain the axis-specific temporal anchors.

F3 does not create a new generic `last_verified_at`, because that would again conflate axes.

---

## 3. Coarse `review_status`

For backward compatibility, C1's existing:

```text
review_status: unknown | current | due | overdue
```

may remain as a dashboard summary.

Suggested aggregation precedence:

```text
if all relevant axes unknown                  → unknown
if any relevant axis overdue                  → overdue
else if any relevant axis due                 → due
else                                           → current
```

A more important axis may also be surfaced in `attention_reason`.

The summary never replaces the axis-specific fields.

---

## 4. `forgetting_risk` semantics refined

`forgetting_risk` is still one convenience summary, but F3 makes clear that it is an operational estimate derived from:

- axis review debt;
- evidence strength;
- retention family;
- active requirement;
- recent contradiction;
- meaningful recent use.

It must not be used as direct proof that mastery declined.

---

## 5. What F3 does not add

Do not add:

```text
mastery_after_decay
automatic_decay_score
percent_forgotten
permanent_next_review_question_id
```

The system must not silently transform time into learner failure or tie the Profile to one future question.

---

## 6. Amendment invariants

1. M/A/C values do not change merely because review status changes.
2. Review freshness is tracked per axis.
3. The coarse review/forgetting fields remain derived caches.
4. Unknown verification history remains distinguishable from overdue known history.
5. F3 policy version must make temporal fields replayable/recomputable.