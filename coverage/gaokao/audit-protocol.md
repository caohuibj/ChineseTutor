# I1 Gaokao coverage-audit protocol

Status: **normative draft**

The target boundary of ChineseTutor is full Gaokao knowledge/capability coverage, but the learning path remains graph-driven rather than grade-driven. I1 therefore uses recent authentic Gaokao demands as **coverage tests for the canonical model**, not as a curriculum sequence for the current learner.

## 1. Audit unit

The unit is a stable task demand evidenced by one or more authentic questions/official analyses:

```text
Material
  -> TaskType
  -> Knowledge demands
  -> Ability demands
  -> recommended Mother Strategy
  -> Dependency assumptions
  -> D2 Complexity
```

The audit asks whether the current architecture can represent the demand cleanly. It does not require the current student to attempt the Gaokao item.

## 2. Source policy

Priority for I1 evidence:

1. imported authentic paper/answer in the existing `高考核心参考` workspace;
2. official Beijing / Ministry examination analysis already linked in the workspace;
3. the existing project synthesis `高考真题能力证据提取｜2022—2026北京＋2025—2026全国`;
4. secondary public mirrors only for question wording/provenance when the project source already marks them as non-official.

Do not invent question numbers, answer wording or exact stems when the imported source summary does not expose them. In fixtures, use a stable descriptive `demand_label` and set `question_no: unknown_in_current_extraction` when necessary.

## 3. Coverage statuses

Each fixture receives one status:

```text
cleanly_mapped
mapped_with_candidate_node_gap
mapped_with_tasktype_gap
mapped_with_knowledge_gap
mapped_with_material_gap
architecture_gap
```

Definitions:

- `cleanly_mapped`: existing entity types and current semantic nodes/strategies are sufficient.
- `mapped_with_candidate_node_gap`: architecture is sufficient, but a recurring Knowledge/Ability node is not yet canonicalized.
- `mapped_with_tasktype_gap`: underlying nodes exist/are expressible, but the authentic task environment needs a stable TaskType.
- `mapped_with_knowledge_gap`: recurring required knowledge family is not yet represented at useful granularity.
- `mapped_with_material_gap`: model is fine, but the authentic inventory lacks enough traceable examples across complexity.
- `architecture_gap`: current B1–G1 entities cannot express the demand without changing schema/semantics.

An `architecture_gap` is the most serious outcome. A missing row/node is not automatically an architecture failure.

## 4. No ad-hoc template rule

When a recurring demand is not cleanly represented:

```text
Do NOT create a new answer template named after the exam question.
```

Instead classify the gap as:

```text
Knowledge gap
Ability gap
TaskType gap
Dependency gap
Complexity-model gap
Material-coverage gap
```

Then open a follow-up issue with an observable construct and examples from more than one demand where possible.

## 5. Strategy test

G1 mother Strategies are stress-tested, not assumed complete.

A Gaokao demand may:

- use one mother Strategy;
- compose 2–3 Strategies;
- use no Strategy because Knowledge/Ability is sufficient;
- expose a genuinely new reusable procedure.

A new Strategy is justified only when existing compositions cannot represent the procedure without diagnostic loss. Surface novelty is insufficient.

## 6. Ability test

A recurring operation deserves a candidate Ability node when:

1. the learner can succeed/fail on it independently of neighboring operations;
2. it appears across multiple materials/tasks or is clearly terminally important;
3. different failure would trigger different tutoring/recommendation actions;
4. observable success can be stated without naming one specific exam item.

## 7. Knowledge test

Knowledge is separated from Ability. Example:

```text
Knowledge: 论证概念、文体特征、文言文化制度、典故
Ability: 用这些知识判断当前材料、评价论证或解释效果
```

Do not create an Ability merely because the student must know a fact.

## 8. Complexity test

Use the D2 vector:

```text
text_load
information_hiddenness
reasoning_depth
material_heterogeneity
knowledge_retrieval_distance
response_openness
expression_load
time_pressure
```

Gaokao source/year is metadata and never a reason for C-band assignment.

Novelty/transfer remains learner-relative C2 Attempt context, not intrinsic complexity.

## 9. Audit corpus design

The representative fixture set must cover:

- information / multi-text reading;
- literary reading;
- classical Chinese;
- poetry;
- language use / real communication;
- writing;
- Beijing and national papers;
- closed, constructed and open/design responses;
- both clean mappings and genuine gaps.

The corpus is representative, not exhaustive. I1's goal is to discover structural omissions before mass backfill.

## 10. Completion criterion

I1 is complete when:

1. representative recent demands map into the canonical model;
2. every recurring unmapped demand is entered in `gap-register.yaml` and linked to an explicit GitHub issue;
3. no issue proposes a question-specific template as the fix;
4. the audit can state whether B1–G1 is structurally expressive enough for Gaokao coverage;
5. remaining work is clearly separated into graph population, TaskType expansion, authentic material supply and runtime calibration.
