# ReviewDecision canonical schema

Status: **F3 normative draft**

A `ReviewDecision` records why one learner-node axis is considered current/due/overdue, what evidence clock was used, and what minimum verification task should be scheduled.

It is a temporal planning/audit object. It does **not** directly change Mastery, Automation or verified Complexity.

```text
LearnerNodeState + Attempt history + retention policy + ActiveRequirement
                              ↓
                        ReviewDecision
                              ↓
                     F1 TrainingMove(review)
                              ↓
                       TrainingAttempt
                              ↓
                     C3 Profile update
```

---

## 1. Identity

```yaml
review_decision_id: RD-...
learner_id: string
node_id: string
generated_at: datetime
policy_version: string
status: proposed | scheduled | satisfied | superseded | cancelled
superseded_by: string | null
```

A ReviewDecision is reproducible from Profile/evidence + policy version. It may be superseded when new evidence arrives.

---

## 2. Review axes

Review debt is axis-specific.

```yaml
axes:
  - mastery
  - automation
  - complexity
```

For each axis:

```yaml
axis_state:
  axis: mastery | automation | complexity
  current_value: string | null
  evidence_strength: none | weak | moderate | strong
  last_verified_at: datetime | null
  retention_family: exact_retrieval | recitation | conceptual_knowledge | reasoning_ability | strategy | writing_production | other
  horizon_days: int | null
  due_at: datetime | null
  overdue_at: datetime | null
  review_status: unknown | current | due | overdue
  review_debt: unknown | none | light | material | urgent
  forgetting_risk: unknown | low | medium | high
  basis_attempt_ids: string[]
  basis_note: string
```

`review_status` describes temporal freshness. `forgetting_risk` is an operational risk estimate. Neither is evidence of actual failure.

---

## 3. Why review now

```yaml
reason_codes:
  - age_of_evidence
  - weak_evidence
  - automation_stale
  - complexity_stale
  - recent_contradiction
  - active_requirement
  - prerequisite_criticality
  - post_repair_recheck
  - memorization_maintenance
  - transfer_retention_check
  - manual_teacher_request

human_reason: string
active_requirement_ids: string[]
downstream_active_node_ids: string[]
```

The reason must distinguish:

- **stale evidence**: we no longer know confidently whether the old state is current;
- **demonstrated regression**: recent evidence shows current failure.

Only the second is learner-failure evidence.

---

## 4. Cheapest valid verification

```yaml
verification_plan:
  evidence_purpose: refresh_mastery | refresh_automation | refresh_complexity | discriminate_regression | retain_exact_recall | retain_strategy_trigger
  preferred_task_type_ids: string[]
  target_complexity_band: C0 | C1 | C2 | C3 | C4 | C5 | null
  minimum_complexity_band: C0 | C1 | C2 | C3 | C4 | C5 | null
  max_intended_hint_level: H0 | H1 | H2 | H3 | H4 | H5 | H6 | H7
  target_familiarity: familiar | partially_familiar | unfamiliar | any
  transfer_probe_required: bool
  time_limit_seconds: int | null
  response_mode: oral | typed_chat | written | artifact | any
  max_probe_cost: micro | short | medium | full_task
  excluded_variant_group_ids: string[]
  probe_note: string
```

The review probe should be the **smallest task that can validly re-observe the due axis**.

Examples:

- lexical retrieval → one or several short recall/context items;
- character-reasoning mastery → one representative evidence→warrant item, not a full reading paper;
- Strategy automation → an item where the strategy must self-trigger without H2;
- writing scene construction → one focused paragraph or revision artifact rather than a full essay.

---

## 5. Implicit-review credit

Review need may be satisfied by a non-review TrainingAttempt only when that attempt genuinely re-observes the construct.

```yaml
implicit_review_credit:
  eligible: bool
  credited_attempt_ids: string[]
  credited_axes: [mastery | automation | complexity]
  rationale: string | null
```

Passive exposure, rereading, teacher explanation, or a downstream task where the node was `not_observed` does not reset the review clock.

---

## 6. Priority handoff to F1

```yaml
recommended_priority_floor: P0_blocking | P1_high | P2_normal | P3_maintenance | P4_defer | null
priority_escalation_reason: string | null
```

Normal stale-but-stable review is usually `P3_maintenance`.

Escalation is allowed when:

- an active requirement is near;
- stale/weak evidence concerns a high-value prerequisite needed now;
- recent contradiction suggests actual regression;
- automation staleness materially threatens timed performance.

Time alone should not create `P0_blocking`.

---

## 7. Outcome linkage

Review execution remains in the normal loop.

```yaml
training_move_id: string | null
attempt_ids: string[]
resolved_at: datetime | null
resolution: null | passed | failed | uncertain | deferred
```

Outcome semantics:

- `passed` → C2 Attempt supplies fresh evidence; C3 refreshes confidence/timestamps if warranted;
- `failed` → new negative/mixed Attempt evidence enters C3 contradiction handling;
- `uncertain` → F1/F2 may schedule a discriminator/diagnostic;
- `deferred` → review need remains visible.

`ReviewDecision` never performs a hidden downgrade.

---

## 8. F3 invariants

1. Time does not directly alter M/A/C values.
2. Review status is axis-specific.
3. Unknown verification date is not equivalent to overdue failure.
4. Review clock can be refreshed only by valid observation of the relevant axis.
5. Passive exposure never counts as verification.
6. Review begins with the cheapest valid probe.
7. Failed review becomes ordinary Attempt evidence and is handled by C3.
8. Review priority is explainable and integrates with F1 rather than bypassing it.
9. Node family affects review cadence, but cadence is policy/configuration—not grade progression.
10. Review history remains auditable and replayable.