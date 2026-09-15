# B1 review summary

Issue: #1 — Define canonical LearningNode schema and taxonomy

## Implemented

- stable `CN-<TYPE>-<DOMAIN>-<SLUG>` ID contract;
- canonical node types: Knowledge / Ability / Strategy;
- required/optional/forbidden fields;
- domain/subdomain taxonomy covering language, modern reading, classical Chinese, poetry, literature/culture, writing, communication and cross-domain reasoning;
- split/merge/generalization rules;
- naming conventions;
- representative canonical node fixtures across major domains;
- explicit migration disposition for all 42 current v1 ability-map rows;
- semantic QA gates;
- ADR for canonical LearningNode boundaries.

## Important decisions to review

1. Use one canonical registry with `node_type`, rather than independent semantic identities per database.
2. Keep `domain` as navigation/coverage, never progression.
3. Generalize identical reasoning across domains while retaining domain-specific decoding prerequisites.
4. Preserve broad v1 concepts only as reporting composites when useful; canonical leaves must remain diagnosable.
5. Move Task Type labels out of LearningNode when the label mainly describes how an exam asks rather than what a learner knows/does.
6. Do not promote every task algorithm to Strategy; only reusable mother procedures qualify.

## Deliberately deferred

- dependency-edge schema (`requires`, `supports`, etc.) — B2;
- live Notion migration/backfill;
- LearnerNodeState/Profile — C1;
- TrainingAttempt evidence — C2;
- exhaustive Gaokao graph population;
- Strategy consolidation — G1.

## B1 acceptance status

The implementation satisfies the semantic acceptance criteria in Issue #1. Live operational migration remains intentionally outside B1.
