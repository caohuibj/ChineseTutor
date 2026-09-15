# Architecture QA gates

These gates prevent the system from drifting back into grade-first, template-first or manually scored learning management.

## Gate 1 - Semantic separation

For any new field, ask:

- Is this true of the canonical node/question for every learner?
- Or is it true only of one learner at one point in time?

Learner-specific data must not be stored as canonical meaning.

## Gate 2 - No hidden grade prerequisites

Search specifications and schemas for grade-based progression language. Source grade is allowed as metadata; prerequisite logic must resolve to actual knowledge/ability requirements.

## Gate 3 - Attempt evidence before mastery

No profile promotion rule may rely solely on a human-entered score without attempt evidence. Manual override can exist, but it must record a reason and should not erase underlying evidence.

## Gate 4 - Transfer diversity

A cross-task ability cannot receive highest mastery from repeated success on one nearly identical question family.

## Gate 5 - Strategy consolidation

Before creating a new learner-facing algorithm, test whether an existing mother strategy plus task-specific boundary information is sufficient.

## Gate 6 - Source traceability

Every authentic training question must retain:

- source category;
- year/region when known;
- original link/file reference when available;
- reliability level;
- whether the full source text is available.

Do not invent a source label for unverified material.

## Gate 7 - Material/question normalization

If several questions share one source text, create one Material and several Questions rather than duplicate the text as separate question records.

## Gate 8 - Minimal logging burden

Routine Attempt entry should take seconds, not minutes. If a required field does not change diagnosis, profile, recommendation or auditability, question whether it belongs in the minimum event schema.

## Gate 9 - Explainable recommendation

Every generated recommendation must expose:

- target node(s);
- profile gap;
- prerequisite status;
- selected training move;
- complexity target;
- success criterion;
- why this task is preferred now.

## Gate 10 - Gaokao coverage

For each recurring demand found in the coverage corpus, at least one of the following must be true:

1. it cleanly maps to existing nodes/task types;
2. it is explicitly judged out of scope with rationale;
3. a graph-gap issue exists.

## Gate 11 - Migration reversibility

Notion migrations use add/backfill/dual-run/verify before deprecation. A PR that deletes or overwrites historical learning evidence must include explicit backup and rollback procedures.

## Gate 12 - Pedagogical behavior preservation

Refactoring data models must not remove these established interaction guarantees:

- student attempts before model answer;
- one minimal prompt at a time;
- second attempt after targeted feedback;
- separate diagnosis of understanding and expression;
- authentic task environment;
- transfer check after method learning;
- exam-ready normalization only after reasoning is valid.
