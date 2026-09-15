# ProfileUpdateDecision audit schema

Status: **C3 normative draft**

`ProfileUpdateDecision` is the auditable record of how a bounded set of learner evidence changed, or deliberately did not change, one `LearnerNodeState`.

It is **not** the source evidence itself. C2 `TrainingAttempt` remains the evidence ledger. It is also not the current Profile; C1 `LearnerNodeState` remains the current projection.

The purpose of this record is explainability and reproducibility:

```text
TrainingAttempt evidence
        ↓
ProfileUpdateDecision
        ↓
LearnerNodeState
```

---

## 1. Identity

One decision concerns one learner and one canonical LearningNode.

```yaml
update_decision_id: PUD-...
learner_id: string
node_id: string
policy_version: string
computed_at: datetime
computed_by: rule_engine | tutor | teacher | migration | mixed
```

A batch recomputation may produce many decisions, one per node.

---

## 2. Evidence references

```yaml
source_attempt_ids: string[]
source_artifact_refs: string[]
legacy_evidence_refs: string[]
```

At least one evidence reference is required for evidence-derived changes.

A manual operational correction may instead use:

```yaml
manual_override_reason: string | null
```

but must not erase or rewrite the underlying evidence history.

---

## 3. Before / after state snapshots

Store the state dimensions relevant to the decision so historical reasoning remains interpretable even if the current Profile later changes.

```yaml
before:
  mastery: null | M0 | M1 | M2 | M3
  automation: null | A0 | A1 | A2 | A3
  verified_complexity_band: null | C0 | C1 | C2 | C3 | C4 | C5
  mastery_evidence_strength: none | weak | moderate | strong
  automation_evidence_strength: none | weak | moderate | strong
  complexity_evidence_strength: none | weak | moderate | strong
  primary_error_pattern: null | K | R | I | E | Q | M | C

after:
  mastery: null | M0 | M1 | M2 | M3
  automation: null | A0 | A1 | A2 | A3
  verified_complexity_band: null | C0 | C1 | C2 | C3 | C4 | C5
  mastery_evidence_strength: none | weak | moderate | strong
  automation_evidence_strength: none | weak | moderate | strong
  complexity_evidence_strength: none | weak | moderate | strong
  primary_error_pattern: null | K | R | I | E | Q | M | C
```

Full Profile snapshots are optional; these core dimensions are sufficient for C3 audit.

---

## 4. Per-axis decision

Each axis is updated independently.

```yaml
mastery_decision:
  action: no_evidence | verify_only | hold | upgrade | downgrade | initialize
  from: null | M0 | M1 | M2 | M3
  to: null | M0 | M1 | M2 | M3
  confidence_effect: none | increase | decrease | replace
  rationale: string

automation_decision:
  action: no_evidence | verify_only | hold | upgrade | downgrade | initialize
  from: null | A0 | A1 | A2 | A3
  to: null | A0 | A1 | A2 | A3
  confidence_effect: none | increase | decrease | replace
  rationale: string

complexity_decision:
  action: no_evidence | verify_only | hold | upgrade | downgrade | initialize
  from: null | C0 | C1 | C2 | C3 | C4 | C5
  to: null | C0 | C1 | C2 | C3 | C4 | C5
  confidence_effect: none | increase | decrease | replace
  rationale: string
```

A single Attempt may therefore produce:

```text
mastery: hold M2, confidence increase
automation: upgrade A1 -> A2
complexity: hold C2
```

This is expected, not exceptional.

---

## 5. Evidence summary

The decision stores an interpretable summary of the evidence topology, not a hidden scalar score.

```yaml
evidence_summary:
  positive_observations: int
  mixed_observations: int
  negative_observations: int
  not_observed: int

  independent_observations: int
  guided_observations: int
  partial_observations: int

  distinct_material_contexts: int | null
  distinct_task_types: int | null
  distinct_variant_groups: int | null
  unfamiliar_transfer_successes: int | null

  strongest_success_complexity: null | C0 | C1 | C2 | C3 | C4 | C5
  strongest_independent_success_complexity: null | C0 | C1 | C2 | C3 | C4 | C5

  recent_contradiction_present: bool
  legacy_only: bool
  assessment_confidence_floor: low | medium | high | mixed | unknown
```

Counts are descriptive. They are not themselves transition rules.

---

## 6. Evidence exclusions / discount reasons

To make non-updates explainable:

```yaml
excluded_or_discounted_evidence:
  - attempt_id: string
    reason: null_target_observation | prerequisite_blocked | excessive_scaffold | near_duplicate | low_assessment_confidence | stale_conflict | legacy_uncertainty | modality_mismatch | other
    note: string | null
```

An Attempt may still be useful for another node even if discounted for this node.

---

## 7. Error-pattern decision

```yaml
error_pattern_decision:
  action: no_evidence | candidate | confirm | hold | clear | replace
  from: null | K | R | I | E | Q | M | C
  to: null | K | R | I | E | Q | M | C
  evidence_strength_after: none | weak | moderate | strong
  rationale: string
```

One error occurrence should normally be `candidate` or `no_evidence`, not `confirm`.

---

## 8. Verification timestamps

Axis-specific timestamps update only when evidence actually verifies that axis.

```yaml
mastery_verified_at_after: datetime | null
automation_verified_at_after: datetime | null
complexity_verified_at_after: datetime | null
last_trained_at_after: datetime | null
```

A successful untimed answer may update mastery verification without updating automation verification.

---

## 9. C3 invariants

1. Every state-changing decision references evidence or an explicit manual override.
2. M/A/C decisions are independent.
3. A hold can still increase evidence confidence.
4. A failure can reduce confidence without immediately downgrading state.
5. `not_observed` evidence never acts as negative evidence.
6. Graph edges do not create Profile evidence by themselves.
7. Decision rationale is human-readable.
8. The same Attempt may have different update effects for different nodes.
9. Recomputing with a later `policy_version` does not delete historical decisions.
10. No opaque global learner score is required.