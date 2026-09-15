# F1 explainable priority policy

Status: **normative draft — semantic self-review applied**

ChineseTutor must choose among many plausible TrainingMoves without hiding the decision inside one opaque score.

The policy therefore uses **eligibility gates + priority classes + ordered semantic tie-breakers**.

---

## 1. Separate three questions

For every candidate move, decide separately:

1. **Need** — would this learner benefit from the move?
2. **Leverage** — does fixing/verifying this node unlock important downstream performance?
3. **Executability** — is there an appropriate task/question/material now?

Do not collapse these into one field.

---

## 2. Eligibility gates

A candidate is not executable now when:

- a demonstrated hard prerequisite blocks valid observation of the target and the move requires independent downstream performance;
- suitable Question supply is `none` and no legitimate generated/editorial probe is allowed;
- the target node is deprecated/invalid;
- the move would repeat a recently exhausted near-identical variant when diversity is required;
- the requested transfer probe is not actually unfamiliar enough for this learner;
- the move demands C/A conditions substantially above current evidence without pedagogical justification.

Blocked target need remains visible; do not erase it.

---

## 3. Priority classes

### P0_blocking

Use when an actionable node is a demonstrated blocker for important downstream targets.

Typical cases:

- hard prerequisite deficit invalidates several current high-value tasks;
- confirmed graph bottleneck repeatedly causes downstream failure;
- severe Q/K/R issue prevents observing otherwise important abilities;
- a previously established prerequisite has demonstrably regressed and is now blocking an active core target.

P0 does **not** mean “lowest mastery number”.

### P1_high

High expected learning value without being a strict blocker.

Typical cases:

- core Gaokao node with meaningful mastery/automation/transfer gap;
- high-centrality supporting node with strong unlock value;
- high-impact uncertainty where one cheap diagnosis would change the training path;
- recently observed stable error pattern affecting multiple tasks;
- M2 node lacking transfer evidence when transfer is central to the target capability;
- overdue/high-forgetting **core** knowledge whose loss would soon threaten active performance;
- explicit near-term school requirement when missing it has a real deadline and the move still trains a canonical high-value node.

### P2_normal

Valuable planned development.

Typical cases:

- routine M1→M2 independent practice;
- bounded complexity extension;
- supporting Knowledge/Ability development;
- current school-linked practice with real canonical value but no urgent bottleneck.

### P3_maintenance

Keep established capability available efficiently.

Typical cases:

- review due with otherwise stable state;
- automation polishing where no larger bottleneck exists;
- spaced re-verification of memorization/lexical/cultural nodes.

Maintenance can escalate to P1/P0 when overdue evidence or demonstrated regression materially threatens core downstream performance. This prevents “maintenance starvation”.

### P4_defer

Valid need, but lower current marginal value or not executable.

Typical cases:

- enrichment while core bottleneck remains;
- no suitable Question supply;
- duplicate practice after evidence purpose is already satisfied;
- high complexity extension before routine mastery is established;
- unknown low-impact node with no active relevance;
- a recently saturated node where the next same-form item has little new evidence value.

---

## 4. Tie-break order within a priority class

When several executable candidates share the same class, compare in this order.

### 4.1 Causal leverage / dependency unlock
Prefer a node whose repair/verification unlocks more important downstream targets **when the causal relation is plausible and relevant now**.

Do not mechanically rank by raw graph degree. A node with many irrelevant edges should not dominate.

### 4.2 Gaokao relevance
Prefer `core` over `supporting` over `enrichment`, all else comparable.

This is coverage importance, not grade progression.

### 4.3 Learner gap severity
Prefer a meaningful demonstrated gap over a minor polish need.

Interpret gap by axis:

- mastery gap;
- automation gap;
- complexity gap;
- transfer gap;
- stable error pattern.

Do not convert null/unknown to “maximum gap”.

### 4.4 Information value / uncertainty reduction
A cheap diagnostic can outrank more practice if it resolves a decision-critical uncertainty.

Example:

```text
translation failure
word-sense prerequisite unknown
```

One lexical probe may have more value than another full translation item.

### 4.5 Forgetting/review urgency
When prior mastery is established but evidence is stale, prefer a cheap re-verification before spending time on lower-value new work.

If review is overdue on a core prerequisite or recent evidence suggests actual regression, its priority class may escalate before tie-breaking.

### 4.6 Transfer deficit
For nodes already M2 with adequate routine confidence, a meaningful transfer probe outranks additional same-form routine repetition.

### 4.7 Recent learning momentum / scaffold-fade opportunity
After a successful repair, a nearby fade-scaffold move often has high marginal value because the just-built reasoning path is active.

Avoid waiting so long that every session restarts from H3/H4.

### 4.8 Recent saturation / diminishing evidence value
Prefer a different high-value node or a different evidence condition when the current node has just produced enough same-purpose evidence.

