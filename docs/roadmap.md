# ChineseTutor implementation roadmap

## Delivery strategy

ChineseTutor will evolve through small, reviewable PRs. The objective is not to build all databases at once; it is to progressively move from a working v1 tutoring workspace to a normalized graph/profile/evidence system while preserving daily usability.

Each PR should contain:

- a precise problem statement;
- design scope and explicit non-goals;
- schema or workflow changes;
- migration/backfill steps;
- acceptance tests using real learning examples;
- rollback path;
- no unrelated cleanup.

---

# Milestone A - Architecture baseline

## PR A1 - Graph-first product architecture

**Status:** current refactor PR.

### Goal

Freeze the v2 product model before changing live Notion schemas.

### Commits

```text
docs: audit current ChineseTutor learning system
docs: define graph-first profile-centered architecture
docs: design non-destructive Notion refactor
docs: add staged PR and commit roadmap
docs: define architecture decisions and QA gates
```

### Acceptance

- grade-first progression is explicitly rejected;
- canonical graph, profile, material/question, attempt and recommendation entities are defined;
- current v1 data has a non-destructive migration mapping;
- no private learner details are committed to the public repository.

---

# Milestone B - Canonical graph foundation

## PR B1 - LearningNode schema and node taxonomy

### Goal

Create the canonical semantic layer for Knowledge, Ability and Strategy nodes.

### Commits

```text
spec: define LearningNode canonical schema
spec: define domain and node-type taxonomy
spec: define observable success criteria format
qa: add node semantic classification examples
```

### Acceptance

- every existing v1 Ability Map row can be classified as knowledge, ability, strategy, or explicit split/merge candidate;
- node naming conventions are stable;
- node IDs do not depend on Notion page IDs.

## PR B2 - Dependency graph model

### Goal

Make progression prerequisite-driven.

### Commits

```text
spec: define dependency edge semantics
spec: define requires supports part-of and transfers-to rules
qa: add classical-chinese dependency examples
qa: add modern-reading and writing dependency examples
```

### Acceptance

- hard prerequisite and soft support are distinguishable;
- cyclic hard dependencies are prohibited;
- examples show how the graph selects prerequisites without using grade.

## PR B3 - Backfill current nodes

### Goal

Map all current v1 nodes into the canonical graph.

### Commits

```text
data: inventory current ability-map nodes
data: classify knowledge and ability nodes
data: extract reusable strategy candidates
qa: validate one-to-one split and merge decisions
```

### Acceptance

- 100% of existing nodes have a migration disposition;
- no current training capability is silently lost;
- likely missing Gaokao-level nodes are recorded as issues rather than improvised additions.

---

# Milestone C - Learner Profile and evidence

## PR C1 - LearnerNodeState schema

### Goal

Separate static graph meaning from personal dynamic state.

### Commits

```text
spec: define learner node state schema
spec: define M0-M3 mastery semantics
spec: define A0-A3 automation semantics
spec: define evidence strength and review-due semantics
```

### Acceptance

- profile state can represent stable, developing, bottleneck, review-due and unknown;
- one node may have strong mastery but low automation;
- one node may have high historical mastery but weak current evidence strength.

## PR C2 - TrainingAttempt event schema

### Goal

Create the primary evidence event.

### Commits

```text
spec: define attempt event model
spec: define H0-H7 hint scale
spec: define diagnostic 0-2 dimensions
spec: map K/R/I/E/Q/M/C errors to attempt events
qa: encode Zhou-Yafu example as fixture
```

### Acceptance

- first and second attempt can be represented without a long recap;
- understanding and expression failures are distinguishable;
- reduced hint dependence can be measured.

## PR C3 - Evidence-based profile update policy

### Goal

Make profile changes explainable and conservative.

### Commits

```text
spec: define mastery promotion rules
spec: define automation promotion rules
spec: define evidence diversity requirements
spec: define downgrade and review-due rules
qa: add contradictory-evidence scenarios
```

### Acceptance

- one correct familiar question cannot produce M3;
- transfer requires unfamiliar evidence;
- every profile change can cite Attempt evidence.

---

# Milestone D - Material and task normalization

## PR D1 - Material entity

### Goal

Represent authentic texts independently from questions.

### Commits

```text
spec: define material schema and source provenance
spec: define genre and medium taxonomy
spec: define source-grade as metadata only
qa: add school-unit multi-question material examples
```

## PR D2 - TaskType v2

### Goal

Migrate current question-type algorithms into a task environment linked to canonical strategies.

### Commits

```text
spec: define TaskType v2 schema
data: map current task types to target nodes
refactor: replace embedded algorithms with strategy relations
qa: identify duplicate algorithms across task types
```

## PR D3 - Question entity and complexity vector

### Goal

Represent each concrete prompt with precise targets and multi-dimensional complexity.

### Commits

```text
spec: define Question schema
spec: define complexity-vector dimensions
spec: define C0-C5 user-facing bands
qa: score representative questions across domains
```

### Acceptance

- difficulty is not inferred from source grade;
- transfer and complexity are not conflated;
- the same Material can own several Questions.

---

# Milestone E - Authentic source pipeline

## PR E1 - Candidate-to-formal promotion workflow

### Goal

