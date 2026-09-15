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

mastery_freshness_confidence: unknown | low | medium | high
automation_freshness_confidence: unknown | low | medium | high
complexity_freshness_confidence: unknown | low | medium | high

retention_family_cache: exact_retrieval | recitation | conceptual_knowledge | reasoning_ability | strategy | writing_production | other | null
retention_policy_version: string | null
forgetting_risk: unknown | low | medium | high
last_review_decision_id: string | null
```

These may be materialized for dashboards/query speed, but they are reproducible from evidence + F3 policy and are not primary learner-state evidence.

`retention_family_cache` is deliberately named as a cache: the policy resolver/classification is the authority. It may be inferred from canonical node semantics or an explicit retention-policy override. Learner state does not redefine the node's retention family.

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

## 3. Evidence strength vs freshness confidence

C1 evidence-strength fields describe the **quality/diversity of the supporting evidence set**:

```text
mastery_evidence_strength
automation_evidence_strength
complexity_evidence_strength
```

C1's earlier shorthand mentioned “old” evidence among reasons a state might be weak. F3 refines that temporal part of the semantics:

- evidence that was structurally weak/legacy-only can still be `weak`;
- but once a native evidence set is established as `moderate`/`strong`, ordinary passage of time should primarily reduce **freshness confidence**, not rewrite the historical evidence set as though it had never been strong.

F3 must therefore not mutate strong historical evidence into “weak evidence” merely because time passed.

Instead, F3 derives axis-specific **freshness confidence**:

```text
mastery_freshness_confidence
automation_freshness_confidence
complexity_freshness_confidence
```

Example:

```text
mastery = M2
mastery_evidence_strength = strong
mastery_freshness_confidence = low
mastery_review_status = overdue
```

Meaning:

> The old M2 evidence was high quality, but it is no longer fresh enough to be confidently treated as current without re-verification.

This avoids rewriting history while still representing temporal uncertainty.

---

## 4. Coarse `review_status`

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

## 5. `forgetting_risk` semantics refined

`forgetting_risk` is still one convenience summary, but F3 makes clear that it is an operational estimate derived from:

- axis review debt;
- freshness confidence;
- evidence strength;
- retention family;
- active requirement;
- recent contradiction;
- meaningful recent use.

It must not be used as direct proof that mastery declined.

---

## 6. Unknown/non-established state does not create ordinary review

If the relevant axis value is `null`, F1 should generally use `diagnose`, not `review`.

If mastery is positively `M0` or the construct has never been established, F1 should generally use `establish` / scaffolded practice rather than treating the node as forgotten maintenance.

F3 review is primarily for previously evidenced claims that need freshness verification.

---

## 7. What F3 does not add

Do not add:

```text
mastery_after_decay
automatic_decay_score
percent_forgotten
permanent_next_review_question_id
```

The system must not silently transform time into learner failure or tie the Profile to one future question.

---

## 8. Amendment invariants

1. M/A/C values do not change merely because review status changes.
2. Review freshness is tracked per axis.
3. Evidence strength and freshness confidence are distinct.
4. The coarse review/forgetting fields remain derived caches.
5. Unknown verification history remains distinguishable from overdue known history.
6. Retention-family cache is policy-derived, not learner-defined canonical meaning.
7. F3 policy version must make temporal fields replayable/recomputable.