# Retention profiles and default verification horizons

Status: **F3 normative draft**

ChineseTutor needs operational review timing, but it must not pretend that one universal forgetting curve applies to every Chinese-language construct or every learner.

F3 therefore defines **retention families + configurable default horizons**. The values below are engineering defaults for scheduling verification, not claims of scientifically optimal spacing.

---

## 1. Retention family is about evidence decay, not school subject labels

A node is assigned a retention family according to how continued competence is best re-observed.

```text
exact_retrieval
recitation
conceptual_knowledge
reasoning_ability
strategy
writing_production
other
```

The family may cut across domains.

Examples:

- 文言实词固定义项 → exact_retrieval
- 古诗背默 → recitation
- 回忆性散文文体特征 → conceptual_knowledge
- 证据→解释→人物判断 → reasoning_ability
- 文言翻译五步法 → strategy
- 场景细节组织 / 论证段建构 → writing_production

Grade/year is irrelevant.

---

## 2. Default horizon table

The table gives a **nominal verification horizon in days** after the most recent credible axis-specific verification.

These values must be configurable globally, per learner, per node family, or per node after empirical calibration.

| retention family | weak evidence | moderate evidence | strong evidence | notes |
| --- | ---: | ---: | ---: | --- |
| `exact_retrieval` | 14 | 30 | 60 | lexical/cultural facts and exact distinctions benefit from frequent retrieval |
| `recitation` | 7 | 21 | 45 | exact production/默写 needs tighter freshness checks |
| `conceptual_knowledge` | 30 | 60 | 120 | concepts are usually less fragile than exact wording |
| `reasoning_ability` | 45 | 90 | 180 | authentic embedded use often provides valid implicit review |
| `strategy` | 30 | 60 | 120 | mastery may persist while self-trigger/automation decays faster |
| `writing_production` | 60 | 120 | 180 | review is preferably embedded in real revision/production artifacts |
| `other` | 30 | 60 | 90 | conservative default pending classification |

These horizons are deliberately broad and conservative. They are not promises that a learner “forgets at day 61”. They answer only:

> When should the system seek fresh evidence again if no qualifying evidence arrives naturally?

---

## 3. Axis-specific horizon modifiers

The same node may have different clocks for mastery, automation and complexity.

### Mastery
Use the family baseline unless stronger evidence indicates a longer calibrated horizon.

### Automation
Automation should often be checked sooner when the construct depends on fast self-trigger/retrieval.

Default multiplier:

```text
automation_horizon = 0.65 × mastery_horizon
```

Round to a practical whole-day scheduling value.

This is a policy heuristic, not a cognitive law.

For writing-production nodes where automation is not meaningfully measured by rapid recall, the automation clock may be `null` / not scheduled.

### Complexity
Complexity verification asks whether the learner can still perform at the previously verified demand band.

Default multiplier:

```text
complexity_horizon = 1.25 × mastery_horizon
```

unless an active requirement needs that band sooner.

Rationale: routine mastery may need freshness checks before expensive high-complexity re-validation.

---

## 4. Due and overdue thresholds

For a valid verification timestamp `t0` and computed horizon `H`:

```text
due_at      = t0 + H
overdue_at  = t0 + 1.75H
```

Derived status:

```text
now < due_at                  → current
due_at ≤ now < overdue_at     → due
now ≥ overdue_at              → overdue
```

`1.75` is a configurable operational threshold. It exists to distinguish “worth checking soon” from “verification is materially stale”; it is not a forgetting constant.

---

## 5. Freshness confidence

Historical evidence strength and current freshness are separate.

Suggested default interpretation relative to the axis horizon `H`:

```text
no usable verification timestamp                        → unknown
elapsed < 0.60H, no contradiction                       → high
0.60H ≤ elapsed < 1.00H, no meaningful contradiction   → medium/high depending evidence strength
1.00H ≤ elapsed < 1.75H                                 → medium
elapsed ≥ 1.75H                                         → low
```

Adjust downward when:

- evidence strength was weak;
- recent contradictions exist;
- the active requirement demands a more exact/high-pressure form than the last verification.

Adjust upward only through valid newer evidence accepted by C3; do not manually “feel confident” without evidence.

These cutoffs are policy defaults, not a psychological law.

---

## 6. Forgetting risk

`forgetting_risk` is not merely elapsed time. It combines:

- review status;
- freshness confidence;
- evidence strength;
- node retention family;
- recent meaningful use;
- recent contradictions;
- active requirement proximity;
- known stable error patterns.

