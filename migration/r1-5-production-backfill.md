# R1.5 production backfill reconciliation

Status: **production backfill written; source-of-truth switch not started**

GitHub issue: #45 — R1.5 Non-destructive v1→v2 production backfill

Notion production hub:

`ChineseTutor v2｜Production Data（并行）`

https://app.notion.com/p/3dcd27635a3881a1b554d80acb609b9c?pvs=204

## 1. Safety state

```text
backfill_status = pilot_slice_populated
source_of_truth_switch = not_started
v1_delete_or_archive = false
rollback_path = v1 remains available
```

R1.5 writes only new v2 production rows. It does not modify, rename, archive or delete v1 records.

## 2. Canonical LearningNode backfill

The v1 ability map contains 42 mixed-granularity rows. R1.5 does **not** perform a 42→42 copy. It applies the reviewed B1/B3 migration dispositions and creates diagnostic Knowledge / Ability / Strategy nodes.

Production result:

- LearningNodes: **77**
- distinct `node_id`: **77**
- duplicate canonical IDs at the point of validation: **0**
- mother Strategy nodes: **13**
- v1 row intentionally not represented by a canonical node: **文言课内外迁移**
  - reason: it is learner-relative transfer evidence/context on Attempt, not a stable canonical skill.

The 77 nodes include:

- modern reading / cross-domain evidence, comparison, structure and evaluation abilities;
- classical Knowledge + Ability decomposition;
- poetry Knowledge + Ability decomposition;
- writing prompt, material, narrative, argument, cohesion, language and transfer abilities;
- language-basics Knowledge + diagnosis/revision abilities;
- the 13 reusable mother strategies;
- the minimum Gaokao coverage-gap nodes already approved in R1.3 (`evidence sufficiency`, `rule induction`, `rule application`).

Every v1-derived node carries a `legacy_v1_ref` such as `能力地图::<旧标签>`. Grade/stage fields are not copied into the canonical graph.

## 3. Minimum dependency graph

R1.5 populated **28** reviewed/minimum LearningEdges for the pilot graph. They include:

- hard prerequisites for classical word sense, sentence parsing, translation and sentence revision;
- `strategy_for` edges for mother strategies;
- `supports` edges where knowledge is useful but not a whole-target prerequisite;
- reading→writing `transfers_to` edges for typical-event analysis and detail analysis.

No edge propagates mastery.

## 4. Pilot reusable content

### TaskTypes — 3

| TaskType | Notion page |
| --- | --- |
| `TT-character-analysis` | `3dcd2763-5a38-8154-ba27-d462193dd43c` |
| `TT-typical-event-characterization` | `3dcd2763-5a38-8140-ae90-d02c21d4affb` |
| `TT-comparison-foil-function` | `3dcd2763-5a38-8175-9246-c9d559dfa857` |

These are authentic recurring task environments. They are not Learner abilities.

### Material — 1

`MAT-zhouyafu-school-occurrence`

Notion page: `3dcd2763-5a38-816e-9e23-de98b0288969`

Metadata preserves the learner-facing school occurrence/version. Full copyrighted source text is not duplicated into the registry.

### Questions — 2

| Question | Notion page |
| --- | --- |
| `Q-zhouyafu-45-character-evidence` | `3dcd2763-5a38-8112-8536-e78407a894a9` |
| `Q-zhouyafu-46-comparison-function` | `3dcd2763-5a38-813e-8aa9-e03dd9f49985` |

Both Questions are linked to the Material, TaskTypes and canonical target nodes. Their D2 band is `C2 / provisional`; the historical fixture does not provide a reviewed eight-dimension vector, so R1.5 does not invent one.

## 5. Real historical learner evidence

Only the two real reconstructed project attempts from `evidence/seed/training-attempt-examples.yaml` were migrated. Synthetic fixtures were excluded.

### Attempts — 2

- `ATT-legacy-zhouyafu-45-01`
  - Notion page `3dcd2763-5a38-8131-95d5-e739a49ae2f4`
