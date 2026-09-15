# QA gates — ChineseTutor v2 MVP

## Stack convergence

1. Merge order is `#10 → #11 → #12 → #13 → #14 → #15 → #16 → #18 → #19 → #22 → #27 → #28 → #37`.
2. Retarget only the next child to `main` after its parent merges.
3. Inspect every retargeted diff for duplicated/dropped parent changes.
4. Rerun each PR's semantic QA/fixtures before merge.
5. Keep #10 high-level; later schemas remain normative detail.

## MVP scope

6. MVP means a trustworthy production learner loop, not exhaustive Gaokao completion.
7. Grade/year remains provenance, never progression logic.
8. #31 evidence-sufficiency Ability is resolved before pilot.
9. #30 rule-induction/rule-application semantics exist before pilot.
10. #29/#32/#33 are post-launch unless pilot tasks directly require them.
11. #34 only needs pilot-used stable TaskTypes pre-launch.
12. #35 only needs current learner/pilot Knowledge + prerequisites pre-launch.
13. #36 only needs minimum ladders for active high-value nodes pre-launch.

## Production data

14. Canonical LearningNode/edge state is separate from LearnerNodeState.
15. Material/TaskType/Question contain no learner-specific training status.
16. Attempt evidence is the source for Profile changes or explicit reviewed legacy backfill.
17. `unknown` is not converted to weak during migration.
18. Split/merge rows do not blindly inherit one legacy mastery value.
19. Historical reconstruction is labeled lower-confidence; unsupported H-level telemetry is not invented.
20. v1 remains intact during pilot.
21. Rollback to v1-only operation is documented and executable.
22. Private learner data stays out of public GitHub.

## Runtime

23. Every session has a TrainingMove/evidence purpose.
24. Question eligibility respects demonstrated hard prerequisites.
25. First response is preserved separately from retry.
26. Intervention normally repairs one earliest causal layer.
27. Expression-only weakness does not trigger comprehension reteaching.
28. H6/H7/model exposure cannot count as independent evidence for supplied content.
29. Tutor explanation alone is not learner evidence; retry is normally required.
30. Every Profile update is auditable.
31. Persistence failure cannot silently mutate Profile state.

## Pilot

32. At least 20 native TrainingAttempts are captured before MVP declaration; 30 is preferred for normal-use confidence.
33. Pilot spans at least four task/domain families, including modern reading and classical Chinese.
34. At least five first-answer → intervention → retry sequences are observed.
35. At least one scaffold-fade/automation move is exercised.
36. Review is tested when naturally due, or explicitly marked not-yet-testable because insufficient time elapsed.
37. Every Profile-relevant Attempt traces to Question + node + support + observation + ProfileUpdateDecision.
38. Same-variant repetition cannot fabricate M3/transfer.
39. Missing eligible Questions surface as a content-supply gap rather than bad recommendation repetition.
40. Any unexplained Profile upgrade, repeated false prerequisite blocking, evidence loss, destructive migration, or systematically wrong intervention is release-blocking.

Pilot counts are engineering integration-test floors, **not psychometric sample sizes or C3 mastery thresholds**.

## R1 definition of done

- [x] explicit v2 MVP operational definition;
- [x] #10→#37 merge train documented;
- [x] #29–#36 classified by launch criticality;
- [x] production/runtime/pilot/rollback gates defined;
- [x] falsifiable MVP finish line defined;
- [x] release-readiness PR opened (#39).