Suggested ordinal policy:

### `unknown`
Use when the system lacks enough verification history to interpret recency.

### `low`
Typical when:

- review status is current;
- freshness confidence is high;
- evidence is moderate/strong;
- no recent contradiction exists.

### `medium`
Typical when one or more apply:

- approaching due horizon;
- review status is due;
- freshness confidence is medium;
- evidence was weak;
- automation has not been observed recently;
- active requirement raises freshness importance.

### `high`
Typical when one or more apply:

- review status is overdue for an important axis;
- freshness confidence is low;
- recent contradictory evidence exists;
- a previously stable hard prerequisite now shows instability;
- exact-retrieval/recitation evidence is substantially stale near an active assessment.

High forgetting risk still does **not** mean M/A/C automatically drops.

---

## 7. Evidence-strength adaptation

The horizon table already gives shorter intervals for weak evidence, but F3 applies additional semantic rules.

### Weak evidence
Examples:

- one sparse success;
- heavy scaffold;
- legacy-only backfill;
- near-identical variants only.

Treat review as **re-verification**, not routine maintenance.

Age itself is represented through freshness confidence rather than retroactively rewriting a historically strong evidence set as weak.

### Moderate evidence
Use normal family horizon.

### Strong evidence
Longer horizon is acceptable, but high-stakes ActiveRequirements may request a fresh probe earlier.

Strong evidence never means “never review again”.

---

## 8. Embedded/implicit review

A TrainingAttempt not labelled `review` may refresh the clock if it genuinely re-observes the axis and C3 accepts it as current verification evidence.

### Can refresh mastery
A recent attempt may refresh mastery when:

- the node was actually observed;
- observation is positive or sufficiently strong mixed evidence;
- independence is appropriate to the claimed mastery state;
- the task is representative enough for the node;
- no supplied answer invalidated the construct.

For M3 specifically, a familiar routine item is not enough. Refreshing an M3 claim requires meaningful unfamiliar/diverse transfer evidence consistent with the C3 M3 semantics.

### Can refresh automation
Only if the attempt actually observes self-trigger/access/low-friction execution, typically:

- H0/H1 or no method cue;
- the learner had to recognize when to invoke the construct;
- time/attention conditions are relevant when claiming A3.

### Can refresh complexity
Only if the attempt independently performs the node at a comparable or higher D2 demand than the currently verified band, with relevant complexity dimensions represented.

### Does not refresh

- passive reading;
- teacher explanation;
- seeing a model answer;
- downstream task where the node is `not_observed`;
- success only after direct supply of the target content;
- a very easy task that does not re-test the relevant complexity claim;
- a familiar routine item when the claim being refreshed is M3 transfer.

---

## 9. Family-specific review shapes

### `exact_retrieval`
Prefer brief retrieval-in-context, discrimination, or production probes. Batch multiple compatible facts efficiently.

### `recitation`
Prefer exact recall/默写 in short chunks with immediate local correction. Do not repeatedly reread as the main review mechanism.

### `conceptual_knowledge`
Prefer recognition + explanation + application/contrast rather than definition recitation only.

### `reasoning_ability`
Prefer a fresh compact authentic task that makes the reasoning operation observable. Do not manufacture review through isolated terminology questions.

### `strategy`
Prefer a task where the learner must decide to invoke and execute the strategy. If automation is due, avoid H2 naming of the strategy.

### `writing_production`
Prefer authentic micro-writing/revision artifacts tied to the specific skill. A full essay is required only when whole-text construction itself is the construct being verified.

---

## 10. Node-level overrides

A canonical node may define a retention override only when there is a stable semantic reason.

Example:

```yaml
retention_family: recitation
retention_override:
  strong_evidence_horizon_days: 30
  reason: exact high-stakes production requirement
```

Learner-specific calibration belongs to personal policy/profile configuration, not to canonical node identity.

---

## 11. F3 invariants

1. Review horizon is a scheduling heuristic, not a mastery score.
2. Time alone never downgrades M/A/C.
3. Evidence family matters more than source grade.
4. Each axis may have a different clock.
5. Evidence strength and freshness confidence remain distinct.
6. Embedded use counts only when the target construct is actually re-observed and accepted through C3.
7. Passive exposure does not reset clocks.
8. M3 retention preserves transfer requirements.
9. Strong evidence extends the interval; it does not create permanent exemption.
10. Policy values remain configurable and should be recalibrated from real learner data later.