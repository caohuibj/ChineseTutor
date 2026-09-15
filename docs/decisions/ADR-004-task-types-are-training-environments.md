# ADR-004: Task types are training environments, not capability nodes

## Status

Accepted.

## Context

Exam-preparation systems often equate a named question type with a capability: `人物形象题`, `结构作用题`, `标题题`, and so on. That produces many templates but weak transfer.

Existing ChineseTutor practice already points in a better direction: the same evidence-reasoning operation appears in character analysis, theme, poetry emotion, classical reading and open evaluation; comparison logic appears across multi-text reading, contrast/foil, poetry comparison and classical comparison.

## Decision

Model three separate concepts:

- **Ability**: a transferable operation;
- **Strategy**: a reusable procedure for carrying out one or more operations;
- **Task Type**: an authentic environment that elicits those abilities and strategies.

A Task Type may map to several abilities, and an ability should normally be trained across more than one Task Type when transfer is expected.

## Consequences

- task-specific recognition signals and output conventions remain useful;
- question templates cannot become the canonical ability taxonomy;
- repeated success on only one task family provides weaker transfer evidence;
- reusable strategies can be consolidated into a small mother-model set.

## Validation

Cross-task abilities such as `证据->解释->结论` must be demonstrable in multiple task families before high transfer mastery is awarded.
