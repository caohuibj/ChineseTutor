# Retention-family resolution

Status: **F3 normative draft**

`retention_family` is a scheduling-policy classification, not a fourth LearningNode semantic type and not learner state.

F3 resolves it from canonical node semantics plus optional explicit overrides.

---

## 1. Resolution precedence

Use the first applicable rule:

1. **Explicit node-level retention override** approved in policy metadata.
2. **Subdomain/family policy mapping** based on canonical semantics.
3. **Node-type default** (`knowledge`, `ability`, `strategy`).
4. `other` if classification remains uncertain.

The resolved value may be cached on `LearnerNodeState` for query performance, but the cache is not authoritative.

---

## 2. Typical semantic mapping

### `recitation`
Use for exact memorized production where wording/characters/order are part of success.

### `exact_retrieval`
Use for lexical meanings, terminology, cultural facts, fixed distinctions, and other compact retrieval constructs where exact access is central.

### `conceptual_knowledge`
Use for knowledge whose success is better judged through explanation, distinction and application than exact wording.

### `reasoning_ability`
Use for observable operations such as evidence integration, inference, comparison and evaluation that are best maintained through representative tasks.

### `strategy`
Use for reusable procedures where both correct execution and self-trigger/automation matter.

### `writing_production`
Use for composition/revision abilities best evidenced through produced artifacts.

---

## 3. Mixed-looking constructs

If a legacy label appears to require multiple retention families, do not create a mixed family. Revisit B1 decomposition.

Example:

```text
“文言实词”
```

should not be one monolithic review object if it hides:

- lexical meaning Knowledge (`exact_retrieval`), and
- contextual sense inference Ability (`reasoning_ability`).

This is another reason B1 split Knowledge from Ability.

---

## 4. Axis behavior

One resolved family can still use different axis horizons.

Example Strategy:

- mastery may remain fresh relatively long;
- automation/self-trigger can become review-due earlier.

If one node repeatedly needs radically different family semantics by axis, define an explicit axis override in policy configuration rather than duplicating the LearningNode.

---

## 5. Learner-specific calibration

The canonical/policy family identifies the evidence shape. Learner-specific scheduling may later modify horizon parameters based on observed retention history.

Do not encode individual retention speed into canonical graph nodes.

---

## 6. Invariants

1. Retention family never determines grade progression.
2. Retention family does not change LearningNode identity.
3. Legacy mixed labels should be decomposed rather than assigned an ambiguous family.
4. Learner-specific timing belongs to policy/profile calibration, not canonical semantics.
5. A cached family value is always replaceable by recomputation from the current policy version.