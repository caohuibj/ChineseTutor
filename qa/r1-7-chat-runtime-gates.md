# R1.7 Chat runtime gates

Status: **native-pilot preflight**

## A. Resolve before present

- [x] Selected TrainingMove resolves from production Notion.
- [x] Selected Question resolves.
- [x] Its single Material resolves.
- [x] Primary/secondary targets resolve.
- [x] Complexity/familiarity/transfer purpose are known before presentation.
- [x] Planned hint ceiling is known before presentation.

## B. No fabricated evidence

- [x] No TrainingAttempt exists for the reserved native series before learner submission.
- [x] No AttemptNodeEvidence exists before learner submission.
- [x] No InterventionDecision is pre-created as though a response had occurred.
- [x] No ProfileUpdateDecision is pre-created.
- [x] Synthetic/mock answers cannot count toward R1.8 counters.

## C. Native telemetry

- [x] First response support condition is H0 with hint_count=0.
- [x] Native Attempts must record actual H0-H7, never null.
- [x] One submitted response = one Attempt.
- [x] Retry = new Attempt in same series.
- [x] `preceding_intervention_decision` links a retry to actual prior intervention.
- [x] Material familiarity and transfer-probe status are snapshotted per Attempt.

## D. Diagnosis and intervention

- [x] Question correctness is not copied to every target node.
- [x] `not_observed` remains distinct from negative.
- [x] Informal expression is assessed separately from reasoning.
- [x] Earliest causal gap drives intervention.
- [x] One intervention normally targets one causal layer.
- [x] Hint-ceiling override is allowed for pedagogy but must be recorded.
- [x] H6/H7 content exposure cannot later masquerade as same-item independent evidence.

## E. Profile projection

- [x] C3 policy is the only authority for M/A/C changes.
- [x] Hold + confidence change is allowed and expected.
- [x] One familiar H0 success cannot establish M3/transfer.
- [x] Variant diversity is required for M2/M3 topology claims.
- [x] Profile update occurs only after durable Attempt/evidence writes.
- [x] ProfileUpdateDecision remains auditable.

## F. Operational failure handling

- [x] Stable Attempt IDs are reserved by convention, not pre-written.
- [x] A storage failure does not force the learner to resubmit solely for storage repair.
- [x] Partial writes reconcile by stable semantic IDs before proceeding.
- [x] Duplicate Attempt IDs are forbidden.
- [x] Profile does not advance after an incomplete evidence write.

## G. Dual-run safety

- [x] v1 source-of-truth switch remains not started.
- [x] v2 pilot may drive controlled recommendations without declaring cutover.
- [x] Raw v2 Attempts are not mechanically copied into v1 ability rows.
- [x] Meaningful session-level recap can continue in existing `学习记录`.
- [x] R1.5 deferred full-table reconciliation remains a cutover blocker.

## H. Pilot launch gate

The next learner-facing question may start R1.8 only when all of these hold:

```text
runtime_status = awaiting_real_learner_response
series = SERIES-r18-01-zhouyafu-46
next_attempt = ATT-r18-01-zhouyafu-46-01
planned_support = H0
question = Q-zhouyafu-46-comparison-function
no native Attempt written yet
```

All conditions are satisfied.

## Exit decision

**GO for R1.8 real-learner pilot.**

R1.7 is complete as an integration contract. R1.8 execution must stop counting whenever the human learner is not the source of the response.