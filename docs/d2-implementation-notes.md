# D2 implementation notes

D2 is stacked on D1 and is intentionally schema-first.

Implementation order after merge:

1. keep legacy difficulty labels as migration metadata;
2. assign provisional vectors only to Questions whose prompt/material version is known;
3. review representative items across domains and calibrate reviewers;
4. promote to `complexity_status=reviewed` only with rationale;
5. use full vector in recommendation rather than only C-band;
6. never infer learner transfer from C-band; use C2 Attempt evidence.

The `profile/d2-complexity-amendment.md` file records the compatibility correction to the earlier C1 shorthand. When stacked PRs are consolidated, the canonical C1 schema text should be edited to use D2 band labels directly.