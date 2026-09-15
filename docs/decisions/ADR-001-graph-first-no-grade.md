# ADR-001: Progression is graph-first, not grade-first

## Status

Accepted.

## Context

Chinese-language learning materials are commonly organized by school grade, textbook unit and exam stage. Those dimensions are useful for scheduling and source provenance, but they are poor primary models of an individual's actual capability.

A learner may be able to perform a high-level reasoning operation on one domain while still having a basic lexical or classical-language gap in another. A grade-first progression therefore causes both artificial blocking and hidden foundational gaps.

## Decision

Canonical progression is driven by:

1. prerequisite graph readiness;
2. learner-node mastery and automation;
3. task complexity;
4. transfer evidence;
5. forgetting/review risk;
6. current curriculum relevance when useful.

Grade/year is retained only as:

- source metadata;
- a reading-load/context signal;
- a school-synchronization preference.

No canonical prerequisite rule may be expressed as `must be grade X before node Y` unless the requirement is actually a knowledge prerequisite and is represented as such.

## Consequences

Positive:

- earlier access to high-value reasoning operations when prerequisites are ready;
- direct remediation of foundational gaps regardless of nominal grade;
- a single architecture can span junior high through Gaokao;
- recommendation becomes personalized rather than cohort-based.

Costs:

- task complexity must be modeled explicitly;
- source-grade labels can no longer serve as a shortcut for difficulty;
- prerequisite quality becomes critical.

## Validation

Gaokao and junior-high tasks should map into the same graph at different complexity and knowledge-distance settings rather than into separate grade-specific capability systems.
