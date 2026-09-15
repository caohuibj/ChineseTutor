# F1 TrainingMove generation policy

Status: **normative draft**

The recommendation engine selects the next **training action** from the learner's current evidence state. It is not a chapter scheduler, grade ladder, or wrong-answer recommender.

Core question:

> Given current Profile + graph + evidence + available Questions, what training action has the highest expected learning value now?

---

## 1. Inputs

Required semantic inputs:

```text
B1 LearningNode
B2 dependency graph
C1 LearnerNodeState
C2 recent TrainingAttempts
C3 ProfileUpdateDecision / evidence confidence
D1 Material / TaskType / Question
D2 complexity vector
active learning goals / available time (when provided)
```

School/unit relevance may be supplied as a preference signal, not a prerequisite or learning-stage definition.

---

## 2. Recommendation pipeline

```text
1. build candidate target nodes
2. detect blockers / prerequisite uncertainty
3. classify the learner gap on each candidate
4. generate one or more candidate move types
5. determine evidence shape needed next
6. check eligible Question supply
7. assign priority class using explainable rules
8. rank within class using semantic tie-breakers
9. emit TrainingMove + rationale + success criterion
10. after Attempts, C3 updates Profile; recommendation is recomputed
```

Do not persist one long fixed path. Recompute after meaningful evidence.

---

## 3. Candidate target generation

A node may enter the candidate set when one or more are true:

- mastery is below active requirement;
- automation is below active requirement;
- verified complexity is below needed demand;
- evidence strength is weak/unknown on a high-value node;
- transfer evidence is missing despite M2 routine mastery;
- forgetting/review status is due;
- stable error pattern points to the node;
- node is a confirmed/candidate graph bottleneck;
- node is a hard prerequisite blocking an important downstream target;
- a recent Attempt shows a repair opportunity with high expected gain;
- current school material offers unusually cheap/valuable practice for an otherwise relevant node.

Do **not** add every unknown Gaokao node to the immediate queue merely because the graph is large.

---

## 4. Prerequisite handling

B2 `requires` is a hard semantic relation and must affect eligibility.

### 4.1 Demonstrated missing hard prerequisite

If node B requires A and A has valid evidence of non-establishment, then an **independent-performance** move on B is blocked when A's absence prevents valid observation of B.

Preferred action:

```text
repair/establish A
or diagnose A more precisely
```

B can remain an active goal, but should not be used as the primary independent target until the blocker is addressed.

### 4.2 Unknown prerequisite

Unknown is not failure.

If a high-value target depends on an unverified prerequisite, prefer a **cheap diagnostic probe** when uncertainty materially affects the next decision.

Do not automatically block the target merely because Profile state is null.

### 4.3 Supports relation

`supports` does not block. Weak supporting nodes can raise the value of support practice but cannot automatically prevent target training.

---

## 5. Gap → move-type mapping

### `diagnose`
Use when the decision is dominated by uncertainty rather than demonstrated weakness.

Signals:

- important node unknown;
- conflicting C3 evidence;
- suspected prerequisite but not verified;
- legacy-only low-confidence Profile state;
- need to distinguish K/R/I/E/M failure before choosing intervention.

Default design: short, high-information probe; avoid unnecessary teaching before observation.

### `establish`
Use for demonstrated M0 / missing foundational Knowledge or Ability.

Default design:

- lower irrelevant complexity dimensions;
- isolate target operation;
- permit sufficient support for correct construction;
- success criterion normally does not demand H0 independence immediately.

### `scaffolded_practice`
Use when current evidence says learner can progress with meaningful support (typically M1) but the target operation is not yet stable.

Default design:

- keep semantic demand representative but bounded;
- use H2-H4 only as needed;
- target one missing inferential/expressive layer at a time;
- record which intervention repairs the failure.

### `independent_practice`
Use to establish M2-quality routine independence.

Default design:

- H0/H1 ceiling;
- representative authentic material;
- enough diversity to avoid same-variant illusion;
- no unnecessary complexity jump.

### `fade_scaffold`
Use when execution quality exists but method/trigger dependence remains.

Typical profile:

```text
M1/M2 with A0/A1
recent H2/H3 repair success
```

Default design:

- keep TaskType and D2 demand roughly stable;
- reduce support one step at a time;
- prefer comparable but non-identical items;
- success is reduced hint dependence with stable quality.

### `automation`
Use when mastery is adequate but invocation/fluency under normal/timed conditions is below target.

Default design:

- do not make semantic reasoning much harder;
- lower prompt friction;
- use H0/H1;
- optionally increase time pressure gradually;
- evaluate speed **and** quality.

### `complexity_extension`
Use when mastery is stable at current demand but target complexity ceiling is lower than desired.

