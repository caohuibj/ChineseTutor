# R1.5 production backfill gates

Status legend: `PASS` / `DEFERRED` / `FAIL`

R1.5 is intentionally non-destructive. A `DEFERRED` post-write query gate blocks source-of-truth cutover but does not invalidate already-created side-by-side v2 rows.

## A. Canonical graph migration

| Gate | Status | Evidence |
| --- | --- | --- |
| v1 ability map read as 42 rows | PASS | Notion v1 query returned the complete 42-label set |
| reviewed B1/B3 migration semantics used instead of 42→42 copying | PASS | 77 typed canonical nodes created from split/merge/task/transfer dispositions |
| `node_id` unique | PASS | pre-content validation: 77 total / 77 distinct |
| grade/stage fields excluded from canonical semantics | PASS | v2 LearningNode writes contain no learner grade progression fields |
| `文言课内外迁移` not modeled as fake canonical skill | PASS | kept as learner-relative Attempt/transfer context |
| all v1-derived canonical nodes retain traceable `legacy_v1_ref` | PASS | production node writes use `能力地图::<label>` references |

## B. Canonical graph integrity

| Gate | Status | Evidence |
| --- | --- | --- |
| only B2 persisted edge types used | PASS | requires / supports / strategy_for / transfers_to only in R1.5 writes |
| no persisted `part_of` edge | PASS | hierarchy remains `parent_node`-based/virtual |
| no mastery propagation encoded in edges | PASS | LearningEdges contain semantic relation metadata only |
| minimum pilot dependencies exist | PASS | 28 production LearningEdges written successfully |
| hard prerequisites are semantic necessities rather than grade order | PASS | classical translation/revision prerequisites are skill dependencies |
| full post-write duplicate edge/count SQL | DEFERRED | Notion Query Data Source quota reached before final aggregate query |

## C. Content separation

| Gate | Status | Evidence |
| --- | --- | --- |
| TaskType is separate from Ability | PASS | three pilot TaskTypes link to canonical Ability/Strategy nodes |
| Material is separate from Question | PASS | one Zhou Yafu Material supports two Question rows |
| learner state absent from Question | PASS | no correctness/mastery/hint/familiarity learner fields written to Questions |
| source grade is metadata only | PASS | Material records `八年级`; no progression rule uses it |
| copyrighted full school material not duplicated | PASS | production Material stores source/content references and metadata only |
| question prompt provenance preserved | PASS | both pilot prompts marked school-provided/verbatim |

## D. Historical evidence reconstruction

| Gate | Status | Evidence |
| --- | --- | --- |
| only real project history migrated | PASS | 2 Zhou Yafu attempts migrated; synthetic fixtures excluded |
| one submitted response = one TrainingAttempt | PASS | 4.5 and 4.6 are separate Attempt rows |
| per-node evidence separated from overall correctness | PASS | 4 AttemptNodeEvidence rows created |
| hint level/count not fabricated | PASS | historical hint fields left null |
| independence not fabricated | PASS | all reconstructed node evidence uses `independence=unknown` |
| transfer/novelty not inferred from C-band | PASS | attempts are familiar, transfer_probe=false; C2 retained only as task complexity |
| source assessment provenance explicit | PASS | legacy attempts mark tutor assessment + medium reconstruction confidence |

## E. Learner Profile backfill

| Gate | Status | Evidence |
| --- | --- | --- |
| legacy zeros not copied to M0/A0 | PASS | states created only for 3 nodes with specific evidence |
| split v1 score not copied to every child | PASS | only evidenced comparison child nodes receive state |
| M3 not initialized from legacy history | PASS | all migrated mastery is M1 |
| verification timestamps not copied from last-trained date | PASS | mastery/automation/complexity verification timestamps remain null |
| incomplete historical counters remain null | PASS | no synthetic total attempt/independent/transfer counts |
| mixed C2 evidence does not create verified C2 | PASS | comparison-significance state has complexity null |
| every state change is auditable | PASS | 3 migration ProfileUpdateDecisions link state, Attempt and node evidence |

## F. Rollback / cutover safety

| Gate | Status | Evidence |
| --- | --- | --- |
| no v1 delete/rename/archive | PASS | R1.5 writes only to v2 production databases |
| v1 remains usable | PASS | no source-of-truth mutation performed |
| source-of-truth switch remains off | PASS | migration manifest explicitly records `not_started` |
| post-write aggregate reconciliation across all production tables | DEFERRED | Notion Query Data Source quota reached |
| dual-run pilot passed | DEFERRED | belongs to R1.6 |
| cutover authorized | DEFERRED | requires R1.6 evidence and rollback gate |

## Release decision

R1.5 may be merged as a **side-by-side production backfill release** because all semantic and non-destructive write gates pass.

It must **not** trigger source-of-truth cutover. Final aggregate reconciliation and native dual-run evidence remain explicit blockers for cutover.
