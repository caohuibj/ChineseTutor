# Legacy difficulty migration to D2

Current v1 data uses labels such as:

```text
基础 / 常规 / 提升 / 迁移
```

These are useful editorial shorthand but are not semantically equivalent to D2 complexity bands.

## Core rule

Do not map automatically:

```text
基础 -> C0/C1
常规 -> C2
提升 -> C3/C4
迁移 -> C4
```

The old labels combine intrinsic task demand, learner novelty, expected school level, and editorial judgment.

## Migration policy

For every migrated Question:

1. preserve the original label in `legacy_difficulty_label` or migration notes;
2. initialize `complexity_status = provisional`;
3. score the eight-vector from the actual prompt/material/rubric;
4. assign reviewed C-band only after vector review;
5. move learner-relative novelty/transfer meaning to C2 Attempt (`material_familiarity`, `transfer_probe`);
6. never use source grade as an automatic band proxy.

## Special handling of `迁移`

`迁移` is especially ambiguous.

It may mean:

- unfamiliar material using the same ability;
- a cross-domain application;
- a harder question;
- a teacher-designated extension item.

Therefore it should migrate as a **curation note / transfer-probe candidate hint**, not as C4.

Example:

```text
legacy difficulty: 迁移
actual task vector: C2 multi-step explanation
learner condition: unfamiliar + H0
```

This is valid and often pedagogically desirable: transfer can be tested while holding intrinsic complexity constant.

## Promotion rule

A candidate Question becomes `complexity_status=reviewed` only when:

- prompt/material version is stable;
- all applicable vector dimensions are scored;
- rationale is recorded;
- source grade has not been used as the decisive argument;
- novelty/transfer has not been encoded into the canonical band.
