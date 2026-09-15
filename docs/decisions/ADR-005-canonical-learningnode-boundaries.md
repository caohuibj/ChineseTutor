# ADR-005 — Canonical LearningNode semantic boundaries

Status: **accepted by B1 semantic self-review**

## Context

The v1 operational model uses single rows for concepts with different semantics, for example:

- `文言实词` — lexical knowledge plus contextual inference;
- `修辞与表达效果` — device knowledge plus contextual effect analysis;
- `标题含义与作用` — mainly a Task Type containing several underlying abilities;
- `开放评价与迁移` — a family of evaluation/transfer operations;
- learner mastery/automation stored beside semantic definitions.

This is useful for manual tutoring but weak for dependency modeling, personal profiles and recommendation.

## Decision

ChineseTutor uses one canonical `LearningNode` registry with exactly three semantic types:

```text
knowledge
ability
strategy
```

The registry is learner-independent.

### Knowledge
Stable facts, concepts, distinctions, conventions and rule systems that can be known, recognized, recalled or understood.

### Ability
Observable operations performed on language, text, information, artifacts or communication situations.

### Strategy
Reusable multi-step procedures that coordinate operations and can be prompted, faded, independently invoked, automated and transferred.

Task Types, Materials, Questions, Learner Profile state and Attempt evidence are separate entities.

## Stable identity

Canonical IDs are:

```text
CN-<TYPE>-<SLUG>
```

`domain` and `subdomain` are deliberately excluded from permanent identity because taxonomy can be refined without semantic change. IDs are never recycled.

## Domain and taxonomy

- `domain` is coverage/navigation, not progression;
- `subdomain` uses stable machine codes such as `META.evidence`, `CLA.lexicon`;
- taxonomic parent has the same node type and normally the same primary domain;
- prerequisites, supports, transfer and contrast are graph edges, not parent relations.

## Alias rule

`aliases_zh` contains only one-to-one semantic synonyms. A broad legacy row that splits into several v2 nodes is documented in the migration map rather than attached as an alias to one child.

## Cross-domain rule

Identical reasoning operations are generalized when doing so preserves diagnosis.

Example:

```text
modern character judgment
classical character judgment
        ↓
CN-A-evidence-to-character-judgment  domain=META
```

Modern/classical decoding remains source-specific prerequisite context.

Genre is handled differently because semantics differ:

```text
genre feature knowledge -> LIT
judge a modern text's genre from evidence -> MRD
```

## Consequences

### Positive

- learner state can overlay any canonical node uniformly;
- dependency edges can cross current subject silos;
- knowledge gaps can be distinguished from reasoning gaps;
- task labels no longer masquerade as abilities;
- cross-domain transfer can be represented explicitly;
- Strategy count can later be consolidated without losing task-specific metadata.

### Costs

- v1 rows will not migrate one-to-one;
- mixed labels require split/generalization;
- migration aliases cannot be used as a shortcut for semantic differences;
- operational Notion migration must be non-destructive until Profile/Attempt layers exist.

## Rejected alternatives

### Encode domain in permanent node ID
Rejected because domain is mutable taxonomy; moving a semantically identical node would otherwise break identity.

### Separate unrelated Knowledge and Ability registries
Rejected because dependency/profile tooling would need special-case identity rules.

### Keep all 42 v1 rows as permanent atomic abilities
Rejected because several visibly mix knowledge, operation and task semantics.

### Organize nodes by grade
Rejected. Grade is source/load metadata; progression follows dependencies, complexity and learner evidence.

### Treat every Task Type algorithm as a Strategy
Rejected. That recreates template proliferation. Only reusable mother procedures qualify; consolidation is deferred to G1.
