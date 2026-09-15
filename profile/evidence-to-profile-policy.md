# C3 — Evidence → Learner Profile update policy

Status: **normative draft**

This policy defines how C2 `TrainingAttempt` evidence contributes to C1 `LearnerNodeState`.

It is deliberately conservative. The goal is not to maximize the number of state changes; the goal is to make every change semantically defensible, explainable, and reversible when later evidence contradicts it.

---

# 1. Update pipeline

For one learner and one canonical node:

```text
1. collect relevant Attempt.node_evidence
2. validate observation causality
3. classify evidence quality
4. separate M / A / C contributions
5. check diversity, recency, scaffold and contradiction
6. decide initialize / hold / upgrade / downgrade / verify-only
7. update confidence + verification timestamps
8. update error-pattern summary if warranted
9. emit ProfileUpdateDecision
10. project result into LearnerNodeState
```

Do not jump directly from `answer_correctness` to mastery.

---

# 2. Evidence unit: node observation, not whole Question correctness

The atomic update input is:

```text
TrainingAttempt.node_evidence[node_id]
```

combined with the Attempt context.

A Question may involve several nodes. Example:

```text
classical word-sense = negative
translation strategy = positive
translation ability = not_observed
```

If lexical failure prevents a meaningful translation attempt, downstream translation Ability must not receive negative evidence merely because the final answer is wrong.

`not_observed` contributes no positive or negative mastery evidence.

---

# 3. Evidence validity before strength

Before asking how strong evidence is, ask whether it is valid for this node.

Discount or exclude evidence when:

- target was `not_observed`;
- upstream prerequisite failure blocked valid observation;
- the intervention supplied the exact operation being judged;
- assessment confidence is too low;
- artifact/modality does not expose the construct being claimed;
- the Attempt is a legacy reconstruction whose details are unknown;
- multiple items are trivial variants and are being counted as independent diversity.

Evidence may remain useful for diagnosis even if it is too weak to move mastery.

---

# 4. Independence is node-specific

Global H0-H7 is necessary but insufficient.

Example:

```text
H4 supplies the paragraph location
```

This may invalidate independent evidence for `text_location`, while still leaving later `reasoning` independently observable.

Therefore use C2:

```text
node_evidence.independence = unknown | guided | partial | independent
```

as the primary independence signal, cross-checked against hint/intervention type.

Guardrails:

- H6/H7 normally prevents independent mastery evidence for any node whose reasoning/answer was supplied;
- H5 normally prevents independent evidence-selection evidence if critical evidence was supplied;
- H4 normally prevents independent location evidence;
- H2/H3 may still allow mastery evidence for execution quality while weakening automation evidence;
- H0/H1 is strongest for self-trigger / automation when the target operation was genuinely recognized without method prompting.

---

# 5. Evidence contribution classes

For update reasoning, classify valid node observations into interpretable contribution classes.

## 5.1 `guided_success`

Typical shape:

- positive/mixed outcome;
- `guided` or `partial` independence;
- meaningful tutor support;
- target operation still partly executed by learner.

Primary contribution:

- supports M1 / learning progress;
- can increase weak mastery confidence;
- does not by itself support A2/A3;
- can identify a successful intervention.

## 5.2 `independent_success`

Typical shape:

- positive node observation;
- `independent`;
- no support that supplied the target operation;
- representative task context.

Primary contribution:

- supports M2;
- may support A2 if trigger/invocation was also independent;
- can verify complexity at the attempted band.

## 5.3 `transfer_success`

Requires all of:

- valid independent positive observation;
- learner-relative unfamiliar material/task context;
- intentional or defensible transfer probe;
- meaningful novelty beyond trivial variant rewrite;
- target semantics remain comparable.

Primary contribution:

- supports M3 generalization;
- strengthens transfer evidence;
- does **not** require the Question itself to be C4/C5.

## 5.4 `automation_success`

Requires evidence about trigger/access fluency, not only answer quality.

Typical shape:

- H0/H1 or otherwise self-triggered target operation;
- normal or timed conditions appropriate to claimed level;
- low-friction execution;
- quality remains adequate.

Primary contribution:

- supports A2/A3.

## 5.5 `negative_valid_observation`

A valid negative observation means the target construct was genuinely exposed and failed.

It can:

- initialize/confirm M0 or A0;
- reduce confidence;
- support downgrade after repeated comparable contradictions;
- identify an error pattern.

It must not be manufactured from downstream failure when the node was not actually observable.

---

# 6. Mastery update semantics

Mastery is a current estimate of quality + independence + generalization.