Examples:

- three recent routine successes already establish M2 confidence;
- another same-variant item adds little transfer information;
- a just-completed diagnostic should not be repeated unless uncertainty remains.

This is not a forced “variety” rule; it prevents low-information overpractice.

### 4.9 School-material relevance
Use as a tie-breaker when canonical learning value is similar.

Exception: an **explicit ActiveRequirement with a near-term school deadline** may raise priority because the requirement itself is urgent. The urgency comes from the declared requirement, not from grade/unit identity.

### 4.10 Question quality and availability
Prefer high-fidelity, traceable authentic Questions when evidence purpose is transfer/validation. For narrow diagnosis, a short teacher/tutor-authored probe may be more efficient.

### 4.11 Cost / fatigue
When learning value is similar, prefer the move that obtains required evidence with lower unnecessary load.

This is particularly important for long-form writing: use micro-writing/targeted revision when a full essay is not needed.

---

## 5. Why no single priority formula

A formula such as:

```text
0.3*weakness + 0.2*importance + ...
```

would create false precision before we have population-level calibration and would make recommendations hard to audit.

F1 therefore uses semantic classes and ordered factors.

Internal implementations may later use numeric features for efficiency, but the final decision must be explainable in the policy vocabulary and pass the same gates.

---

## 6. Candidate examples

### Example A — isolated low score vs central prerequisite

```text
标题作用: M0, isolated current gap
证据→解释推理: M1, used by character/theme/open evaluation/writing argument
```

If both are otherwise relevant, the shared evidence-reasoning bottleneck can outrank the lower-scoring title task because it unlocks multiple important targets.

### Example B — unknown is not automatically urgent

```text
high-school literary-history enrichment node: unknown
current evidence-to-character Ability: M1/A1
```

The unknown enrichment node does not outrank the active core reasoning gap.

### Example C — transfer outranks more routine repetition

```text
character reasoning: M2 strong on familiar routine texts
transfer evidence: none
```

Recommend unfamiliar transfer probe, not the sixth similar familiar character question.

### Example D — review is cheap

```text
classical lexical knowledge: prior M2, stale, forgetting high
```

A 2-minute retrieval probe can outrank a low-impact new supporting node; if passed, stop.

### Example E — school relevance tie-break

Two P2 moves have similar leverage. One can be trained inside the text currently taught at school. Prefer that move because it gives immediate curricular integration without redefining the graph.

### Example F — school deadline without grade staging

A near-term school assessment explicitly requires a canonical Ability already on the long-term graph. That ActiveRequirement may temporarily raise the move to P1. The reason is the real deadline + canonical value, not “because this is an eighth-grade ability”.

### Example G — maintenance escalation

A core classical lexical prerequisite was previously M2 but is overdue and now fails two recent routine probes, blocking translation. The move becomes P0/P1 repair, not permanent P3 maintenance.

---

## 7. Anti-thrashing rule

Do not switch the recommended primary move after every minor event.

Keep the current move while:

- its success criterion is still meaningfully unresolved;
- no new blocker appears;
- no C3 state/confidence change materially alters the decision;
- the move is not saturated or invalidated by new evidence.

Recompute when the evidence purpose is satisfied, contradicted, blocked, or superseded by a materially higher-value need.

This gives the learner enough continuity to complete a learning cycle without turning recommendation into noise.

---

## 8. Priority explanation template

Every selected move should be explainable with four clauses:

```text
为什么现在：当前证据显示什么缺口/不确定性
为什么它重要：高考相关性/依赖解锁/稳定错误/明确期限
为什么这样练：选择此 move type、复杂度、提示上限的原因
如何判断完成：明确 success criterion
```

Example:

> 当前“人物证据→品质判断”已能在 H3 下完成，但自动化仅 A1；该能力是人物、主旨和开放评价的公共推理前置。下一步不加难度，继续 C2 陌生材料，把提示上限降到 H1，若两个不同材料都能独立补出解释链，则本次淡化支架训练完成。

---

## 9. Priority invariants

1. Low mastery alone does not define highest priority.
2. Unknown alone does not define highest priority.
3. Raw graph degree alone does not define unlock value.
4. Hard prerequisite deficit may create P0; `supports` weakness cannot automatically do so.
5. High-value diagnosis can outrank additional practice.
6. Transfer need rises only after routine mastery is sufficiently established.
7. Review due does not imply full reteaching; overdue/regressed core maintenance may escalate.
8. School relevance is normally a tie-breaker; explicit deadline requirements may temporarily raise priority without creating grade progression.
9. Question availability affects executability, not learner need.
10. Recent saturation can lower marginal priority even when a node remains globally important.
11. Recommendation should remain stable until meaningful evidence changes the decision.
12. Every selected priority class must have a human-readable causal rationale.