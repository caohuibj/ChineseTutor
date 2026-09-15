# D2 compatibility amendment for C1 Learner Profile

C1 was drafted before the final task-complexity model and contains an older shorthand:

```text
C4 = unfamiliar transfer
```

D2 supersedes that line.

The profile field remains:

```text
verified_complexity_band: null | C0 | C1 | C2 | C3 | C4 | C5
```

but its meaning is now:

> highest intrinsic Question complexity band at which the learner has sufficiently credible successful evidence for this node.

Canonical D2 bands:

```text
C0 recognition/direct retrieval
C1 single-step application
C2 multi-step explanation
C3 integrated multi-node/multi-source synthesis
C4 high integration / remote application / constrained evaluation
C5 open evaluation / complex construction
```

Transfer/generalization remains an independent Mastery-evidence condition:

- C1 `M3` still requires stable generalization/transfer;
- C2 Attempt records `material_familiarity`, `transfer_probe`, variant diversity and hint dependence;
- no particular C-band proves transfer;
- unfamiliar success on a C2 task can be valid M3 evidence when sufficiently diverse and stable;
- familiar success on a C4 task is not automatically transfer evidence.

This amendment should be applied when the stacked PRs are consolidated into main documentation.