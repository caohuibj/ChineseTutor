# ADR-016 — Gaokao is the coverage boundary, not the learner progression axis

Status: **Accepted in I1 semantic design**

## Context

ChineseTutor targets complete Gaokao knowledge/capability coverage, while the current learner is younger and follows school coursework. A naive implementation would organize the graph by school year or prematurely push Gaokao question forms downward.

The architecture instead needs a stable answer to two different questions:

1. **What must the system eventually be capable of teaching/assessing?**
2. **What should this learner train next?**

These must not be conflated.

## Decision

Recent authentic Gaokao demands define the **terminal coverage test** for Knowledge, Ability, TaskType, Strategy and complexity inventory.

Learner progression remains determined by:

```text
canonical dependencies
+ Learner Profile
+ evidence quality
+ ActiveRequirement
+ F1 TrainingMove value
```

not by grade/year.

Therefore:

- Gaokao year/region/stage is provenance on Material/Question;
- a recurring Gaokao operation may motivate a canonical node at any point in the graph;
- the node is trainable whenever prerequisites/readiness justify it;
- the current learner need not attempt the original Gaokao item;
- lower-complexity authentic tasks may train the same underlying Ability;
- no “高中能力” label may serve as a prerequisite by itself.

## Coverage-audit consequence

When a representative Gaokao task does not map cleanly, I1 classifies the gap as:

```text
Knowledge
Ability
TaskType
Dependency
Complexity
Material coverage
Architecture
```

and opens an explicit issue.

The fix must not be:

```text
create a question-specific template
or
create a grade-specific duplicate node
```

## Findings from I1

The representative 2022–2026 Beijing / 2025–2026 National demand set did not require a new top-level entity type or a new complexity dimension.

The principal gaps were:

- representation/discourse transformation;
- rule induction/application;
- evidence sufficiency/claim validity;
- purpose-driven question formulation;
- multi-constraint solution synthesis;
- real-communication TaskType inventory;
- Gaokao-supporting Knowledge population;
- authentic complexity ladders.

All are expressible inside the existing B1–G1 architecture.

## Why this matters for the current learner

A younger learner may begin building the same underlying operations through current school material:

```text
证据为什么支持结论
比较时先建立维度
从实例概括规则
判断一个证据够不够
根据对象/目的调整表达
```

The representation and task complexity can grow later without changing the underlying graph identity.

This preserves cognitive ambition without pretending that current coursework should be replaced by Gaokao drills.

## Rejected alternatives

### Grade ladder as graph spine

Rejected because it duplicates constructs, creates artificial prerequisites and makes source year determine learning semantics.

### Gaokao task label as node

Rejected because authentic TaskTypes often compose several Abilities/Knowledge/Strategies and cannot serve as atomic learner state.

### Ignore Gaokao until high school

Rejected because it prevents backcasting terminal coverage and may allow early graph design to omit high-value shared operations.

## Consequences

Positive:

- one canonical Ability can be trained from junior-school to Gaokao complexity;
- full terminal coverage is auditable;
- school material remains usable as current authentic training supply;
- graph gaps are discovered before mass migration.

Cost:

- content ingestion must map tasks semantically rather than by grade bucket;
- Knowledge/Ability graph population requires disciplined decomposition;
- coverage must be re-audited as new exam forms appear.

## Invariant

> **Gaokao defines how complete the map must eventually be; the learner's evidence and dependencies decide where to walk next.**
