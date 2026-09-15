# ADR-010 — Task complexity is independent of learner novelty and grade

Status: **proposed in D2**

## Context

Earlier architecture shorthand used a convenient sequence such as:

```text
C0 recognition
C1 single-step
C2 multi-step
C3 integration
C4 unfamiliar transfer
C5 open evaluation
```

That shorthand conflates two different constructs:

1. intrinsic demands of the Question;
2. whether the Question/material is unfamiliar to this learner and therefore valid transfer evidence.

It also risks recreating grade-first difficulty when source grade is used as a proxy for challenge.

## Decision

ChineseTutor models Question complexity as an eight-dimensional learner-independent vector:

```text
text load
information hiddenness
reasoning depth
material heterogeneity
knowledge retrieval distance
response openness
expression load
time pressure
```

A reviewed `C0-C5` band summarizes dominant semantic demand but never replaces the vector.

Learner-relative novelty/transfer remains in `TrainingAttempt`:

```text
material_familiarity
transfer_probe
question_variant_group_id
hint context
```

### Compatibility amendment

This ADR **supersedes the earlier C1 shorthand `C4 = unfamiliar transfer` wherever it appears in stacked draft documentation**.

After D2:

```text
C4 = high integration / remote application / constrained evaluation
```

An unfamiliar transfer probe may be C1, C2, C3, C4 or C5.

M3 Profile semantics continue to require stable generalization/transfer evidence, but that evidence comes from C2 Attempt context rather than from the C-band number.

## Band interpretation

```text
C0 recognition/direct retrieval
C1 single-step application
C2 multi-step explanation
C3 integrated multi-node/multi-source synthesis
C4 high integration / remote application / constrained evaluation
C5 open evaluation / complex construction
```

Band review follows semantic precedence rather than a naive weighted average.

## Consequences

### Positive

- difficulty can be compared across source grades;
- transfer can be tested while holding intrinsic task demand constant;
- recommendation can manipulate one dimension at a time;
- expression bottlenecks can be trained without unnecessarily raising reasoning depth;
- automation can be trained by time/hint reduction without pretending the semantic task became harder;
- high-school/Gaokao bridge tasks can be selected by actual demand rather than label.

### Costs

- reviewers must score several dimensions instead of one subjective difficulty label;
- some legacy `基础/常规/提升/迁移` labels cannot be migrated automatically;
- inter-rater calibration is needed for stable Question curation.

## Rejected alternatives

### Use source grade as complexity
Rejected because grade is provenance/load context, not cognitive demand.

### Treat unfamiliarity as C4
Rejected because novelty is learner-relative. The same Question can be familiar to one learner and unseen to another.

### Use a single weighted difficulty score
Rejected because different vectors with identical averages demand different training interventions.

### Let time pressure raise the semantic band
Rejected because time pressure primarily changes automation evidence; a simple task under time pressure remains semantically simple.

## Migration

Legacy difficulty labels remain as provenance/curation notes and become provisional until the actual Question is vector-scored. `迁移` may flag transfer-probe candidacy but does not determine C-band.