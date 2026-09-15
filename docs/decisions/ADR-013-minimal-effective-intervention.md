# ADR-013 — Minimal effective intervention with causal, node-specific support

Status: Proposed / F2

## Context

ChineseTutor already had two strong but separate ideas:

1. pedagogically, do not give the answer immediately; identify what the learner already understands, repair one missing layer, then ask for a second attempt;
2. evidentially, C2/C3 distinguish hint levels and node-specific independence.

Without a formal F2 layer, these can drift apart. A tutor might give rich explanations that feel helpful but destroy diagnostic value, or preserve “clean evidence” by withholding help after the learner is clearly blocked.

The system also must distinguish weak Chinese expression from weak reasoning, because this learner often understands a relation before being able to write it in exam-ready language.

## Decision

ChineseTutor will use an explicit `InterventionDecision` between learner Attempts.

The default policy is **minimal effective intervention**:

```text
observe what is correct
→ diagnose earliest actionable causal gap
→ target one layer
→ use least revealing support likely to work
→ request learner retry
→ reassess
→ fade support or escalate causally
```

### Key semantic decisions

#### 1. Tutor help is not one global scalar

H0-H7 remains useful for historical support context, but evidence interpretation is node-specific.

Example:

```text
H4 supplies location
```

may invalidate independent `text_location` evidence while preserving independent reasoning evidence.

#### 2. Planned hint ceiling is not a prohibition

F1 may plan H1 maximum for a fade-scaffold move. If the learner unexpectedly needs H4, F2 gives the help, records the override, and marks the intended evidence condition as unmet.

The system optimizes learning first while preserving honest evidence.

#### 3. One intervention normally repairs one causal layer

The tutor can acknowledge what is right and ask one focused question, but should not simultaneously supply location, evidence, reasoning and final prose.

#### 4. Second attempt is the normal checkpoint

Tutor explanation does not itself demonstrate learner change. The learner usually responds again so repair can be observed.

#### 5. Expression conversion is a separate teaching path

When reasoning is valid but writing is weak, route to `E / expression_conversion` rather than reopening comprehension.

Oral/arrow reasoning is accepted diagnostically, but independent written expression remains a training endpoint.

#### 6. H6/H7 are legitimate teaching modes, not evidence fraud

Once partial reasoning or a near answer is supplied, the supplied node cannot later count as independently demonstrated on the same item. A new item is required for independent verification.

## Consequences

Positive:

- preserves student thinking rather than replacing it;
- makes support efficient and personalized;
- creates a direct audit trail from first answer to intervention to second answer;
- enables C3 to interpret evidence honestly;
- supports scaffold fading and automation training;
- prevents expression weakness from being misdiagnosed as comprehension weakness.

Costs:

- tutor must infer causal failure, which can be uncertain;
- InterventionDecision adds another event/entity;
- H0-H7 cannot carry all semantics alone, so node effects must be recorded;
- prompt implementation must resist the tendency to over-explain.

## Rejected alternatives

### Always provide full explanation after an error
Rejected because it reduces learner cognitive work and destroys evidence about the missing layer.

### Strict fixed H1→H7 ladder
Rejected because the relevant support depends on causal failure; a location problem and a reasoning problem need different interventions.

### Never exceed F1 hint ceiling
Rejected because recommendation intent must not override actual teaching needs.

### Treat final answer quality as enough diagnosis
Rejected because the same wrong answer can result from Q/K/R/I/M/E/C failures.

### Treat oral answer as sufficient endpoint
Rejected because the project explicitly aims to convert understanding into stable written Chinese, not bypass that weakness.

## Verification

This ADR is satisfied when F2 QA demonstrates:

- same final error receives different intervention under different causal diagnoses;
- reasoning-correct/expression-weak receives expression conversion;
- H4/H5 help preserves unaffected downstream observation;
- hint ceiling can be overridden transparently;
- H6/H7 triggers later new-item verification;
- second-attempt delta is captured through C2.