- `ATT-legacy-zhouyafu-46-01`
  - Notion page `3dcd2763-5a38-8129-b0ae-ebd1b17dc026`

Conservative historical fields:

- learner: `project-learner`
- `legacy_reconstructed = true`
- material familiarity: `familiar`
- transfer probe: `false`
- complexity snapshot: `C2`
- response mode: `typed_chat`
- hint level/count: **null / unknown**
- independence: stored only in node evidence and remains **unknown**

### AttemptNodeEvidence — 4

- 4.5 character judgment: positive / independence unknown
- 4.5 special-context strategy: mixed / independence unknown
- 4.6 comparison dimension: positive / independence unknown
- 4.6 comparison significance: mixed / independence unknown

Overall question correctness is not copied blindly to every target node.

## 6. Conservative LearnerNodeState backfill

Only three nodes receive learner state because only these have sufficiently specific historical node evidence.

| Node | State | Reason |
| --- | --- | --- |
| `CN-A-evidence-to-character-judgment` | `M1 / A1 / C2`, weak evidence | 4.5 node evidence positive; historical hint dependence incomplete |
| `CN-A-establish-comparison-dimension` | `M1 / A1 / C2`, weak evidence | 4.6 dimension evidence positive; only one familiar task |
| `CN-A-explain-comparison-significance` | `M1 / A1 / complexity null`, weak evidence | 4.6 evidence mixed; C2 success not established for this node |

Notion pages:

- character state: `3dcd2763-5a38-819e-9985-e76594b11a37`
- comparison-dimension state: `3dcd2763-5a38-8164-88e5-fdafa17c8b4e`
- comparison-significance state: `3dcd2763-5a38-819c-8336-d4fb368ef1b6`

Important conservative choices:

- no legacy zero becomes M0/A0 automatically;
- no verification timestamp is invented from `最近训练`;
- no `independent_success_count`, `transfer_success_count` or total attempt count is invented from incomplete history;
- no M3 is possible from this backfill;
- no Strategy state is created because autonomous strategy invocation is not evidenced.

## 7. ProfileUpdateDecision audit — 3

Every migrated state has an explicit migration decision:

- `PUD-R1.5-legacy-character-20260915`
- `PUD-R1.5-legacy-comparison-dimension-20260915`
- `PUD-R1.5-legacy-comparison-significance-20260915`

`computed_by = migration`

`policy_version = R1.5-legacy-backfill-v1`

The decisions link the state to its source Attempt and node-level evidence and explicitly list excluded/inferentially unsafe evidence.

## 8. Validation performed

Before the Notion Query Data Source quota was exhausted:

- LearningNode total count = 77;
- distinct `node_id` = 77;
- v1 ability-map labels were read as the complete 42-row set;
- the canonical node registry was separately inspected to confirm legacy references;
- the only intentionally non-node v1 row is `文言课内外迁移`.

After the query quota was exhausted, subsequent writes were validated by successful Notion create responses and stable relation IDs. Full post-write count/reconciliation SQL is therefore a **deferred QA gate**, not claimed as passed.

## 9. Out of scope / still not done

- no production source-of-truth switch;
- no v1 archive/delete;
- no exhaustive Gaokao Knowledge population;
- no bulk migration of all candidate materials/questions;
- no synthetic training history;
- no automatic recommendation/ReviewDecision generation from these weak legacy states;
- no claim that C2 equals transfer;
- no dual-run/native pilot yet.

## 10. R1.6 handoff

Next step is **dual-run / native-pilot preparation**:

1. rerun deferred post-write reconciliation when Notion Query Data Source access is available;
2. generate explainable ActiveRequirements and TrainingMoves from the three conservative states;
3. run new learner Attempts natively into v2 with actual H0–H7 and independence telemetry;
4. compare v1 tutor behavior/state with v2 decisions without switching source of truth;
5. define cutover/rollback gate based on observed pilot evidence, not migration completeness alone.
