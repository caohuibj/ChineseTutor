# F2 amendment to C2 TrainingAttempt

Status: **normative stacked amendment**

F2 adds an auditable tutor-decision object between successive C2 Attempts. Native Attempt events should therefore support an optional reference to the decision that shaped the support available before that response.

Add:

```yaml
preceding_intervention_decision_id: string | null
```

Semantics:

- first response to a newly presented Question normally has `null` unless planned F1/F2 support was explicitly delivered before the first response;
- a repaired second/third response references the `InterventionDecision` immediately preceding it;
- the Attempt continues to snapshot actual `max_hint_level_before_response`, because historical evidence must remain interpretable even if the decision object is later reclassified;
- decision reference and hint snapshot must not contradict each other without an audit note.

F2 also permits richer intervention functions than C2's original compact enum. Until schemas are consolidated, map F2 actions into C2 fields as follows:

| F2 action family | C2 compact projection |
| --- | --- |
| task_reframe | task_reminder |
| strategy_activation | strategy_reminder |
| knowledge_recall_cue | strategy_reminder + intervention_summary (lossy compatibility) |
| guiding_question | guiding_question |
| location_narrowing | text_location |
| evidence_focus | evidence_supply when evidence is actually supplied; otherwise guiding_question |
| reasoning_bridge | partial_reasoning |
| micro_explanation | partial_reasoning + intervention_summary (lossy compatibility) |
| model_exposure | near_answer |
| expression_conversion | task_reminder/none + intervention_summary; reasoning independence must be interpreted from F2 node effects |

Long-term schema cleanup should replace this lossy compact projection with either:

1. a richer `intervention_function` enum on Attempt; or
2. reliance on `preceding_intervention_decision_id` plus only the H-level snapshot.

F2 recommendation: option 2 is cleaner. Attempt should record evidence conditions; InterventionDecision should own pedagogical intent.