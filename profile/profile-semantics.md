# Learner Profile semantics

Status: **C1 design draft**

The Learner Profile is the personal, changing state overlay on the canonical Learning Graph.

Its job is to answer four questions:

1. What does current evidence suggest the learner can do/know now?
2. How reliable is that judgment?
3. What is the likely current failure pattern or upstream constraint?
4. Which nodes deserve attention, without yet deciding the exact next Question?

The Profile is not a report card and not a static percentage dashboard.

---

## 1. Canonical graph vs personal state

```text
LearningNode
= stable meaning of Knowledge / Ability / Strategy

LearningEdge
= stable semantic dependency / support / transfer relation

LearnerNodeState
= current estimate for one learner on one LearningNode
```

No personal field belongs in B1/B2 canonical records.

The same node can therefore have:

```text
Learner A: M3 / A2 / C4
Learner B: M1 / A0 / C1
Learner C: unknown
```

without changing the node itself.

---

## 2. The Profile is a projection, not the evidence ledger

The current Profile is analogous to a materialized view:

```text
TrainingAttempt history
+ artifact/version evidence
+ legacy backfill evidence
+ canonical graph context
        ↓
LearnerNodeState current projection
```

C1 defines the projection shape. C2 will define TrainingAttempt as the primary event source.

Do not rely on Profile snapshots as the only history. If current M2 later becomes M1 after contradictory evidence, the underlying evidence sequence must remain inspectable.

---

## 3. State dimensions should disagree when reality disagrees

A core design goal is to preserve meaningful disagreement between dimensions.

### Example A — understanding stronger than automatic invocation

```text
人物证据推理
mastery: M2
automation: A1
verified_complexity: C2
```

Interpretation:

> The learner can independently produce the reasoning once the relevant model is activated, but does not yet reliably recognize/trigger it alone.

Training implication later:

> fade task/model reminders rather than reteach the reasoning.

### Example B — procedure automatic, execution still weak

```text
文言翻译五步法
mastery: M1
automation: A2
```

Interpretation:

> The learner quickly invokes the procedure, but underlying lexical/syntax execution is still weak.

Training implication:

> do not repeatedly re-explain the five steps; diagnose prerequisites.

### Example C — good estimate, weak confidence

```text
mastery: M2
evidence_strength: weak
state_source: legacy_backfill
```

Interpretation:

> M2 is a useful starting hypothesis, not a verified conclusion.

Training implication:

> schedule a low-cost verification/transfer probe before treating the node as stable.

---

## 4. Unknown-first policy

New graph nodes should begin as `unknown`, not deficient.

Bad system behavior:

```text
new Gaokao node created
-> default M0
-> dashboard suddenly shows a huge weakness
-> recommendation engine floods learner with untested topics
```

Correct behavior:

```text
new node
-> mastery null, automation null, complexity null
-> evidence_strength none
-> attention_status unknown
-> test only when graph importance/current training makes verification worthwhile
```

This is essential for a graph that will keep expanding toward full Gaokao coverage.

---

## 5. Current state is not monotonically increasing

A real learner can:

- forget lexical/cultural knowledge;
- lose fluency after long disuse;
- reveal that earlier success was over-scaffolded;
- fail on a genuinely unfamiliar transfer even after routine success;
- improve after a small number of high-quality interventions.

Therefore:

```text
M2 -> M1
A2 -> A1
C3 -> C2
```

must be permitted when evidence justifies it.

Downgrade is not punishment; it is a better estimate.

---

## 6. Stable does not mean “finished forever”

`stable` is an operational status relative to an active requirement.

A learner may be:

```text
stable at current target: C2 independent routine performance
```

while eventual Gaokao coverage expects:

```text
C4/C5 transfer/evaluation
```

The canonical node does not need grade-specific copies. Goal/TrainingMove layers determine the currently required evidence threshold.

Thus the Profile stores observed state; “meets current need” is derived by comparing observed state to active goal requirements.

---

## 7. Bottleneck semantics

A bottleneck is not simply the lowest mastery score.

A node becomes a bottleneck candidate when:

```text
learner weakness/uncertainty
× high downstream dependency unlock value
× evidence that downstream failures are plausibly upstream-caused
```

Example:

```text
解释证据为何支持结论
```

may be more valuable to repair than a lower-scoring narrow title-question skill because it feeds:

- character judgment;
- theme inference;
- open evaluation;
- poetry emotion explanation;
- argumentation.

C1 stores bottleneck state semantics. Exact centrality/priority formula belongs to recommendation work.

---

## 8. Review Due semantics

Review is mainly about **confidence decay**, not automatic mastery deletion.

Example:

```text
mastery: M2
last_verified_at: old
forgetting_risk: high
review_status: due
```

means:

> prior M2 evidence exists, but a quick verification is now worth more than assuming it still holds.

For memory-sensitive nodes, review may be frequent. For reasoning nodes, varied usage may maintain confidence longer.

---

## 9. Profile updates should be explainable

Every material state change should be explainable in teacher language.

Good:

> 人物形象仍为M2，但自动化从A1升到A2：最近两次陌生叙事题都没有题型/方法提醒，能主动写出“情境—选择—意义”。

Good:

> 文言实词从“中等证据”降为“弱证据”：距离上次验证较久，且最近两次陌生短篇出现语境义误判；建议先做小规模复现，不直接重学全部词义。

Bad:

> priority_score 0.734，所以推荐。

Numerical internals may exist later, but user-facing recommendation must expose causal reasons.

---

## 10. No automatic propagation through the graph

B2 edges inform diagnosis and candidate recommendations; they do not create learner evidence.

Forbidden automatic conclusions:

```text
P mastered + T requires P
=> T mastered          # false

SOURCE M3 + SOURCE transfers_to TARGET
=> TARGET M3           # false

child M2
=> parent M2            # not automatically valid

parent M2
=> every child M2       # false
```

Graph structure changes what to inspect/test next, not what the learner is declared to know.

---

## 11. Profile views to support later product UX

C1 does not implement UI, but the model should support these views without schema hacks.

### Strong / verified
Nodes with sufficient evidence and current target satisfied.

### Developing
Nodes with clear evidence gaps currently worth training.

### Bottlenecks
High-impact upstream nodes constraining multiple important targets.

### Review due
Previously supported nodes whose confidence is decaying.

### Unknown high-value
Important graph nodes with little/no evidence that deserve efficient diagnostic probes.

### Recent changes
State transitions such as:

```text
M1 -> M2
A1 -> A2
evidence weak -> moderate
review_due -> current
```

These changes should eventually be reconstructed from Attempt/profile events, not manually narrated.

---

## 12. Minimum-effective-intervention connection

Profile data changes tutoring behavior during a Question.

If Profile says:

```text
text location weak
reasoning unknown
```

tutor repairs location first.

If Profile says:

```text
reasoning strong
written expression repeatedly weak
```

tutor skips re-explaining comprehension and asks for one explicit inferential sentence.

If Profile says:

```text
mastery M2
automation A2
```

tutor should normally remove algorithm prompts and raise authenticity/complexity.

This connection is why Profile must preserve dimensions instead of collapsing everything to one percentage.
