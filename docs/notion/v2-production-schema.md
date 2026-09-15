# ChineseTutor v2 production Notion schema

Status: **R1.4 production schema created side-by-side with v1**

This document records the actual Notion production objects created for ChineseTutor v2. It is a deployment registry and write-path contract; semantic meaning remains governed by the canonical GitHub schemas/ADRs already merged to `main`.

## 1. Production hub

Notion page:

`ChineseTutor v2｜Production Data（并行）`

https://app.notion.com/p/3dcd27635a3881a1b554d80acb609b9c?pvs=204

The v2 production databases live under that page. They coexist with the existing v1 databases. R1.4 did **not** delete, rename, overwrite, migrate or switch the source of truth for any v1 record.

```text
backfill_status = not_started
source_of_truth_switch = not_started
rollback_path = v1 remains available
```

---

## 2. Data-source registry

| Production entity | Notion data source |
| --- | --- |
| Learning Nodes | `collection://62a72e51-9b03-4d13-aeb0-4be44b540f7f` |
| Learning Edges | `collection://6912ee4a-8e79-4660-bf82-64df3f44a68e` |
| Learner Node States | `collection://b63ef26e-e860-4c68-9779-348403d50a9b` |
| Materials | `collection://4f591b9a-29d5-4d94-aaa6-7527ee87c234` |
| Task Types | `collection://8ae239d1-abe4-4c7e-a1fe-6491c454297a` |
| Questions | `collection://a6cda4bd-d5bc-4974-8e13-bc3e6676af42` |
| Active Requirements | `collection://3f6de06e-e699-4cb7-988b-4dfd72ca2fa7` |
| Training Moves | `collection://b2adfff7-763e-485e-aefd-2b034375872b` |
| Training Attempts | `collection://74d62afd-db70-4443-8302-f8569083f21d` |
| Attempt Node Evidence | `collection://03d30a34-231e-43aa-9a63-af3c52573737` |
| Profile Update Decisions | `collection://173ac6cd-c1a0-4794-abfd-53992d23a3c9` |
| Intervention Decisions | `collection://0e47f12a-c21b-4312-a5c1-f0546b4471cb` |
| Review Decisions | `collection://209844d5-06d6-447d-8551-1d24cbf16595` |

These IDs are deployment identifiers, not canonical semantic identities. Canonical node/question/etc. IDs remain fields inside the corresponding records.

---

## 3. Write-path ownership

```text
Canonical graph
  -> Learning Nodes
  -> Learning Edges

Reusable content
  -> Materials
  -> Task Types
  -> Questions

Observed learner evidence
  -> Training Attempts
  -> Attempt Node Evidence

Current learner projection
  -> Learner Node States

Profile-state transition audit
  -> Profile Update Decisions

Desired learner state / active demand
  -> Active Requirements

Next-best training action
  -> Training Moves

In-session adaptive tutoring decision
  -> Intervention Decisions

Time/freshness/re-verification decision
  -> Review Decisions
```

A durable session recap in the existing `学习记录` remains a promoted human-readable synthesis; it is not the raw evidence ledger.

---

## 4. Relation topology

### Canonical graph

- `Learning Nodes.parent_node -> Learning Nodes`
- `Learning Edges.from_node -> Learning Nodes`
- `Learning Edges.to_node -> Learning Nodes`

Only `requires`, `supports`, `strategy_for`, `transfers_to`, and `contrasts_with` are persisted as Learning Edges. `part_of` remains a virtual projection of `parent_node`.

### Content

- `Materials.component_materials -> Materials`
- `Task Types.default_{knowledge,ability,strategy}_nodes -> Learning Nodes`
- `Questions.material -> Materials`
- `Questions.primary_task_type -> Task Types`
- `Questions.secondary_task_types -> Task Types`
- `Questions.parent_question -> Questions`
- `Questions.{knowledge,ability,strategy}_nodes -> Learning Nodes`
- `Questions.primary_target_nodes -> Learning Nodes`

### Requirements and recommendation

- `Active Requirements.node -> Learning Nodes`
- `Training Moves.primary_target_nodes -> Learning Nodes`
- `Training Moves.secondary_target_nodes -> Learning Nodes`
- `Training Moves.blocking_or_unverified_prereq_nodes -> Learning Nodes`
- `Training Moves.preferred_task_types -> Task Types`
- `Training Moves.avoid_task_types -> Task Types`
- `Training Moves.selected_question -> Questions`
- `Training Moves.active_requirements -> Active Requirements`
- `Training Moves.recent_attempts -> Training Attempts`

### Evidence and profile update

