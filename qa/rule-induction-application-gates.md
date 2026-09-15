# R1.3 rule induction/application semantic gates

A release-candidate implementation passes when all of the following hold.

## Node boundaries

- [ ] `CN-A-induce-rule-from-examples` is an Ability, not a TaskType or Strategy.
- [ ] `CN-A-apply-rule-to-case` is an Ability, not a TaskType or Strategy.
- [ ] Neither node identity contains grade, exam year, source domain, novelty or transfer status.
- [ ] Induction success requires abstraction of a reusable relation/condition, not merely listing facts.
- [ ] Application success requires matching rule conditions to case facts, not merely restating the rule.
- [ ] Evidence-sufficiency evaluation remains separate from both nodes.
- [ ] Multi-constraint full-plan synthesis remains outside this slice.

## Diagnostic causality

- [ ] In derive-then-apply tasks, failed induction can make application `not_observed` rather than automatically negative.
- [ ] Correct induction plus incorrect case mapping can yield induction=positive and application=negative.
- [ ] When a rule is supplied by the task, induction is `not_observed` while application can be independently assessed.
- [ ] Question-level correctness is never copied to both nodes without node-specific evidence.

## Graph semantics

- [ ] S4/S13/S10 links use `strategy_for`, not `requires`.
- [ ] Rule induction does not globally hard-require rule application or vice versa.
- [ ] Induction→application uses only a scoped soft relation because supplied-rule tasks exist.
- [ ] No mastery propagates automatically across the relation.

## Evidence/Profile compatibility

- [ ] H3 repair can support guided mastery evidence without fabricating automation.
- [ ] Near-duplicate variants cannot produce M3 by count alone.
- [ ] Cross-domain unfamiliar independent success can contribute to M3 when C3 diversity requirements are met.
- [ ] Transfer is recorded in Attempt context, not encoded in C-band or node identity.

## Source discipline

- [ ] Authentic mappings use only demands already represented in I1/project evidence.
- [ ] Paraphrased demand labels are not presented as verbatim exam stems.
- [ ] Non-exam examples are explicitly marked as generality checks or synthetic fixtures.

## MVP scope control

- [ ] No new TaskType is created in this PR.
- [ ] No new mother Strategy is created in this PR.
- [ ] #33, #34, #35 and #36 remain separate follow-up work.
