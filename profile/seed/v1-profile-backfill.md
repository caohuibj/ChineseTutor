# v1 learner-state → v2 LearnerNodeState backfill plan

Status: **C1 non-destructive migration design**

The current Notion ability map stores personal state next to canonical concept definitions. C1 separates those fields without pretending that every v1 row maps one-to-one to a v2 node.

---

## 1. v1 fields and v2 destination

| v1 field | v2 destination | rule |
| --- | --- | --- |
| `当前掌握度` | `LearnerNodeState.mastery` | direct only when v1 row maps one-to-one to a canonical node; otherwise use split/merge rules below |
| `自动化度` | `LearnerNodeState.automation` | same rule as mastery |
| `最近训练` | `last_trained_at` | never automatically treated as `last_verified_at` |
| `待加强` | `attention_reason` legacy note | does not automatically mean `bottleneck` |
| `阶段定位` | no direct LearnerNodeState field | legacy curriculum/planning metadata; grade/stage is not mastery |
| `初二达标标准` | not personal state | useful content may become canonical observable-success example / load guidance / active goal outside Profile |
| `真实题型训练场` | Task graph relation | not learner state |
| `高考终局能力(L1)` | canonical graph/coverage | not learner state |
| `稳定模块(L2)` | canonical taxonomy/reporting | not learner state |

---

## 2. Direct one-to-one backfill

When a v1 row maps cleanly to one canonical v2 node, e.g. a stable Ability concept retained semantically, backfill may initialize:

```yaml
mastery: mapped v1 value
automation: mapped v1 value
last_trained_at: v1 最近训练
last_verified_at: null
evidence_strength: weak
state_source: legacy_backfill
attention_reason: ["legacy v1 state; requires attempt-based verification"]
```

Why evidence defaults to `weak`:

- current v1 state was not computed from the future structured Attempt model;
- evidence may be valid pedagogically but lacks normalized diversity/hint/complexity metadata;
- C1 must preserve useful history without overstating confidence.

A reviewer may upgrade selected legacy states to `moderate` only when explicit historical records already contain strong independent/transfer evidence.

---

## 3. Split-node rule — never copy mastery blindly

Many v1 rows split semantically.

Example:

```text
修辞与表达效果
  -> Knowledge: 修辞知识
  -> Ability: 解释具体语境中的修辞效果
```

If the old row says:

```text
掌握度 = 2
```

it is invalid to set both new children to M2 automatically.

The learner may know the device name but fail contextual explanation, or the reverse.

Backfill policy:

```text
v1 mixed row
-> preserve legacy state in migration reference
-> create child LearnerNodeState as unknown unless historical evidence distinguishes them
-> add attention_reason: "split from v1 <label>; needs targeted verification"
```

If historical work clearly distinguishes a child, that child may receive a low-confidence backfill with evidence reference.

Representative split rows:

- 修辞与表达效果;
- 文言实词;
- 文言虚词;
- 古今异义与一词多义;
- 词类活用与特殊句式;
- 意象与意境;
- 诗词修辞/表现手法;
- 作文审题与立意;
- 作文语言与修改;
- 字音字形与书写;
- 语病与句子表达.

---

## 4. Task-Type-only rule

If a v1 label becomes primarily a Task Type rather than a canonical LearningNode, do **not** create a LearnerNodeState for the task label.

Example:

```text
标题含义与作用
词句赏析
炼字
```

Their historical performance should later be mapped through Questions/Attempts to the actual underlying canonical nodes.

Until that mapping is available, preserve the legacy value only in migration documentation; do not invent a pseudo Ability to hold it.

---

## 5. Cross-domain merge/generalization rule

Example:

```text
现代人物形象分析
文言人物形象
  -> shared CN-A-evidence-to-character-judgment
```

Historical success in one domain is valid evidence for the shared operation, but not automatically evidence of broad transfer.

Backfill policy:

