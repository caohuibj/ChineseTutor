# QA gates — canonical LearningNode (B1)

These are normative acceptance tests for Issue #1.

## Gate 1 — Semantic type can be decided

Every candidate must answer exactly one primary question:

```text
Knowledge: what stable fact/concept/distinction is known?
Ability: what observable operation is performed?
Strategy: what reusable multi-step procedure is invoked?
```

Fail mixed rows unless explicitly split.

Representative cases:

| candidate | expected |
| --- | --- |
| 比喻 | Knowledge |
| 解释比喻在本句中的具体效果 | Ability |
| 词句→语境义→手法→效果→主旨 | Strategy |
| 标题含义与作用题 | Task Type, not LearningNode leaf |
| 文言实词 | split Knowledge + Ability |

---

## Gate 2 — Stable identity is taxonomy-independent

The same semantic node must retain its ID if:

- Notion page/database changes;
- display name changes without semantic change;
- domain/subdomain navigation is refined;
- code/storage implementation changes.

Required format:

```text
CN-<TYPE>-<SLUG>
```

Fail if ID encodes a mutable domain, grade, source or Notion ID.

---

## Gate 3 — No learner state leaks into canonical node

Reject:

```text
mastery
automation
complexity ceiling
current score
last trained
review due
needs improvement
attempt count
personal bottleneck
```

These belong to Learner Profile / Attempt layers.

---

## Gate 4 — No grade-first semantics

A node's identity/definition/parent structure must not depend on grade/year.

Fail:

```text
初二人物形象
CN-A-grade8-character-analysis
高一议论文概括能力
```

Pass:

```text
CN-A-evidence-to-character-judgment
CN-A-identify-argument-structure
```

---

## Gate 5 — Stable machine subdomain

`subdomain` must use `<DOMAIN>.<family>` machine code, e.g.:

```text
META.evidence
CLA.lexicon
WRT.material
```

Chinese prose labels may be rendered in views/docs but must not be mixed with machine codes in canonical records.

---

## Gate 6 — Domain follows semantic precedence

Check in order:

1. cross-domain reasoning → META;
2. reusable language form/system → LAN;
3. extended composition → WRT;
4. practical/audience-purpose/cross-media communication → COM;
5. literary/cultural concept → LIT;
6. representation-specific reading/decoding → MRD/CLA/POE.

Fail if the same operation is duplicated solely because one source is modern and another classical.

---

## Gate 7 — Ability is observable

Every active Ability must have evidence that can be judged from an Attempt/artifact.

Fail: `理解人物形象。`

Pass: `在陌生文本中给出事实/情境，并显性解释为何这些证据支持某项人物特征。`

---

## Gate 8 — Strategy is reusable and fadeable

A Strategy passes only if it:

1. coordinates >=2 meaningful operations;
2. applies across multiple questions/materials;
3. can be reminded at lower hint level;
4. can later be invoked without reminder;
5. is not a fixed answer sentence.

Fail: `这句话运用了……生动形象地……`

Pass: `比较维度 → 分别取证 → 同异 → 意义`

---

## Gate 9 — Parent is same-type taxonomy, not dependency

A valid parent:

- has same `node_type`;
- normally has same primary domain;
- is genuinely broader than the child.

Fail:

```text
parent(文言翻译 Ability) = 文言实词 Knowledge
```

Pass:

```text
Knowledge: 宾语前置 -> 文言倒装句知识
requires(文言翻译, 文言实词语境义)   # B2 edge, not parent
```

---

## Gate 10 — Aliases are one-to-one semantic synonyms

Pass:

```text
回忆性散文的文体特征
alias: 回忆性散文特征
```

Fail:

```text
CN-A-revise-sentence-minimally
alias: 语病与句子表达
```

because the legacy label includes multiple Knowledge/Ability semantics and must stay in migration mapping.

---

## Gate 11 — Mixed v1 labels have explicit disposition

All 42 v1 rows must appear in `graph/seed/v1-ability-map-migration.md` with one or more of:

- keep Ability;
- split K+A;
- split family;
- Task Type + underlying nodes;
- generalize/merge;
- transfer-evidence;
- performance family.

No row may silently disappear.

---

## Gate 12 — Cross-domain transfer is not blocked by duplicates

Representative case:

```text
modern character reasoning
classical character reasoning
```

Expected:

```text
CN-A-evidence-to-character-judgment  domain=META
+ source-specific decoding prerequisites
```

Fail if the same evidence→judgment operation exists as unrelated MRD/CLA nodes.

---

## Gate 13 — Granularity is useful, not maximal

Require at least one practical reason to split:

- distinct intervention;
- distinct prerequisite;
- distinct evidence;
- distinct decay/review behavior;
- distinct recommendation behavior.

Fail both extremes:

- micro-node with no changed behavior;
- giant node that hides actionable failure.

---

# Representative acceptance scenarios

## A. Modern reading: 修辞与表达效果

Expected:

```text
Knowledge: rhetorical device/construction
Ability: explain concrete local effect
Task Type may ask 赏析/表达效果
```

## B. Classical Chinese: 文言实词

Expected:

```text
Knowledge: word-sense network
Ability: choose/infer contextual sense
```

This distinguishes “does not know the word” from “knows meanings but selects incorrectly in context”.

## C. Cross-domain character reasoning

Expected one shared Ability:

```text
CN-A-evidence-to-character-judgment
primary domain=META
```

Modern/classical source decoding remains separate.

## D. Genre

Expected:

```text
Knowledge: 回忆性散文特征 -> LIT.genre
Ability: 根据现代文本证据判断文体 -> MRD.genre
```

Do not place both under one mixed row.

## E. Poetry: 意象与意境

Expected:

```text
Knowledge: conventional image association
Ability: construct current poem's atmosphere from local evidence
Ability: link image/scene to emotion when required
```

## F. Writing: 作文审题与立意

Expected decomposition:

```text
parse prompt constraints
form central question
form controlling idea
```

## G. Language use: 语病与句子表达

Expected:

```text
Knowledge: syntax/logical relation
Ability: diagnose problem
Ability: make minimal valid revision
```

---

# B1 definition of done

- [x] stable domain-independent ID convention specified;
- [x] canonical required/optional/forbidden fields specified;
- [x] Knowledge/Ability/Strategy decision rules specified;
- [x] stable domain/subdomain taxonomy specified without grade progression;
- [x] domain-assignment precedence specified;
- [x] parent vs prerequisite distinction specified;
- [x] alias rules specified;
- [x] split/merge/generalization rules specified;
- [x] all 42 v1 rows accounted for;
- [x] fixtures cover modern reading, classical Chinese, poetry, writing and language use;
- [x] cross-domain character reasoning normalized to META;
- [x] learner-state fields prohibited from LearningNode;
- [x] semantic self-review completed.

## Explicitly out of B1 scope

- live Notion migration/backfill;
- dependency-edge schema and cycle rules — B2;
- LearnerNodeState/Profile — C1;
- TrainingAttempt — C2;
- exhaustive Gaokao graph population;
- Strategy consolidation — G1.
