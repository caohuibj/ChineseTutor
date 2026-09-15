# R1.7 — Chat runtime production contract

Status: **runtime-ready for native learner pilot**

Issue: #49

## 1. Runtime boundary

The production runtime for the v2 MVP is the actual tutoring conversation in ChatGPT plus the v2 Notion production stores.

R1.7 does **not** introduce a second application/server layer merely to satisfy an architecture diagram. The runtime contract is the deterministic choreography that converts real conversation events into the canonical production entities already defined by C2/C3/F1/F2.

```text
selected TrainingMove
  -> resolve Question + Material + target nodes
  -> present planned support condition
  -> wait for actual learner response
  -> TrainingAttempt
  -> AttemptNodeEvidence
  -> assess earliest causal gap
  -> if needed: InterventionDecision
  -> wait for actual learner retry
  -> new TrainingAttempt in same series
  -> new AttemptNodeEvidence
  -> C3 ProfileUpdateDecision when evidence warrants
  -> LearnerNodeState projection
  -> TrainingMove complete/supersede/recompute
```

No step may manufacture learner evidence.

## 2. Pre-response invariant

Before the learner submits a response, runtime may create or update:

- TrainingMove selection/state;
- Question/Material curation;
- runtime preflight metadata.

It must **not** create:

- TrainingAttempt;
- AttemptNodeEvidence;
- InterventionDecision pretending to respond to a learner answer;
- ProfileUpdateDecision based on an answer that has not happened.

This distinction is release-critical.

## 3. First production runtime binding

R1.7 resolved the current selected move from Notion:

```yaml
training_move_id: TM-r16-01-comparison-significance-h0-baseline
training_move_page_id: 3dcd2763-5a38-8180-a36a-cfd1a8180cda
question_id: Q-zhouyafu-46-comparison-function
question_page_id: 3dcd2763-5a38-813e-8aa9-e03dd9f49985
material_id: MAT-zhouyafu-school-occurrence
material_page_id: 3dcd2763-5a38-816e-9e23-de98b0288969
primary_target: CN-A-explain-comparison-significance
secondary_target: CN-A-establish-comparison-dimension
move_type: fade_scaffold
complexity: C2
material_familiarity: familiar
transfer_probe: false
planned_hint_ceiling: H0
response_mode: typed_chat
```

The move was updated with this runtime preflight binding:

```yaml
session_id: SESSION-r18-pilot-01
attempt_series_id: SERIES-r18-01-zhouyafu-46
next_attempt_number: 1
support_before_response: H0
```

No Attempt exists yet for this native series.

## 4. Presentation rule

The current Question is presented without method reminder, comparison template, evidence cue or answer skeleton because the TrainingMove explicitly requests H0.

The learner may answer in conversational language. Runtime must not penalize informal phrasing before distinguishing reasoning from written-expression quality.

For the first move, the learner-facing prompt is simply:

> 霸上、棘门军与细柳营的对比有何作用？

No hidden “记得写差异→突出作用” hint is added before Attempt #1.

## 5. On learner submission: append Attempt first

A substantive learner answer creates exactly one `TrainingAttempt`.

For the first response the runtime will use:

```yaml
attempt_id: ATT-r18-01-zhouyafu-46-01
attempt_series_id: SERIES-r18-01-zhouyafu-46
session_id: SESSION-r18-pilot-01
attempt_number: 1
question: Q-zhouyafu-46-comparison-function
training_move: TM-r16-01-comparison-significance-h0-baseline
response_mode: typed_chat
max_hint_level_before_response: H0
hint_count_before_response: 0
intervention_type: none
material_familiarity: familiar
transfer_probe: false
legacy_reconstructed: false
complexity_band_snapshot: C2
```

`raw_answer` may be retained in Notion because it is the actual learner response needed for audit; it must not be copied into public GitHub fixtures.

## 6. Assessment: overall dimensions and node evidence are separate

After storing the response context, runtime assesses only observable dimensions.

For the current Question:

- `CN-A-establish-comparison-dimension` can be positive even if significance reasoning remains mixed;
- `CN-A-explain-comparison-significance` can be mixed/negative independently;
- overall `answer_correctness` must not be copied to both nodes.

Native node independence is derived from actual support:

- if Attempt #1 genuinely performs the operation at H0, node independence can be `independent`;
- if the answer never exposes a node, use `not_observed` rather than `negative`;
- informal wording can coexist with positive reasoning evidence and weaker written-expression score.