Normalize the existing candidate pool into Materials and Questions without losing provenance.

### Commits

```text
spec: define candidate promotion states
spec: define source reliability rules
spec: define duplicate detection policy
spec: define incomplete-source handling
```

## PR E2 - Coverage-gap driven sourcing

### Goal

Search for missing training coverage rather than accumulating generic questions.

### Commits

```text
spec: define node-task-complexity coverage matrix
spec: define minimum coverage targets
prompt: add targeted Work-mode sourcing prompt
qa: add example gap report
```

### Acceptance

A sourcing request can say:

> Find two unfamiliar C3 tasks for Ability X using Task Types Y/Z because current inventory only covers C1-C2.

instead of:

> Find more reading questions.

---

# Milestone F - Recommendation engine v1

## PR F1 - TrainingMove model

### Goal

Recommend an instructional action, not merely a question.

### Commits

```text
spec: define TrainingMove schema
spec: define learn/scaffold/fade/timed/transfer/review moves
spec: define success criteria format
```

## PR F2 - Explainable priority policy

### Goal

Select the next high-value target from learner state and graph structure.

### Commits

```text
spec: define need-value-feasibility priority model
spec: define prerequisite blocking rules
spec: define dependency unlock value
spec: define transfer-gap and forgetting-risk rules
qa: add learner-profile selection scenarios
```

### Acceptance

Every recommendation includes:

- target node;
- why now;
- chosen move type;
- desired complexity;
- preferred task family;
- success criterion.

## PR F3 - Training Queue / review integration

### Goal

Replace manually fragmented review status with a unified queue.

### Commits

```text
spec: define queue lifecycle
migration: map current review-plan records
spec: define review as one recommendation reason
qa: test queue deduplication and completion
```

---

# Milestone G - Adaptive tutoring

## PR G1 - Failure-layer diagnosis policy

### Goal

Identify the earliest causal failure instead of over-explaining.

### Commits

```text
spec: define question-reading localization knowledge reasoning method expression diagnosis
spec: define intervention choice rules
qa: add same-question different-profile examples
```

## PR G2 - Minimal Effective Intervention

### Goal

Reduce scaffolding as quickly as evidence allows.

### Commits

```text
spec: define intervention ladder
spec: define scaffold-fading policy
spec: define when to skip explanation and go directly to timed practice
qa: add hint-reduction progression fixtures
```

## PR G3 - Mother strategy consolidation

### Goal

Reduce learner-facing algorithms to a compact transferable set.

### Commits

```text
data: cluster current algorithms by shared reasoning structure
spec: define 10-15 mother strategies
refactor: link task types to mother strategies
qa: preserve task-specific boundaries
```

---

# Milestone H - Writing, reading and literature expansion

## PR H1 - Writing graph and portfolio evidence

- narrative construction;
- revision;
- argumentation;
- material transfer;
- versioned writing evidence.

## PR H2 - Authentic reading and whole-book graph

- sustained reading;
- literary/cultural knowledge;
- reader response;
- cross-text relationships;
- no conversion of all reading into worksheet questions.

## PR H3 - Practical/oral/cross-media communication

- audience-purpose-information-order-expression model;
- interview, speech, discussion, chart/text conversion, cross-media tasks.

---

# Milestone I - Gaokao coverage validation

## PR I1 - Recent-paper mapping fixtures

### Goal

Map selected recent Beijing and national questions to:

```text
Material -> TaskType -> Knowledge -> Ability -> Strategy -> Complexity
```

## PR I2 - Coverage tests

### Tests

```text
all recurring exam demands map to nodes
all important abilities have task environments
central abilities have transfer across multiple task families
high-value nodes have increasing-complexity material coverage
no grade-level dependency rules remain in canonical progression logic
```

## PR I3 - Gap closure

Only add graph nodes when a recurring authentic demand cannot be represented cleanly.

---

# Milestone J - v2 operational cutover

## PR J1 - Notion migration M1-M3

Create canonical graph, learner state and Attempt databases while keeping v1 live.

## PR J2 - Dual-run evaluation

Use both systems for a defined set of real training sessions and compare:

- logging burden;
- diagnostic precision;
- recommendation quality;
- profile explainability;
- ability to choose transfer tasks.

## PR J3 - Deprecation plan

Hide redundant v1 fields/views only after validation. Do not delete historical data until backup and migration audit are complete.

---

# PR Definition of Done

Every implementation PR must answer:

1. **Problem** - what current limitation is being solved?
2. **Goal** - what new capability exists afterward?
3. **Non-goals** - what is deliberately excluded?
4. **Data impact** - which entities or fields change?
5. **Migration** - how existing data survives?
6. **Learner impact** - what changes during actual tutoring?
7. **Acceptance** - observable completion criteria?
8. **Rollback** - how to revert safely?

---

# Immediate next sprint

Do not implement all milestones in parallel.

Recommended first implementation sprint after this architecture PR:

```text
B1 LearningNode schema
B2 Dependency model
B3 Current-node backfill audit
C1 LearnerNodeState schema
C2 TrainingAttempt schema
```

Only after real attempts are logged should the recommendation policy be tuned. Profile/recommendation design without event evidence would otherwise be speculative.