1. collect all legacy source rows that map to the shared node;
2. initialize the current estimate conservatively;
3. cap legacy-only mastery at `M2` unless there is explicit unfamiliar cross-domain transfer evidence;
4. set `evidence_strength` according to source diversity, normally `weak` or `moderate`;
5. never infer `M3` solely from two familiar domain labels existing in v1.

If v1 values conflict, preserve the lower/uncertain estimate or leave mastery unknown pending targeted verification; do not average scores mechanically.

---

## 6. Composite reporting node rule

Some broad v1 abilities may remain useful as reporting composites while canonical leaves provide diagnosis.

If the composite remains a canonical Ability, its state must not be populated by simply averaging child mastery.

Possible later derivation can use explicit aggregate rules, but C1 backfill should either:

- preserve a clearly one-to-one legacy composite estimate as `legacy_backfill`; or
- leave it unknown until C2/F1 defines evidence aggregation.

No parent/child mastery propagation is implicit.

---

## 7. `最近训练` is not verification

A training session can include:

- heavy hints;
- exposure only;
- failed attempts;
- explanation without independent second attempt.

Therefore:

```text
v1 最近训练 -> last_trained_at
```

but:

```text
last_verified_at = null
```

unless the learning record explicitly supports independent verification.

This avoids false recency confidence.

---

## 8. `待加强` is not Bottleneck

Legacy `待加强 = true` means the teacher/system considered the node worth improvement.

It does not prove:

- high downstream unlock value;
- causal impact on other failures;
- confirmed bottleneck status.

Migration:

```yaml
attention_reason:
  - "legacy v1: 待加强"
bottleneck_status: unknown
```

Bottleneck determination belongs to graph + evidence analysis.

---

## 9. `阶段定位` is not personal ability

Values such as:

```text
当前重点
当前常规
埋种子
高中展开
```

reflect historical planning, not the learner's actual mastery.

Do not map them into:

- mastery;
- automation;
- complexity ceiling;
- readiness;
- prerequisite status.

They may remain as legacy planning metadata until TrainingMove/goal configuration replaces them.

---

## 10. Initial seed examples from current project

### Typical-event / character reasoning

Current evidence from 《周亚夫军细柳》 shows the learner could, with prompting, articulate:

```text
ordinary battle result != distinctive character evidence
special emperor-inspection context
-> enforcing military discipline even toward emperor
-> choice becomes discriminative evidence of character
```

For a shared character-evidence Ability this supports a useful legacy starting estimate, but because evidence is narrow and scaffolded it should not become strong M2/M3 certainty automatically.

Recommended C1 backfill style:

```yaml
mastery: M1
automation: A1
verified_complexity_band: C2
evidence_strength: weak
state_source: legacy_backfill
last_trained_at: 2026-09-14
last_verified_at: null
primary_error_pattern: E
attention_reason:
  - "evidence/reasoning direction established; written inferential link still needs stabilization"
```

Exact values remain subject to historical-record review during live migration; this file defines the migration logic, not a production write.

### 对比衬托

The learner recognized the contrast between 霸上/棘门 and 细柳营 and connected it to highlighting 周亚夫. Because automation still required explicit method framing, a plausible legacy hypothesis is:

```text
mastery around M1-M2
automation A1
weak evidence
```

Do not force a precise state until the Attempt model captures a comparable unfamiliar task.

---

## 11. Migration procedure

```text
1. export/inspect v1 rows and migration dispositions from B1
2. classify mapping: one-to-one / split / task-only / merge-generalize / composite
3. create LearnerNodeState only where canonical target exists
4. apply conservative legacy backfill rules
5. mark state_source = legacy_backfill
6. keep evidence_strength weak unless explicit records justify more
7. do not set last_verified_at merely from last_trained
8. create a targeted verification queue for high-value unknown/low-confidence nodes
9. after C2, let real Attempt evidence progressively replace legacy uncertainty
```

Migration is additive and non-destructive until the new profile is validated.
