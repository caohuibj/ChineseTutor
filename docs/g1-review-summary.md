# G1 semantic self-review summary

Status: **self-review complete**

Scope: Strategy boundaries, mother-strategy count, TaskType mapping, profile/automation semantics, and cross-domain reuse.

The review asked a strict question:

> Are we actually reducing procedural complexity, or merely renaming dozens of TaskType templates as Strategies?

---

## 1. Risk: one TaskType could silently recreate one Strategy

### Problem
The existing 题型地图 contains many useful 3–5 step algorithms. Mechanical migration would produce 30+ Strategy nodes.

### Correction
G1 uses a 13-strategy mother catalog and maps TaskTypes onto it. Task-specific differences remain in recognition signals, Knowledge/Ability mappings, output skeletons and boundaries.

---

## 2. Risk: overcompression into one generic algorithm

### Problem
If every task used one generic “找证据→答题” method, real differences between comparison, information compression, argumentation, translation and writing construction would disappear.

### Correction
The catalog preserves procedures with distinct failure/intervention patterns, while still consolidating surface variants.

---

## 3. Risk: Strategy could duplicate an Ability

Several candidate strategies sit near existing Abilities.

Examples:

```text
Ability: 根据证据判断人物特征
Strategy: 特殊情境—关键选择—意义链

Ability: 解释诗歌意象与情感关系
Strategy: 景事人—关键词—情绪方向—对象—验证

Ability: 解析写作任务限制
Strategy: 题目限制—核心任务—候选答案—校验

Ability: 最小修改句子
Strategy: 定位问题—保留原意—最小修改—复核
```

### Correction
G1 keeps the distinction:

- Ability = observable operation/outcome;
- Strategy = ordered procedure coordinating several operations and decisions.

Strategy evidence requires observable invocation/execution, not only the final Ability result.

---

## 4. Risk: output templates could masquerade as reasoning methods

### Problem
Exam sentence stems can look procedural but may let learners produce fluent answers without the reasoning being present.

### Correction
Output skeleton remains on TaskType / normalization layer. It is explicitly excluded from Strategy identity.

---

## 5. Risk: recommended Strategy could become a hard prerequisite

### Problem
A TaskType often has a preferred method, but learners can sometimes reach a valid answer through another route.

### Correction
TaskType→Strategy mapping is advisory. It must not be converted into B2 `requires`.

A correct Question may support Ability evidence while the recommended Strategy remains `not_observed`.

---

## 6. Risk: Strategy automation could be confused with Strategy mastery

### Problem
The student may execute a method perfectly after hearing its name, but fail to recognize when to use it independently.

### Correction
G1 explicitly connects Strategy to C1:

```text
Mastery = can execute correctly
Automation = can self-trigger/use with low friction
```

H2 method reminders may support mastery but normally cannot prove A2/A3.

---

## 7. Risk: comparison Strategy could degrade into “A写一点+B写一点”

### Correction
S2 makes **shared comparison dimension** the first step. Without a common dimension, the Strategy is considered failed even if both texts are mentioned.

---

## 8. Risk: S5 argumentation and S1 evidence-warrant could be duplicates

### Review result
They overlap but are not identical.

- S1 is a general local justification chain: evidence → warrant → conclusion.
- S5 constructs an argumentative unit: claim → reason → evidence → warrant → qualification/boundary.

S1 can appear as a sub-operation inside S5, but S5 has a distinct planning/failure pattern and remains separate.

---

## 9. Risk: S13 relation-chain and S1 could be duplicates

### Review result
They remain distinct.

- S13 models causal/conditional/purpose relations among facts/events.
- S1 explains why evidence supports a judgment.

A reason-analysis task may use S13; an evaluation of whether those reasons prove a conclusion may additionally use S1/S5.

---

## 10. Risk: forcing a mother Strategy onto every TaskType

### Problem
Some tasks are primarily Knowledge + atomic Ability and do not benefit from a reusable multi-step method.

### Correction
The mapping deliberately leaves examples such as:

- 文言实词语境义;
- 虚词/句式判断;
- currently, 文言断句

without a default mother Strategy.

The first draft incorrectly mapped 文言断句 to S13 solely because syntax has relations. That mapping was removed: a dedicated Strategy should be added only if a stable “句法—语义分块” procedure later proves necessary.

---

## 11. Risk: domain label could be mistaken for application boundary

### Correction
Strategy `domain` remains coverage/navigation only. A Strategy may transfer across domains when its procedure still applies.

Example: S8 is primarily a narrative-reading Strategy but can support writing material selection; this does not require cloning it as a writing Strategy.

---

## 12. Risk: a routine TaskType could accumulate many Strategies

### Correction
Default mapping discipline is:

```text
~1 primary Strategy
0–2 secondary Strategies
```

More than that triggers decomposition review. Complex Questions may compose more procedures, but the TaskType default should remain teachable.

---

## 13. Risk: strategy transfer could mean rigid template transfer

### Correction
M3-like Strategy transfer requires **flexible adaptation**. Forcing irrelevant steps into a new TaskType is negative evidence, not successful transfer.

---

## 14. Risk: duplicate Strategy fixtures could diverge

B1 already included example fixtures for S1, S7 and S8. G1's canonical catalog fixture uses the same stable IDs and compatible semantics. Future graph-integrity checks should treat B1 examples as mirrors/examples, not separate canonical identities.

G1 seed nodes now include all required B1 LearningNode fields rather than partial pseudo-records.

---

## 15. Risk: fake TaskType IDs during migration

The repository currently defines the TaskType schema but does not yet maintain a complete canonical TaskType registry with stable IDs for all existing Notion rows.

### Correction
G1 maps the current high-frequency TaskType **semantics/names** and does not invent IDs. During D1/Notion backfill, the name-level mapping is converted to stable TaskType IDs.

This is preferable to fabricating canonical identities in G1.

---

# Review conclusion

The 13-strategy catalog is sufficiently small to be learnable and sufficiently differentiated to preserve real reasoning differences.

The key invariant is:

> **题型可以很多，能力/知识节点可以很多，但学生需要自动调用的核心思维程序应保持少而稳定。**

Future additions must pass the Strategy admission rule rather than being created because a new question label appears.