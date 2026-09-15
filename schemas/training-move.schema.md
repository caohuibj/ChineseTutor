# TrainingMove canonical schema

Status: **F1 normative draft**

A `TrainingMove` is a learner-specific, time-bounded recommendation for **what to train next and under what conditions**. It is not a Question, not a learner-state field, and not a permanent curriculum assignment.

A move sits between Profile diagnosis and concrete Question selection:

```text
Learning Graph + Learner Profile + recent evidence
                    ↓
              TrainingMove
                    ↓
          eligible Question set
                    ↓
             TrainingAttempt
```

---

## 1. Identity

```yaml
training_move_id: TM-...
learner_id: string
generated_at: datetime
policy_version: string
status: proposed | selected | completed | superseded | cancelled
superseded_by: string | null
```

A TrainingMove is ephemeral planning state. Recomputing recommendations may supersede it without altering historical Profile evidence.

---

## 2. Target

```yaml
primary_target_node_ids: string[]
secondary_target_node_ids: string[]
move_type: diagnose | establish | scaffolded_practice | independent_practice | fade_scaffold | automation | complexity_extension | transfer_probe | review
```

### Move-type semantics

- `diagnose` — obtain evidence where state/confidence/prerequisite readiness is unknown or contradictory.
- `establish` — build a Knowledge/Ability/Strategy that is M0 or clearly not established.
- `scaffolded_practice` — learner can progress with meaningful support; aim to build correct execution.
- `independent_practice` — aim to establish representative H0/H1 independent performance toward M2.
- `fade_scaffold` — hold semantic demand roughly steady while reducing hint dependence.
- `automation` — hold conceptual demand roughly steady while improving self-trigger, speed or low-friction execution.
- `complexity_extension` — raise one or more D2 complexity dimensions while keeping the target construct stable.
- `transfer_probe` — test generalization on meaningfully unfamiliar/diverse material without confusing novelty with intrinsic C-band.
- `review` — efficiently re-verify or refresh a previously established node due to forgetting risk / stale evidence.

Do not create a separate move type for every TaskType.

---

## 3. Recommendation reason snapshot

```yaml
reason_codes:
  - mastery_gap
  - automation_gap
  - complexity_gap
  - transfer_gap
  - evidence_uncertainty
  - contradiction_verification
  - forgetting_risk
  - hard_prerequisite_block
  - dependency_unlock
  - gaokao_core
  - school_relevance
  - error_pattern
  - strategy_fade

human_reason: string
profile_snapshot_refs: string[]
blocking_node_ids: string[]
unverified_prerequisite_node_ids: string[]
```

`human_reason` must explain why this move is worth doing **now**, not merely restate the node name.

Example:

> 人物判断本身已能在提示下完成，但连续证据显示仍依赖“为什么该行为有意义”的追问；下一步应保持 C2、撤到 H0/H1，验证能否自主补出证据—解释链。

---

## 4. Priority explanation

F1 deliberately avoids one hidden numeric learner score.

```yaml
priority_class: P0_blocking | P1_high | P2_normal | P3_maintenance | P4_defer
priority_factors:
  learner_gap: none | low | medium | high
  graph_unlock_value: low | medium | high
  gaokao_relevance: enrichment | supporting | core
  evidence_uncertainty: low | medium | high
  forgetting_risk: unknown | low | medium | high
  transfer_need: none | low | medium | high
  school_relevance: none | low | medium | high
  question_availability: none | weak | adequate | strong
priority_rationale: string
```

`priority_class` is a policy output, not a permanent property of the LearningNode.

---

## 5. Training conditions

