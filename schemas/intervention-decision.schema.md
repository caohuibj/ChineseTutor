# InterventionDecision canonical schema

Status: **F2 normative draft**

An `InterventionDecision` records one tutor decision made **after a learner response and before the next learner response** inside a concrete tutoring series.

It answers:

> Given what the learner just demonstrated, what is the smallest next tutor action that is likely to expose or repair the earliest causal gap without unnecessarily supplying the answer?

It is not a TrainingAttempt, not a TrainingMove, and not a LearnerNodeState update.

---

## 1. Identity and linkage

```yaml
intervention_decision_id: INT-...
learner_id: string
session_id: string | null
attempt_series_id: string
after_attempt_id: string
training_move_id: string | null
question_id: string | null
decided_at: datetime
policy_version: string
```

`after_attempt_id` is the response being interpreted. The next learner response, if any, becomes a new C2 `TrainingAttempt` whose hint/intervention snapshot reflects this decision.

---

## 2. Diagnostic snapshot

```yaml
primary_causal_layer: none | Q | K | R | I | M | E | C | uncertain
secondary_layers: [Q | K | R | I | M | E | C]
diagnosis_confidence: low | medium | high
causal_rationale: string

preserved_strengths: string[]
missing_link: string | null
blocked_node_ids: string[]
observed_target_node_ids: string[]
not_observed_node_ids: string[]
```

The decision must preserve what the learner already did correctly. It should not restart the entire solution when only one link is missing.

`primary_causal_layer` means the **earliest actionable cause of the current failure or incompleteness**, not necessarily the most visible final error.

---

## 3. Tutor action

```yaml
action_family:
  - acknowledge_and_retry
  - task_reframe
  - prerequisite_check
  - knowledge_recall_cue
  - strategy_activation
  - guiding_question
  - location_narrowing
  - evidence_focus
  - reasoning_bridge
  - expression_conversion
  - execution_check
  - micro_explanation
  - model_exposure
  - standardize_answer
  - switch_to_new_probe
  - conclude_series

intended_hint_level: H0 | H1 | H2 | H3 | H4 | H5 | H6 | H7
planned_prompt_summary: string
request_new_learner_response: bool
requested_response_mode: oral | arrow_outline | typed_chat | written | artifact | same_as_before | any
```

The exact message wording is UI/prompt implementation. The canonical record captures pedagogical function.

---

## 4. One-layer target

```yaml
intervention_target_node_ids: string[]
intervention_target_dimension:
  null | question_understanding | task_type_recognition | prerequisite_knowledge |
  text_location | evidence_selection | reasoning | strategy_invocation |
  terminology | written_expression | execution

one_layer_exception_reason: string | null
```

Normal rule: one intervention turn targets one causal layer.

A turn may contain three rhetorical parts without violating the rule:

1. acknowledge the correct part;
2. identify one missing link;
3. ask one key question / retry request.

It becomes a multi-layer violation only when the tutor simultaneously teaches several independent missing operations.

---

## 5. Support-budget relation to F1

```yaml
training_move_hint_ceiling: H0 | H1 | H2 | H3 | H4 | H5 | H6 | H7 | null
ceiling_exceeded: bool
ceiling_override_reason: null | learner_blocked | prerequisite_discovered | misconception_repair | closure_needed | accessibility | other
```

F1's hint ceiling is a **planned evidence condition**, not a prohibition on helping the learner.

If actual tutoring requires more support:

- the tutor may exceed the ceiling;
- `ceiling_exceeded=true` is recorded;
- the next Attempt remains valid learning evidence under the actual hint level;
- the move's intended success criterion may remain unmet.

Never withhold pedagogically necessary support merely to preserve a clean score.

---

## 6. Node-specific evidence effects

Hint level is not a blanket penalty. Record which constructs become guided/not independently observable because of the intervention.

```yaml
node_effects:
  - node_id: string
    effect: preserves_independence | weakens_independence | invalidates_independent_observation | enables_observation
    reason: string
```

Examples:

- H4 supplies text location → location Ability loses independent evidence, but downstream reasoning may remain independently observable.
- H2 reminds the evidence→explanation strategy → Strategy automation evidence is weakened, while the learner's actual reasoning execution may still be independently observable.
- H6 supplies the missing warrant → independent evidence for that reasoning node is invalidated for the next response.

C2 should snapshot the actual `max_hint_level_before_response`; C3 interprets node-specific independence using these causal effects.

---

## 7. Expected next evidence

```yaml
expected_next_observation:
  target_node_ids: string[]
  desired_observation: mixed | positive
  desired_independence: guided | partial | independent
  success_signal: string
  failure_signal: string
```

This is not a Profile update rule. It tells the tutor what the next response should reveal.

---

## 8. Escalation / fade instruction

```yaml
if_success:
  next_action: retry_same_level | fade_support | standardize | conclude | switch_to_transfer_probe
  note: string | null

if_still_blocked:
  next_action: repeat_same_level | escalate_one_step | change_diagnosis | inspect_prerequisite | model_then_reprobe
  note: string | null
```

Default policy prefers **diagnostic change before brute-force escalation** when the lower intervention fails unexpectedly.

---

## 9. Standardization / model exposure flag

```yaml
answer_content_exposure:
  none | structure_only | partial_content | near_complete | complete

same_item_independence_recoverable: bool
```

Rules:

- after `near_complete` / `complete` exposure, the same item cannot later provide independent mastery evidence for the supplied content;
- a new sufficiently distinct Question is required for independent verification;
- standardizing an answer **after** the learner has independently supplied the reasoning does not retroactively erase the prior Attempt evidence.

---

## 10. Decision provenance

```yaml
decided_by: tutor_ai | teacher | hybrid
source_attempt_assessment_confidence: unknown | low | medium | high
manual_note: string | null
```

---

## 11. F2 invariants

1. Diagnose before intervening.
2. Preserve the learner's correct reasoning and target the earliest actionable missing link.
3. One turn normally advances one causal layer.
4. Minimum effective intervention beats maximum explanation.
5. Hint level is causal/node-specific, not a global deduction score.
6. F1 hint ceiling may be exceeded when teaching requires it; actual support is logged.
7. Reasoning-correct/expression-weak routes to expression conversion, not comprehension reteaching.
8. H6/H7/model exposure cannot later masquerade as same-item independent evidence.
9. Second response is the normal repair mechanism.
10. Every intervention decision is auditable and can explain why this prompt, at this level, was chosen now.