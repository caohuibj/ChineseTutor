# ADR-002: Learner Profile is a dynamic overlay, not part of canonical nodes

## Status

Accepted.

## Context

The v1 workspace stores learner-specific properties such as mastery, automation, last-trained and needs-improvement directly on ability definitions. Similar mixing occurs when a reusable question stores `已练/需复练`, or a method card stores learner familiarity.

This works for one learner and a small dataset, but it makes the semantic model ambiguous and prevents reliable evidence-based updates.

## Decision

Canonical entities contain only properties that remain true independently of a learner:

- definition;
- domain;
- dependencies;
- applicable strategies;
- source provenance;
- examination relevance;
- observable success criteria.

Learner-specific properties live in separate state/event entities:

- `LearnerNodeState` for current profile state;
- `TrainingAttempt` for raw evidence;
- `TrainingMove/Queue` for learner-specific next actions;
- `ErrorPattern` for promoted recurring failures.

## Consequences

A single canonical node can support multiple learners later without redesign.

More importantly, the system can answer two distinct questions:

1. What does this node mean?
2. What evidence shows this learner's current state on the node?

## Validation

A canonical node exported without learner tables must still be a complete semantic definition. Deleting a learner profile must not change the canonical graph. Conversely, profile reconstruction should be possible from attempt evidence plus update policy.
