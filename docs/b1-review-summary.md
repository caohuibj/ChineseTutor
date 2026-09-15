# B1 review summary

Issue: #1 — Define canonical LearningNode schema and taxonomy

## Implemented

- stable domain-independent `CN-<TYPE>-<SLUG>` identity contract;
- canonical node types: Knowledge / Ability / Strategy;
- required/optional/forbidden fields;
- eight-domain taxonomy with stable machine subdomain codes;
- semantic domain-assignment precedence;
- split/merge/generalization rules;
- same-type taxonomic parent rule;
- one-to-one alias rule;
- representative canonical fixtures across major domains;
- explicit migration disposition for all 42 current v1 ability-map rows;
- semantic QA gates;
- ADR for canonical LearningNode boundaries.

## Self-review findings and resolution

The first B1 draft exposed seven issues. All are resolved in the current branch:

1. **ID/domain contradiction — fixed.** Permanent IDs no longer encode domain; taxonomy may move without identity change.
2. **Character reasoning duplicated across domains — fixed.** Evidence→character reasoning is canonical `META`; modern/classical decoding remains contextual prerequisite.
3. **Subdomain inconsistency — fixed.** Canonical records use machine codes such as `CLA.lexicon` and `META.evidence`.
4. **Aliases hiding split migrations — fixed.** Aliases are now restricted to one-to-one semantic synonyms.
5. **Genre placement ambiguity — fixed.** Genre Knowledge is `LIT`; modern evidence-based genre judgment is `MRD`.
6. **QA scope wording — fixed.** Live Notion migration is explicitly outside B1 rather than an unchecked completion condition.
7. **LAN/COM/META overlap — fixed.** Taxonomy now defines semantic domain-assignment precedence.

Additional hardening:

- `parent_node_id` must point to a node of the same semantic type and normally the same primary domain;
- grade/year remains excluded from identity and progression;
- mixed v1 names remain visible in the migration map rather than being silently attached to one child node.

## B1 acceptance judgment

**PASS — semantic contract is internally consistent and ready to serve as the base for B2.**

The key remaining uncertainty is not B1 semantics but edge behavior: which dependencies are hard vs soft, how cycles are prevented, and how supports/transfer/strategy relations should affect recommendation. Those questions are intentionally B2 scope.

## Deliberately deferred

- dependency-edge schema and integrity rules — B2;
- live Notion migration/backfill;
- LearnerNodeState/Profile — C1;
- TrainingAttempt evidence — C2;
- exhaustive Gaokao graph population;
- Strategy consolidation — G1.