- `Learner Node States.node -> Learning Nodes`
- `Training Attempts.question -> Questions`
- `Training Attempts.training_move -> Training Moves`
- `Training Attempts.previous_attempt -> Training Attempts`
- `Training Attempts.preceding_intervention_decision -> Intervention Decisions`
- `Attempt Node Evidence.attempt -> Training Attempts`
- `Attempt Node Evidence.node -> Learning Nodes`
- `Profile Update Decisions.node -> Learning Nodes`
- `Profile Update Decisions.learner_state -> Learner Node States`
- `Profile Update Decisions.source_attempts -> Training Attempts`
- `Profile Update Decisions.source_node_evidence -> Attempt Node Evidence`

### Tutoring and review

- `Intervention Decisions.after_attempt -> Training Attempts`
- `Intervention Decisions.training_move -> Training Moves`
- `Intervention Decisions.question -> Questions`
- `Intervention Decisions.{intervention_target_nodes,blocked_or_unverified_nodes,preserved_independent_nodes} -> Learning Nodes`
- `Review Decisions.node -> Learning Nodes`
- `Review Decisions.learner_state -> Learner Node States`
- `Review Decisions.active_requirements -> Active Requirements`
- `Review Decisions.training_move -> Training Moves`
- `Review Decisions.outcome_attempts -> Training Attempts`

`Intervention Decisions.after_attempt` means the learner Attempt after which the intervention decision is made; the decision is subsequently referenced by the next Attempt through `preceding_intervention_decision`.

---

## 5. Production invariants

### 5.1 Canonical graph is learner-free

Do not write mastery, automation, review timing, student score, hint dependence or attempt counts into Learning Nodes/Edges.

### 5.2 Question is reusable content, not learner state

Question stores provenance, prompt/rubric, canonical targets and reviewed D2 task complexity. Familiarity, novelty, transfer status, correctness, hint use and learner training status live in Attempt/Profile/Move.

### 5.3 One submitted response is one TrainingAttempt

A retry on the same concrete Question creates a new Attempt in the same `attempt_series_id`. Per-node observations are normalized into Attempt Node Evidence. Overall correctness must never be copied blindly to every mapped node.

### 5.4 Unknown is not failure

Missing historical evidence remains null/unknown. It must not be backfilled as M0/A0/C0, `negative`, H0, independent or unfamiliar without direct evidence.

### 5.5 Profile changes require an auditable reason

Native runtime mutation of Learner Node States must be backed by a Profile Update Decision. Migration/manual override is allowed only with explicit provenance/reason. Profile is a current projection, not the evidence ledger.

### 5.6 Time does not directly change M/A/C

Review Decisions change review/freshness need and schedule verification. Actual mastery/automation/verified-complexity changes still require learner evidence and C3 profile-update semantics.

### 5.7 Novelty is learner-relative

Unfamiliarity and transfer probes are TrainingAttempt/TrainingMove context. They do not become intrinsic Question complexity fields, and no C-band proves transfer by itself.

### 5.8 v1 stays rollback-safe through the pilot

R1.4 does not change the v1 source of truth. A production cutover is forbidden until R1.5 backfill validation, dual-run, native pilot and rollback release gates pass.

---

## 6. Notion implementation choices

Notion is used as a practical production store rather than as the semantic specification itself. A few implementation details therefore intentionally differ from the normalized conceptual model:

- `Attempt Node Evidence` is a separate table so one TrainingAttempt can carry multiple independently assessed node observations without embedding opaque arrays.
- nullable semantic values are represented by empty Notion properties; select options do not introduce fake `unknown` levels where the canonical model requires null.
- deployment relations use Notion page relations, while stable semantic IDs (`CN-*`, `Q-*`, etc.) remain explicit fields and are the migration/reconciliation keys.
- relation direction is optimized for the main write/read path; not every relation is mirrored back into the canonical entity.

---

## 7. R1.5 handoff

R1.5 is **non-destructive backfill**, not schema redesign.

Minimum scope:

1. create/backfill canonical v2 nodes corresponding to the existing 42-row v1 ability map according to the reviewed B1 migration disposition;
2. retain traceable `legacy_v1_ref` values;
3. populate the current-learner/pilot minimum Knowledge, TaskType, Material and Question slice;
4. reconstruct only historical learner evidence that is genuinely supported;
5. leave unknown historical hint level, independence, familiarity/novelty and transfer status unknown;
6. create conservative Learner Node States from supported evidence, never by copying legacy labels mechanically;
7. validate record counts, references and rollback before any source-of-truth switch.

Out of scope for R1.4 and still not done:

- production backfill rows;
- v1 archival/deletion;
- source-of-truth cutover;
- exhaustive Gaokao Knowledge population;
- full authentic task ladders;
- pilot evidence collection.