```yaml
preferred_task_type_ids: string[]
avoid_task_type_ids: string[]
material_familiarity_target: familiar | unfamiliar | mixed | any
transfer_probe_required: bool

complexity_band_target: C0 | C1 | C2 | C3 | C4 | C5 | null
complexity_vector_constraints:
  text_load: {min: int|null, max: int|null}
  information_hiddenness: {min: int|null, max: int|null}
  reasoning_depth: {min: int|null, max: int|null}
  material_heterogeneity: {min: int|null, max: int|null}
  knowledge_retrieval_distance: {min: int|null, max: int|null}
  response_openness: {min: int|null, max: int|null}
  expression_load: {min: int|null, max: int|null}
  time_pressure: {min: int|null, max: int|null}

max_intended_hint_level: H0 | H1 | H2 | H3 | H4 | H5 | H6 | H7
time_limit_target_seconds: int | null
response_mode_preference: oral | typed_chat | written | artifact | any
```

### Important distinction

`max_intended_hint_level` is the **planned ceiling** for the next move. Actual C2 Attempt records what support was really used.

If the learner exceeds the ceiling because teaching requires more help, the Attempt remains valid evidence; the move simply did not meet its success criterion.

---

## 6. Question-selection constraints

```yaml
required_material_properties: string[]
excluded_material_ids: string[]
excluded_question_ids: string[]
excluded_variant_group_ids: string[]
source_preferences: string[]
question_selection_note: string | null
```

Typical uses:

- exclude recently seen variants for a transfer probe;
- prefer current school text only as a tie-breaker;
- prefer official/traceable authentic questions when transfer validation is the goal;
- avoid long-form writing when the move only needs a narrow diagnostic probe.

The TrainingMove specifies the **shape of evidence needed**. A later selector chooses a concrete Question that satisfies the constraints.

---

## 7. Success criterion

```yaml
success_criterion:
  description: string
  required_attempts: int | null
  distinct_contexts_required: int | null
  max_hint_level: H0 | H1 | H2 | H3 | H4 | H5 | H6 | H7 | null
  minimum_node_observation: mixed | positive | null
  minimum_independence: guided | partial | independent | null
  required_familiarity: familiar | unfamiliar | any | null
  required_transfer_probe: bool | null
  required_complexity_band: C0 | C1 | C2 | C3 | C4 | C5 | null
  diagnostic_thresholds: object | null
```

Success criteria are move-local. Completing one TrainingMove does not automatically force a Profile level change; C3 still decides Profile updates from actual Attempts.

Example:

```yaml
description: 在两个不同材料的 C2 人物题中，不给方法提示，独立写出“证据→解释→人物判断”完整链。
required_attempts: 2
distinct_contexts_required: 2
max_hint_level: H1
minimum_node_observation: positive
minimum_independence: independent
```

---

## 8. Recommendation provenance

```yaml
input_profile_version: string | null
input_graph_version: string | null
input_question_bank_version: string | null
recent_attempt_ids: string[]
recommendation_generated_by: rule_policy | tutor | hybrid
manual_adjustment_note: string | null
```

Recommendation must remain explainable and reproducible enough to answer:

> 为什么是这个训练动作，而不是另一个？

---

## 9. Fields forbidden from canonical TrainingMove

TrainingMove must not become a second Profile or Question record.

Do not store as authoritative:

```text
mastery
automation
verified_complexity
full question prompt
reference answer
permanent node priority
final Profile update
```

References/snapshots are allowed for explanation, but source-of-truth remains C1/C2/D1/C3.

---

## 10. F1 invariants

1. Recommend a training action, not merely a Question ID.
2. A move is learner-specific and temporary.
3. Hard prerequisites can block independent downstream training; uncertain prerequisites trigger verification rather than assumed failure.
4. School relevance is a preference/tie-breaker, never the global learning order.
5. Full D2 vector constraints may be manipulated independently.
6. Novelty/transfer is learner-relative and lives in move/attempt conditions, not C-band.
7. Hint ceiling is planned support, not actual evidence.
8. Success criteria are explicit and testable.
9. Completing a move does not bypass C3 Profile-update policy.
10. Recommendation rationale is human-readable and auditable.