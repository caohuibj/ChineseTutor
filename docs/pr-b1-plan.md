# PR B1 implementation plan — Canonical LearningNode schema and taxonomy

This file is a temporary implementation note for Issue #1. It will be removed or folded into the final schema docs before the PR is merged.

## Objective

Define a stable canonical `LearningNode` contract that separates three semantics:

- **knowledge** — what the learner knows or recognizes;
- **ability** — what the learner can observably do;
- **strategy** — a reusable procedure the learner can deliberately invoke.

The schema must be independent of Notion page IDs, learner-specific mastery state, school grade, and any one question type.

## Deliverables

1. `schemas/learning-node.schema.md` — normative field contract and invariants.
2. `graph/taxonomy.md` — domain/subdomain taxonomy and naming rules.
3. `graph/seed/v1-ability-map-migration.md` — migration disposition for all 42 current v1 nodes.
4. `qa/learning-node-gates.md` — semantic QA checks and representative examples.
5. update `docs/architecture-v2.md` if implementation decisions sharpen the architecture contract.

## Acceptance checks

- every v1 ability-map row has an explicit disposition: keep-as-ability, split, move-to-task-type, merge/cross-domain, or knowledge-family;
- no canonical node contains mastery, automation, recent-training, stage/grade or review status;
- canonical IDs survive Notion recreation/renaming;
- domain and subdomain classify navigation/coverage, not progression;
- ability names use observable operations rather than vague topic labels where practical;
- strategy names express procedures rather than answer templates;
- modern reading, classical Chinese, poetry, writing, and language use all have valid examples.
