# ChineseTutor

ChineseTutor is a graph-first Chinese-language learning system oriented toward complete Gaokao knowledge and capability coverage.

The project models learning as a dynamic loop across:

- a canonical learning graph (knowledge, abilities, reusable strategies, and dependencies),
- authentic tasks and source materials,
- learner-state profiles backed by real attempt evidence,
- adaptive task recommendation,
- minimal-effective-intervention tutoring,
- transfer validation and spaced review.

Grade level is source metadata and a load-control signal, not the primary organization axis. Training progression is driven by prerequisites, learner state, task complexity, and transfer evidence.

## Design documents

- [`docs/current-state-audit.md`](docs/current-state-audit.md) - what already exists and what the refactor must preserve.
- [`docs/architecture-v2.md`](docs/architecture-v2.md) - target graph/profile/evidence architecture.
- [`docs/notion-refactor-plan.md`](docs/notion-refactor-plan.md) - non-destructive migration design for the operational workspace.
- [`docs/roadmap.md`](docs/roadmap.md) - staged PR/commit implementation roadmap.
- [`docs/decisions/`](docs/decisions/) - architecture decision records.
- [`qa/architecture-gates.md`](qa/architecture-gates.md) - gates for semantic separation, evidence quality, source traceability and Gaokao coverage.

The current v2 architecture work is intentionally documentation-first. Live schema migration follows only after the model and acceptance criteria are reviewed.
