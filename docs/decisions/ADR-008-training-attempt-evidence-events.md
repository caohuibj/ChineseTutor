# ADR-008 — TrainingAttempt is the learner-evidence event

Status: **proposed in C2; semantic self-review complete**

## Context

The existing `学习记录` is valuable as a durable recap but is too heavy and unstructured to act as per-question telemetry. A final answer alone also loses the most important teaching signal: what the learner could do before support, what minimal intervention was needed, and what changed in the second attempt.

ChineseTutor needs evidence granular enough to distinguish:

- understanding vs written expression;
- prerequisite failure vs downstream target failure;
- independent success vs heavily scaffolded success;
- familiar practice vs unfamiliar transfer;
- first attempt vs repaired second attempt;
- oral reasoning vs written production;
- one real target node vs incidental nodes in a multi-skill question.

## Decision

Each submitted learner response becomes one append-oriented `TrainingAttempt` event.

Successive responses to the same concrete Question share `attempt_series_id` and increment `attempt_number`.

The event records:

- Question/legacy reference;
- target nodes and per-node observed evidence;
- H0-H7 support context;
- 0/1/2/null diagnostic dimensions;
- Q/R/K/I/M/E/C error coding;
- material/task/familiarity/complexity snapshot;
- response modality and optional artifact reference;
- attempt-to-attempt delta on the later event;
- concise intervention/feedback notes when useful.

## Diagnostic scores are local observations

A `2` means successful **in this attempt under these conditions**, not global M2 mastery.

Profile aggregation occurs later across attempts, diversity, complexity, recency and hint dependence.

## Null is distinct from zero

Not applicable/not observed dimensions are null.

This prevents oral reasoning from becoming `written_expression=0` and open composition from becoming `answer_correctness=0` merely because a scalar score is inappropriate.

## Hint context changes what success proves

Correct after H6 is not the same evidence as correct at H0.

C2 treats hints as context, not punishment. The key automation signal is decreasing support requirement across comparable tasks.

## Per-node evidence is explicit

Question-level correctness must not be copied to every involved node.

If a classical lexical prerequisite fails before character reasoning can be assessed:

```text
lexical node = negative
character-reasoning node = not_observed
```

This prevents false profile downgrades.

## Response modality is explicit

During self-review, C2 added:

```text
written | typed_chat | oral | artifact | mixed | unknown
```

because ChineseTutor intentionally allows oral/rough reasoning before written normalization. Modality must not be confused with ability quality.

Long-form writing can reference an artifact/version instead of duplicating the full draft into every attempt.

## Historical context is snapshotted

Material/task/complexity/familiarity/variant-group metadata are stored with the event so later retagging does not silently alter what the historical evidence meant.

## Second-attempt delta is append-friendly

The later event stores `previous_attempt_id` and `delta_from_previous`. Earlier events never contain future information.

## Attempt events do not replace durable summaries

```text
TrainingAttempt = raw structured evidence
学习记录 / Session Summary = promoted durable synthesis
```

Long recap is reserved for meaningful method formation, stable errors, breakthroughs, transfer events and closure.

## Consequences

### Positive

- learner progress becomes measurable at the actual tutoring step;
- hint dependence becomes visible;
- understanding and expression stop being conflated;
- C1 Profile can later be recomputed from evidence;
- multi-node causal diagnosis becomes possible;
- ordinary attempts remain cheap to log;
- current historical sessions can be migrated conservatively.

### Costs

- questions need node/task/complexity metadata for high-quality aggregation;
- per-node evidence adds some structure to multi-skill tasks;
- legacy records cannot recover telemetry that was never captured;
- C2 still does not define the final M/A/C update formula.

## Rejected alternatives

### Store only final correctness
Rejected because it loses hint dependence, causal failure layer and second-attempt learning.

### Store one record per Question/session
Rejected because first and second attempts have different evidentiary meaning.

### Score every dimension 0 when not relevant
Rejected because not-applicable is not failure.

### Apply attempt correctness to every target node
Rejected because upstream failure may make downstream nodes unobservable.

### Put second-attempt delta on the first event
Rejected because it inserts future information and breaks append-oriented event history.

### Treat oral answers as written-expression evidence by default
Rejected because the project explicitly separates thinking from final written expression.
