# R1.4 Notion v2 production-schema gates

Status: **release gate checklist for the created side-by-side production schema**

R1.4 passes only when every gate below is true.

## A. Production object existence

- [x] One dedicated production hub exists under the v2 workbench.
- [x] The hub contains exactly the intended 13 v2 production data sources for graph, content, learner evidence/state and runtime decisions.
- [x] Every production data source has a stable registry entry in `docs/notion/v2-production-schema.md`.
- [x] R1.4 created no replacement copy of a v1 database under a misleading old name.

## B. Semantic separation

- [x] Learning Nodes/Edges contain no learner mastery/review/attempt state.
- [x] Learner Node States points to canonical Learning Nodes rather than duplicating node semantics.
- [x] Materials and Questions contain no learner correctness/mastery/familiarity state.
- [x] TaskType remains a reusable task environment rather than an Ability score.
- [x] Active Requirement is desired state, separate from observed Learner Profile.

## C. Evidence path

- [x] One submitted response can be persisted as one TrainingAttempt.
- [x] Retries can be linked through `attempt_series_id`, `previous_attempt_id` and `previous_attempt`.
- [x] Per-node observation/independence is stored in Attempt Node Evidence, not inferred from overall correctness.
- [x] Attempt retains the relevant context snapshot: familiarity, transfer probe, D2 complexity, modality, hint/support and assessment provenance.
- [x] Legacy reconstruction can explicitly remain uncertain instead of inventing H0/independent/unfamiliar evidence.

## D. Profile audit path

- [x] Learner Node States can represent current M/A/C and per-axis evidence strength independently.
- [x] Profile Update Decisions can link the affected node/state to the source Attempts and node-level evidence.
- [x] Hold/verify-only/no-evidence outcomes are representable; a training event does not have to force an upgrade/downgrade.
- [x] Manual/migration changes have explicit provenance/reason fields.

## E. Recommendation, intervention and review

- [x] Training Move links targets, prerequisites, TaskTypes, selected Question, requirements and recent Attempts.
- [x] Intervention Decision links the source Attempt, TrainingMove, Question and node-level evidence effects.
- [x] The next TrainingAttempt can reference the preceding InterventionDecision.
- [x] Review Decision is separate from Profile and can link node/state, active requirement, verification move and outcome Attempts.
- [x] No Review field directly encodes automatic M/A/C decay.

## F. Relation integrity

- [x] Learning Edge has explicit `from_node` and `to_node` relations.
- [x] LearningNode parent is a self relation; `part_of` is not duplicated as a persisted LearningEdge.
- [x] Question links exactly through Material/TaskType/LearningNode entities rather than embedding mutable copies of their semantics.
- [x] Attempt Node Evidence and Profile Update Decision resolve to canonical Learning Nodes.
- [x] Runtime audit entities use Notion relations for traceability while stable canonical IDs remain explicit record fields.

## G. Non-destructive migration gate

- [x] Existing v1 databases remain present and usable.
- [x] R1.4 did not delete, rename or trash any v1 database.
- [x] No v1 row has been moved or overwritten as part of R1.4.
- [x] No production v1→v2 backfill rows have been claimed as complete.
- [x] Hub and GitHub registry explicitly state `backfill_status = not_started`.
- [x] No source-of-truth cutover is authorized by R1.4.

## H. R1.5 entry gate

R1.5 may start when:

- [x] this schema registry is merged to `main`;
- [x] the Notion production hub and relations have been re-fetched/inspected after creation;
- [x] v1 remains the rollback-safe path;
- [x] backfill rules preserve unknown historical hint/independence/novelty values as unknown.

R1.5 must stop and treat the schema as a blocker if real backfill exposes a semantic field that cannot be represented without violating B1–F3 invariants. It must not silently overload an unrelated property merely to finish migration.
