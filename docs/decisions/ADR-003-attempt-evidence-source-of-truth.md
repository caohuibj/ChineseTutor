# ADR-003: Training Attempts are the source of truth for learner evidence

## Status

Accepted.

## Context

A manual mastery number is convenient but weakly grounded. One correct answer may come from familiarity, heavy scaffolding or a nearly identical previous task. Conversely, an incorrect final sentence may hide valid comprehension and reasoning.

ChineseTutor needs a dynamic personal profile that explains both progress and training recommendations.

## Decision

The primary evidence unit is a structured `TrainingAttempt` event.

At minimum an attempt records:

- concrete question/material;
- target nodes;
- attempt number;
- hint level;
- task recognition;
- text location;
- evidence selection;
- reasoning;
- terminology;
- written expression;
- correctness;
- error code(s);
- intervention and second-attempt change when applicable.

Profile mastery, automation, error patterns and recommendation priority are derived or justified from a sequence of these events.

Session summaries are secondary human-readable synthesis. They do not replace raw attempt evidence.

## Consequences

Benefits:

- separates understanding from expression;
- measures scaffold dependence;
- supports transfer validation;
- makes profile changes explainable;
- allows recommendation rules to improve over time without losing historical evidence.

Costs:

- data entry must be kept deliberately small;
- backfill from old narrative records will be incomplete;
- early profile confidence should remain conservative.

## Validation

For any future claim such as `M3/A2`, the system should be able to point to supporting attempts and explain why the evidence satisfies the promotion rule.