Default design:

- change one or a small number of D2 dimensions deliberately;
- avoid simultaneous large jumps across text load, reasoning, openness and expression unless testing integrated performance;
- retain the same canonical target operation.

### `transfer_probe`
Use when M2 is established and the main uncertainty is generalization.

Default design:

- unfamiliar material/task surface;
- exclude recent variants and over-familiar texts;
- H0/H1;
- keep intrinsic complexity near established range unless the goal also includes complexity extension;
- use meaningful semantic diversity, not cosmetic rewording.

### `review`
Use for established Knowledge/Ability/Strategy with meaningful forgetting risk or stale verification.

Default design:

- cheapest valid re-verification first;
- if successful, stop; do not force a full relearning sequence;
- if failed, C3 determines whether confidence/state changes and recommendation can switch to establish/scaffolded practice.

---

## 6. Minimal Effective Intervention is the tutoring objective

When the move is instructional rather than diagnostic, support should be sufficient to trigger productive repair but no larger than needed.

Desired progression often looks like:

```text
H5/H4 -> H3 -> H2 -> H1/H0
```

but it is not a mandatory fixed staircase.

The engine should choose support based on the **specific failed layer**:

```text
Q misunderstanding -> clarify task
R location -> location cue
K missing knowledge -> teach/retrieve knowledge
I reasoning gap -> one key causal/relational question
E expression gap -> ask learner to convert existing logic into scoreable sentence
M method trigger -> strategy/model cue
```

Do not reteach comprehension when evidence already shows the issue is expression.

---

## 7. Same TaskType, different learner → different move

Example: 人物形象题.

### Learner A

```text
M0 / location unstable
```

Move:

```text
establish
focus: relevant evidence location + fact extraction
H4/H3 permitted
```

### Learner B

```text
character reasoning M1
can identify evidence but omits warrant
```

Move:

```text
scaffolded_practice / fade_scaffold
focus: evidence -> explanation -> character judgment
H3 then H0
```

### Learner C

```text
M2 / A2 / C2 strong, transfer missing
```

Move:

```text
transfer_probe
unfamiliar material
H0
same C2-ish semantic demand
```

The TaskType is the same; the learning action is not.

---

## 8. School-material relevance

Current school material is valuable when it lowers friction or creates timely integration, but it is not the architecture's progression axis.

Use `school_relevance` as a tie-breaker when:

- two moves have similar learner value;
- the school text provides an authentic opportunity to train the same canonical node;
- curriculum timing makes immediate transfer useful.

Do not let it override a confirmed high-impact bottleneck without an explicit reason.

---

## 9. Availability handling

A theoretically ideal move with no suitable Question cannot be executed.

Question availability is therefore an operational factor, but it must not silently redefine learner need.

Possible outcomes:

```text
need high + supply strong -> recommend now
need high + supply weak -> recommend move + trigger material-search/authoring gap
need high + supply none -> preserve unmet need; select next-best executable move
need low + supply abundant -> do not train merely because questions exist
```

This separates **learning priority** from **content inventory convenience**.

---

## 10. Success criteria and stopping rules

A move should stop when its local evidence purpose is satisfied.

Examples:

### Diagnose

Stop after one or few probes sufficiently distinguish the failure layer.

### Fade scaffold

Stop when comparable performance is sustained below the target hint ceiling; then recompute, usually to independent practice/transfer rather than endless repetition.

### Review

One strong cheap re-verification may be enough.

### Transfer probe

One probe can provide evidence, but M3 promotion still follows C3 diversity rules. A TrainingMove can therefore end after one probe even if Profile remains M2.

---

## 11. Recommendation recomputation triggers

Recompute when:

- a TrainingMove success criterion is met or failed;
- C3 materially changes M/A/C/confidence;
- a prerequisite is newly established or disproved;
- a new stable error pattern emerges;
- review status becomes due/overdue;
- new suitable authentic material enters the Question bank;
- active learning goal/time constraint changes materially.

Do not recompute after every trivial metadata edit.

---

## 12. F1 policy invariants

1. The unit of recommendation is TrainingMove, not Question.
2. Recommendation is derived from current evidence and may change quickly.
3. Hard prerequisite deficits can block independent downstream moves; uncertainty triggers diagnosis rather than assumed failure.
4. High-centrality bottlenecks can outrank isolated lower scores.
5. School relevance is a preference, not progression.
6. The engine controls hints, complexity, novelty and TaskType independently where useful.
7. Transfer probes need unfamiliar/diverse context but need not be intrinsically C4/C5.
8. Review should seek the cheapest valid verification before reteaching.
9. No question is selected solely because inventory is abundant.
10. C3 remains the only layer that turns resulting evidence into Profile state.