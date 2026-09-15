# F3 integration with F1 TrainingMove recommendation

Status: **normative draft**

F3 does not create a second recommendation engine. It produces temporal review needs that F1 consumes alongside mastery, automation, transfer, prerequisites, ActiveRequirements and Question supply.

---

## 1. Inputs added to F1 candidate generation

For each learner-node candidate, F1 may consume:

```yaml
retention_family: string | null
mastery_review_status: unknown | current | due | overdue
automation_review_status: unknown | current | due | overdue
complexity_review_status: unknown | current | due | overdue
mastery_review_debt: unknown | none | light | material | urgent
automation_review_debt: unknown | none | light | material | urgent
complexity_review_debt: unknown | none | light | material | urgent
forgetting_risk: unknown | low | medium | high
last_review_decision_id: string | null
```

F1 must still inspect active requirements, dependency leverage, recent contradictions and available evidence rather than ranking by review status alone.

---

## 2. Candidate move shapes

F3 normally creates/strengthens one of these F1 move candidates:

### `review`
Use when the learner was previously established and the main question is temporal freshness.

### `diagnose`
Use when review evidence is ambiguous or verification history is insufficient to distinguish staleness from regression.

### `automation`
Use when mastery is current but automation review is due and the learner must re-demonstrate self-trigger/fluency.

### `transfer_probe`
Use when long-term retention of M3/generalization needs fresh evidence and routine mastery is already current.

F3 does not invent a special `reteach_due_to_time` move. Reteaching requires actual evidence of need.

---

## 3. Priority defaults

### Current
No review candidate unless an ActiveRequirement explicitly asks for fresh validation.

### Due + stable history
Usually `P3_maintenance`.

### Overdue + core relevance
Usually `P2_normal` or `P3_maintenance`, depending on downstream consequence and current training budget.

### Active requirement near
May rise to `P2_normal` / `P1_high` when fresh evidence materially matters.

### Recent contradiction
Prefer `P1_high diagnose/review` if the contradiction can change the path.

### Confirmed regressed hard prerequisite
Use ordinary F1 causal blocking logic. It may become `P0_blocking` or `P1_high` because of demonstrated regression—not because time elapsed.

---

## 4. Review vs new learning tie-breaks

When a maintenance review and a development move have similar priority:

1. prefer a cheap review if it protects an important prerequisite and costs little;
2. prefer development if the stale node is low-impact and evidence is still strong;
3. embed review into the development task when one Question can validly observe both;
4. do not allow many low-impact stale nodes to crowd out one major bottleneck.

---

## 5. Review saturation

Once a review purpose is satisfied by fresh valid evidence:

- suppress additional same-axis review candidates until the next horizon;
- do not continue because more inventory exists;
- variant repetition after successful verification has sharply lower marginal value.

If review fails, the node returns through C3/F1 based on the new state rather than remaining in a special review loop forever.

---

## 6. Embedded review selection

Before emitting a standalone review TrainingMove, F1 should check scheduled high-value tasks.

If an upcoming selected task:

- targets or validly observes the due node;
- meets independence requirements;
- meets the necessary complexity/automation conditions;

then F1 may annotate that task as also satisfying the F3 review purpose.

This reduces maintenance overhead without hiding what evidence is being sought.

---

## 7. Examples

### Example A — stale 文言实词

```text
Mastery M2 strong
mastery review due
no recent contradiction
```

→ short H0 contextual retrieval review, normally P3.

### Example B — same lexical node blocks current translation

```text
review overdue
recent translation failure traces to wrong word sense
current unit requires translation
```

→ P1/P0 prerequisite repair/diagnosis according to evidence. This is no longer mere maintenance.

### Example C — evidence reasoning used naturally

A new modern-text task independently shows the learner still completes evidence→warrant reasoning at C2/H0.

→ credit implicit review; do not schedule another standalone reasoning review.

### Example D — automation only due

```text
Mastery current M2
Automation A2 but last automation verification stale
```

→ automation move with H0/H1 and appropriate time pressure; no comprehension reteaching.

---

## 8. Integration invariants

1. F3 supplies temporal need; F1 remains the single global prioritizer.
2. Time alone never creates blocking status.
3. Review candidate generation is axis-specific.
4. Standalone review is avoided when an existing task can validly re-observe the construct.
5. Review success ends the current evidence purpose.
6. Review failure returns to the ordinary C2→C3→F1 loop.