Assessment provenance:

```yaml
assessed_by: tutor_ai
assessment_policy_version: R1.7-chat-runtime-v1
assessment_confidence: low | medium | high
```

## 7. Intervention decision occurs only after the Attempt

If the response is sufficient for the local move:

```text
Attempt + node evidence
  -> no repair intervention required
  -> optional expression normalization only after reasoning is established
  -> C3 projection decision
  -> move close / next probe
```

If one causal gap is clear:

```text
Attempt + node evidence
  -> InterventionDecision
  -> tutor sends smallest useful prompt
  -> wait for retry
```

The InterventionDecision must preserve correct parts and target one earliest actionable layer.

Example if the learner again states only “形成对比，突出周亚夫” without explaining the discriminating difference:

```yaml
primary_causal_layer: I
missing_link: 具体差异怎样证明/突出周亚夫治军严整、军纪严明
suggested_action_family: guiding_question
intended_hint_level: H3
request_new_learner_response: true
answer_content_exposure: none
```

The actual wording still follows the project rule:

1. identify one thing already right;
2. isolate one missing link;
3. ask one question;
4. return the work to the learner.

## 8. Retry rule

Every meaningful retry is a **new Attempt** in the same series.

Example:

```yaml
attempt_id: ATT-r18-01-zhouyafu-46-02
attempt_series_id: SERIES-r18-01-zhouyafu-46
attempt_number: 2
previous_attempt: ATT-r18-01-zhouyafu-46-01
preceding_intervention_decision: INT-r18-01-zhouyafu-46-after-01
max_hint_level_before_response: actual intervention level
hint_count_before_response: actual count
```

The retry's independence is node-specific. A strategy reminder may weaken strategy automation evidence while preserving independent execution evidence for another node.

## 9. Profile update rule

C3 is the only semantic authority for M/A/C projection.

Runtime never applies rules such as:

```text
correct answer => M2
retry success => A2
hard question correct => M3
```

Instead it emits a `ProfileUpdateDecision` that can:

- hold state and increase confidence;
- hold state and decrease confidence;
- initialize;
- upgrade;
- downgrade conservatively;
- update only one of M/A/C;
- record a candidate error pattern without promoting it to a stable pattern.

For the first H0 baseline, one native positive Attempt can materially replace legacy uncertainty, but one familiar repeated Question cannot establish transfer or M3.

## 10. TrainingMove lifecycle

`selected` means runtime is waiting to execute the move.

After local evidence is collected:

- `completed` when the move's evidence purpose is satisfied;
- `superseded` when new evidence changes the best next action;
- `cancelled` only for an explicit operational reason.

Completing a Move does not imply the related ActiveRequirement is satisfied. C3 + F1 recompute that separately.

## 11. Dual-run / rollback behavior during pilot

v2 pilot evidence is written natively to the v2 stores.

v1 remains the release rollback/source-of-truth baseline until the later cutover gate. During R1.8:

- do not mirror every raw Attempt into the v1 ability map;
- do not mechanically copy v2 M/A/C into v1 fields;
- keep the existing human-readable `学习记录` workflow for meaningful session-level recap/breakthroughs;
- preserve v1 data unchanged enough to compare and roll back;
- v2 recommendations may drive the controlled pilot without declaring production cutover.

## 12. Runtime error handling

If a production write fails after the learner responded:

1. do not ask the learner to repeat solely to repair storage;
2. retain the response in current chat context;
3. retry the storage operation when safe;
4. do not create duplicate Attempt IDs;
5. if partial write occurred, reconcile by stable semantic IDs before continuing;
6. do not update Profile unless the underlying Attempt/evidence write is durable.

If Question/Material cannot be resolved before presentation, do not start the evidence series.

## 13. R1.8 pilot counters

The release integration floor remains:

- 20–40 **native** TrainingAttempts;
- at least four task/domain families;
- at least five intervention → retry sequences.

These are integration-test floors, not psychometric mastery thresholds.

Only real learner submissions count.

## 14. R1.7 exit condition

R1.7 is complete when:

```text
selected move resolves
+ Question resolves
+ Material resolves
+ support condition is known
+ stable session/series IDs are reserved
+ no pre-response Attempt exists
+ next real learner response can be appended as native C2 evidence
```

That condition is satisfied for `TM-r16-01-comparison-significance-h0-baseline`.
