# ADR-005 — Canonical LearningNode semantic boundaries

Status: **proposed in B1**

## Context

The v1 operational model uses a single ability-map row for concepts with very different semantics. Examples include:

- `文言实词` — accumulated lexical knowledge plus contextual inference;
- `修辞与表达效果` — rhetorical-device knowledge plus contextual analysis;
- `标题含义与作用` — primarily an exam Task Type containing several underlying abilities;
- `开放评价与迁移` — a family of evaluation/transfer operations;
- mastery/automation values stored next to the semantic definition itself.

This shape is useful for manual tutoring but weak for a dependency graph, personal profile and recommendation engine.

## Decision

ChineseTutor will use one canonical `LearningNode` registry with exactly three semantic node types:

```text
knowledge
ability
strategy
```

The registry is canonical and learner-independent.

### Knowledge
Stable facts, concepts, distinctions, conventions and rule systems the learner can know/recognize/recall/understand.

### Ability
Observable operations the learner can perform on language, text, information, artifacts or communication situations.

### Strategy
Reusable, multi-step procedures used to coordinate multiple operations across tasks; strategies can be prompted, faded, independently invoked and automated.

Task Types, Materials, Questions, Learner Profile state and Attempt evidence are separate entities.

## Consequences

### Positive

- profile state can attach uniformly to any relevant knowledge/ability/strategy node;
- dependency edges can cross current subject silos;
- knowledge gaps can be separated from reasoning gaps;
- task labels no longer masquerade as cognitive abilities;
- identical reasoning can transfer across modern/classical/poetry tasks;
- learner-facing method count can be kept small through Strategy consolidation.

### Costs

- v1 ability rows will not migrate one-to-one;
- current mixed labels must be split/generalized;
- aliases are necessary so school terminology remains recognizable;
- migration must be non-destructive until Profile and Attempt layers are ready.

## Rejected alternatives

### Separate unrelated Knowledge and Ability databases with independent identifiers

Rejected as the canonical semantic model because dependency edges and profile overlays would need special-case handling. Operational Notion views may still separate node types while sharing one canonical identity model.

### Keep the existing 42 rows as permanent atomic abilities

Rejected because several rows visibly combine knowledge, operation and task semantics, preventing precise diagnosis.

### Organize canonical nodes by grade

Rejected. Grade is source/load metadata. Progression is determined by prerequisites, complexity and learner evidence.

### Treat every question-type algorithm as a Strategy node

Rejected. This would recreate template proliferation. Only reusable mother procedures qualify as canonical Strategy nodes; consolidation is handled in PR G1.
