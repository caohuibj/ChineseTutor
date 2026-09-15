# TaskType G1 amendment — mother Strategy mapping

Status: **G1 amendment to D1 TaskType**

D1 already allows `recommended_strategy_node_ids`. G1 refines the mapping so a TaskType can distinguish its normal primary procedure from optional secondary composition without turning Strategy into a hard prerequisite.

---

## 1. Recommended mapping shape

Preferred representation:

```yaml
recommended_strategies:
  - strategy_node_id: CN-S-...
    role: primary | secondary
    trigger_condition: string | null
    mapping_note: string | null
```

A compatibility view may still expose:

```yaml
recommended_strategy_node_ids: string[]
```

but the richer mapping is preferred when multiple mother strategies are common.

---

## 2. Mapping semantics

### `primary`
The procedure most often worth teaching/triggering for this TaskType when the relevant gap is methodological.

### `secondary`
A commonly composed procedure needed only for some variants or substeps.

Example:

```yaml
TaskType: 人物形象分析
recommended_strategies:
  - strategy_node_id: CN-S-special-context-key-choice-significance
    role: primary
  - strategy_node_id: CN-S-evidence-explanation-conclusion
    role: secondary
```

The concrete Question may target either/both/none depending on its actual training purpose.

---

## 3. Strategy is not a hard prerequisite

TaskType mapping means:

> This strategy is often an efficient reusable way to solve this kind of task.

It does **not** mean:

> The learner cannot solve the task without this exact named strategy.

Hard semantic prerequisites remain B2 `requires` edges between LearningNodes.

---

## 4. Strategy-target evidence rule

A Question belonging to a TaskType that recommends Strategy S does not automatically generate evidence for S.

To observe Strategy Mastery/Automation, C2 must show that the learner actually:

- selected/invoked the procedure;
- executed the relevant steps;
- did so with the recorded support level.

A correct answer produced by another valid method may support Ability mastery while Strategy S remains `not_observed`.

---

## 5. Mapping-count discipline

Normal high-frequency TaskTypes should usually have:

```text
1 primary Strategy
0–2 secondary Strategies
```

If a routine TaskType is mapped to four or more mother strategies by default, review the decomposition. The mapping is probably:

- too broad;
- mixing multiple TaskTypes;
- or treating atomic Abilities as Strategies.

---

## 6. Output skeleton remains separate

`output_skeleton` answers:

> How should a correct result be minimally organized in exam prose?

Strategy answers:

> What reasoning procedure should the learner execute to reach that result?

Do not merge them.

Example:

```text
Strategy: 证据→解释→结论
Output skeleton: “从……可以看出……，因为……，表现了……”
```

The second can help normalization after reasoning exists, but it is not the Strategy node.

---

## 7. G1 invariants

1. TaskType may recommend Strategy; it does not own a unique Strategy.
2. Mapping is advisory, not `requires`.
3. Strategy evidence requires observed invocation/execution.
4. Output skeleton and reasoning strategy remain distinct.
5. Default mapping stays small enough to be teachable and auditable.