## 6.1 Unknown → M0

Do **not** set M0 because a node has never been tested.

M0 requires valid evidence that the construct is not established.

Strong cases include:

- direct diagnostic task exposes the target and learner cannot perform even under ordinary cueing; or
- repeated valid negative observations across more than one non-trivial context.

A single failure caused by misunderstood instructions, missing prerequisite, fatigue, or excessive task complexity should usually keep mastery unknown and create a diagnostic need instead.

## 6.2 Unknown/M0 → M1

M1 is appropriate when evidence shows the learner can succeed with meaningful scaffold or in tightly familiar/constrained conditions.

Typical evidence:

- one or more valid guided successes;
- repair after H2-H4 where the learner still performs the key target operation;
- successful execution in a familiar condition without enough independent evidence for M2.

H6/H7 near-answer success normally cannot establish M1 for the reasoning node whose key reasoning was supplied, though it may show learning of a narrower sub-operation.

## 6.3 M1 → M2

M2 requires independent success on representative tasks.

Minimum semantic topology:

- more than one valid independent success;
- not all from the same near-identical variant group;
- representative coverage of the node's observable-success semantics;
- no unresolved recent contradiction of comparable quality.

This is a **minimum topology**, not an attempt-count formula. Two trivial items are not sufficient merely because the number is two.

Hints that only activate a separate Strategy may still permit M2 for execution quality if node-specific independence remains `independent`; they usually weaken automation evidence instead.

## 6.4 M2 → M3

M3 means stable generalization, not “very good at routine tasks”.

Require:

1. M2-quality independent performance is already established;
2. at least two meaningfully distinct unfamiliar transfer contexts or equivalent strong diversity evidence;
3. target operation remains semantically the same while material/task surface changes;
4. success is not dependent on meaningful strategy/reasoning prompts;
5. no strong contradictory transfer evidence remains unresolved.

At least two distinct transfer contexts is a guardrail against one lucky item; it is necessary but not automatically sufficient.

Repeated familiar high-complexity success can raise complexity confidence without producing M3.

---

# 7. Automation update semantics

Automation is independent from mastery.

## 7.1 Unknown/A0 → A1

A1 is supported when an explicit cue reliably activates retrieval/operation/strategy.

Pattern:

```text
without cue -> not invoked
with task/strategy reminder -> successfully invoked
```

This can coexist with M2.

## 7.2 A1 → A2

A2 requires self-recognition/self-trigger in normal conditions.

Evidence should show:

- target operation is invoked without meaningful method prompt;
- H0/H1 or equivalent low support;
- repeated across more than one representative context;
- execution quality is at least adequate.

If the learner always needs “这是什么题型/用哪个模型”的 reminder, remain A1 even if final reasoning is M2 quality.

## 7.3 A2 → A3

A3 requires robust low-friction access under pressure.

Evidence should include:

- self-triggered H0/H1 performance;
- timed/attention-constrained contexts when speed is part of the claim;
- stable quality rather than fast-but-sloppy output;
- repeated evidence, not one unusually fast item.

Untimed work cannot by itself verify A3.

---

# 8. Verified complexity update semantics

`verified_complexity_band` is the highest intrinsic D2 complexity band at which the learner has sufficiently credible evidence for the node.

Rules:

1. attempted maximum is not verified maximum;
2. success under heavy scaffold may train at C3 but only verify the target at C1/C2 depending on what remained independent;
3. unfamiliarity does not alter the C-band;
4. a C4 failure does not erase established C2 success;
5. complexity may downgrade only when recent evidence shows the learner no longer performs reliably at the previously verified band under comparable conditions.

Verification requires node-specific positive evidence at that band and appropriate independence for the claimed mastery level.

---

# 9. Confidence updates can occur without state changes

A major C3 principle:

```text
hold state + increase confidence
```

is often the correct result.

Examples:

- M2 learner produces another independent success on a distinct routine context → keep M2, raise mastery evidence strength;
- A1 learner again needs one explicit method reminder → keep A1, raise confidence;
- C2 verified learner succeeds on a second distinct C2 item → keep C2, strengthen complexity evidence.

Likewise:

```text
hold state + decrease confidence
```

may be correct after one meaningful contradiction that is not yet sufficient for downgrade.

---

# 10. Contradiction and downgrade policy

Profile is not monotonic.

But downgrades must be conservative because one bad item may reflect noise, task mismatch, or a higher complexity ceiling rather than loss of the underlying ability.

## 10.1 First meaningful contradiction

Normally:

- keep current M/A/C;
- reduce relevant evidence confidence or mark contradiction;
- schedule verification if high-impact.

