# R1.3 semantic self-review — rule induction/application

## Reviewed risks

1. **Collapsing induction and application.** Rejected because supplied-rule tasks can observe application with zero induction evidence, while derive-then-apply tasks can show correct induction plus failed application.
2. **Encoding novelty in the node.** Rejected. Novelty/transfer stays in `TrainingAttempt` context.
3. **Making induction a hard prerequisite of application.** Rejected because authentic supplied-theory tasks exist. A scoped `supports` edge is sufficient.
4. **Creating a new mother Strategy.** Rejected. S4, S10 and S13 already coordinate the required procedures.
5. **Treating failed downstream answers as application failure after bad induction.** Rejected. Application is `not_observed` when the usable rule never existed.
6. **Merging evidence sufficiency into induction.** Rejected. A learner can form a rule but overstate its evidential boundary; #31 remains a separate Ability.
7. **Absorbing solution synthesis.** Rejected. Applying one rule/condition to a case is not the same as satisfying several constraints in one coherent plan (#33).
8. **Using item count as transfer proof.** Rejected under C3. Variant diversity and independent unfamiliar performance remain required.

## Result

The Issue #30 minimum slice can be represented with exactly two new Ability nodes and existing B2/G1/C2/C3 semantics. No architecture, learner-state axis, Strategy, TaskType or complexity-model change is needed.
