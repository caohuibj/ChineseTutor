# ADR-015 — Consolidate TaskType algorithms into a small mother Strategy set

Status: **proposed / G1**

## Context

ChineseTutor has many TaskTypes and will eventually cover the full Gaokao space. If every TaskType owns its own 3–5 step method, the learner accumulates dozens of superficially different templates and transfer remains weak.

At the same time, collapsing every procedure into one generic “read carefully / find evidence / answer” algorithm would destroy useful diagnostic distinctions.

## Decision

Maintain approximately **10–15 canonical mother Strategy nodes** that represent reusable procedural structures. TaskTypes map to one primary and at most a small number of secondary Strategies where appropriate.

Task-specific differences remain in:

- recognition signals;
- Knowledge prerequisites;
- Ability nodes;
- output skeletons;
- scoring/evidence standards;
- boundary conditions.

### Initial G1 catalog

1. 证据—解释—结论链
2. 比较维度—分别取证—同异—意义链
3. 内容—位置—上下文关系—作用链
4. 定位—分类—合并—压缩
5. 观点—理由—证据—论证—边界
6. 景事人—关键词—情绪方向—对象—验证
7. 文言翻译五步法
8. 特殊情境—关键选择—意义链
9. 关键词句—内容—写法—具体效果—意义
10. 题目限制—核心任务—候选答案—校验
11. 中心—典型材料—关键场景—细节—变化—点题
12. 定位问题—保留原意—最小修改—复核
13. 现象结果—关系类型—事实链—核心原因—回扣

This is an initial canonical catalog, not an immutable final count.

## Strategy admission rule

A new Strategy is justified only when:

1. it coordinates multiple operations;
2. it recurs across multiple Questions/materials;
3. it has a distinct failure/intervention pattern;
4. existing mother strategies or their composition cannot express it without diagnostic loss;
5. its execution and automation can be observed separately.

## TaskType mapping

TaskType→Strategy is a recommendation mapping, not B2 `requires`.

A learner may solve a Question correctly through another valid method. In that case underlying Ability may be positive while the recommended Strategy remains unobserved.

## Profile semantics

Strategy nodes use the same C1 axes:

- Mastery: can the procedure be executed correctly?
- Automation: is it self-triggered when needed?
- Complexity: under what demand does it remain valid?

A common state is:

```text
M2 / A1
```

meaning correct once reminded but not self-invoked.

## F2 semantics

H2 is the normal level for naming/cueing a known mother Strategy. H2 success may support Strategy mastery but normally cannot prove A2/A3 self-trigger.

## Consequences

### Positive

- fewer methods to learn and recall;
- stronger cross-TaskType transfer;
- clearer Strategy automation Profile;
- F2 can fade a small stable set of method cues;
- TaskType map remains rich without becoming a template library.

### Costs

- some Questions compose two Strategies;
- editors must resist creating new Strategies for every surface form;
- mappings require semantic judgment rather than mechanical one-to-one conversion.

## Rejected alternatives

### A. One Strategy per TaskType
Rejected because it maximizes memorized templates and minimizes transfer.

### B. One universal reading algorithm
Rejected because comparison, argumentation, translation, information compression and narrative construction have materially different procedures and errors.

### C. Treat answer skeletons as Strategies
Rejected because output formatting does not guarantee the reasoning procedure occurred.

### D. Make recommended Strategy a hard prerequisite
Rejected because valid alternative solution paths can exist.

## Validation

G1 must demonstrate that high-frequency modern, classical, poetry, writing and language-use TaskTypes can be expressed with the catalog, including composition, without creating a dedicated Strategy per label.