## 10.2 Repeated comparable contradiction

Downgrade becomes appropriate when recent valid negative/mixed evidence:

- directly exposes the same node;
- occurs at the same or lower complexity than previously verified;
- is not explained by prerequisite failure or modality mismatch;
- appears across more than one non-trivial context;
- outweighs stale/weak historical evidence semantically.

## 10.3 High-complexity failure

Example:

```text
Profile: M2 / verified C2
Attempt: C4, unfamiliar, H0, failure
```

Default effect:

```text
M2 stays
C2 stays
record transfer/high-complexity gap
possibly lower M3 hypothesis / create training need
```

Do not downgrade M2 simply because C4 exceeded the current ceiling.

---

# 11. Recency and evidence provenance

No fixed decay equation is defined in C3, but ordering principles are normative.

When evidence conflicts, prefer evidence that is:

1. valid for the node;
2. native C2 telemetry over vague legacy reconstruction;
3. higher assessment confidence;
4. more independent;
5. more representative/diverse;
6. more recent when quality is otherwise comparable.

Staleness alone normally lowers confidence / triggers review before it lowers mastery.

Legacy-only evidence cannot establish:

- strong confidence;
- A3;
- M3;
- verified high-complexity transfer claims.

---

# 12. Variant and diversity handling

Evidence diversity matters at two levels.

### Surface diversity
Different texts/questions.

### Semantic diversity
Different contexts that still expose the same canonical operation.

Five superficial rewrites in one `variant_group_id` should behave closer to one evidence cluster than five independent demonstrations.

M3 requires semantic diversity, not merely item count.

---

# 13. Error-pattern promotion

Attempt errors remain event-level by default.

Profile `primary_error_pattern` should progress roughly as:

```text
single occurrence -> candidate only
repeated same causal error in distinct contexts -> moderate stable pattern
persistent pattern despite task variation / intervention -> strong pattern
```

Promote only when the error is causally meaningful.

Example:

```text
I + E on one answer
```

should not become a stable Profile pattern immediately.

But repeated:

```text
correct evidence + correct conclusion + missing warrant/explanation
```

across different texts can justify a stable `E` or `I` pattern depending on the actual causal diagnosis.

When the old pattern disappears across sufficiently varied recent evidence, reduce confidence or clear it.

---

# 14. Graph relations guide diagnosis, not state propagation

B2 edges are used to interpret failures and choose what to verify next.

They never grant mastery.

Forbidden:

```text
prerequisite M3 => dependent M1/M2
transfer source M3 => target M2
parent M2 => child M2
```

Allowed:

```text
downstream failure + prerequisite uncertainty
=> verify prerequisite before treating downstream as negative evidence
```

This is a causal diagnostic use of the graph, not evidence propagation.

---

# 15. Update outcomes for tutoring

C3 itself does not choose the next TrainingMove, but it emits enough interpretable state for the next layer.

Examples:

### Outcome A — guided repair

```text
Attempt 1 H0: reasoning mixed
Attempt 2 H3: reasoning positive
```

Profile effect:

- mastery may initialize/hold at M1;
- automation remains A0/A1 or unknown;
- record intervention as effective;
- next recommendation should test same operation with reduced support.

### Outcome B — independent routine stability

```text
several distinct C2 H0 successes
```

Profile effect:

- M2 supported;
- C2 verified;
- confidence increases;
- if self-triggered, A2 may also rise.

### Outcome C — transfer proof

```text
M2 established + distinct unfamiliar H0 transfer successes
```

Profile effect:

- M3 becomes eligible;
- transfer confidence increases;
- complexity remains based on intrinsic bands of those tasks, not novelty.

---

# 16. Policy invariants

1. Node-specific observation beats whole-question correctness.
2. Unknown never becomes weak without valid negative evidence.
3. Scaffold level changes the meaning of success.
4. Independence is evaluated per node, not globally.
5. M/A/C update independently.
6. Hold + confidence change is a first-class outcome.
7. Diversity matters; repeated variants do not fabricate stability.
8. M3 requires unfamiliar generalization evidence.
9. Complexity is intrinsic Question demand; novelty is Attempt context.
10. One high-complexity failure does not automatically downgrade lower-band stable mastery.
11. Recent strong contradiction can eventually downgrade state.
12. Legacy evidence is preserved but cannot dominate stronger native evidence.
13. Error patterns require repetition and causal stability.
14. Graph relations guide diagnosis but never propagate mastery.
15. Every material Profile change is auditable through `ProfileUpdateDecision`.