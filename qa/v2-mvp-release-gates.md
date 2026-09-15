# QA gates — ChineseTutor v2 MVP release readiness

These gates protect the transition from architecture work to real learner use.

## Stack convergence

### Gate 1 — Merge order respects semantic dependency
The merge train follows:

`#10 → #11 → #12 → #13 → #14 → #15 → #16 → #18 → #19 → #22 → #27 → #28 → #37`.

### Gate 2 — Child PRs are retargeted sequentially
Do not retarget the full stack at once. Retarget the next child to `main` only after its parent has merged.

### Gate 3 — Retarget diff is inspected
After each retarget, confirm the PR contains its own intended change and does not accidentally duplicate/drop parent semantics.

### Gate 4 — Root architecture remains high-level
Do not rewrite #10 to duplicate all later schemas. Only correct contradictions that would make the merged architecture misleading.

### Gate 5 — Issue completion follows accepted semantics
An issue is closed only after the corresponding merged PR still satisfies its acceptance criteria on `main`.

---

## MVP scope discipline

### Gate 6 — MVP is an operational learner loop
Release is not defined as “all Gaokao content finished”. It is defined as a trustworthy production Profile/Attempt/recommendation/intervention/review loop for the current learner.

### Gate 7 — Gaokao is coverage boundary, not launch prerequisite
Future/high-school graph population cannot block current use unless the missing node is an actual current target or prerequisite.

### Gate 8 — #31 is resolved before pilot
Evidence sufficiency / claim-validity evaluation must have canonical Ability semantics and cross-domain fixtures before MVP pilot.

### Gate 9 — #30 minimum semantics exist before pilot
Rule induction and rule application must be separable Profile outcomes before pilot; exhaustive material coverage may wait.

### Gate 10 — #29/#32/#33 are not accidental blockers
Representation transformation, question formulation and multi-constraint synthesis remain post-launch unless the pilot explicitly targets them.

### Gate 11 — #34 uses pilot-needed TaskTypes first
Only stable TaskTypes required by the pilot must exist pre-launch; future authentic forms may be added later.

### Gate 12 — #35 Knowledge population is sliced by active need
Current learner materials, target Questions and their prerequisites must be covered; exhaustive Gaokao Knowledge population is post-launch.

### Gate 13 — #36 has minimum active-node ladders
High-value pilot targets need routine/independent/transfer-or-integrated evidence opportunities where appropriate. The whole graph does not.

---

## Production data readiness

### Gate 14 — Canonical and learner-specific state are separate in production
LearningNode/edge semantics cannot contain personal mastery, automation, review or training state.

### Gate 15 — Content and learner history are separate
Material/TaskType/Question cannot contain `待练/已练/稳定` learner-specific status.

### Gate 16 — Attempt is the evidence ledger
Profile state changes must be traceable to TrainingAttempt/node observations or explicit reviewed legacy backfill.

### Gate 17 — Unknown remains different from weak
Migration cannot convert missing evidence into M0/A0/C0.

### Gate 18 — Split/merge backfill is conservative
A legacy mastery value cannot be copied blindly across multiple canonical child nodes or merged into M3 transfer evidence.

### Gate 19 — Historical reconstruction is labeled
Old sessions lacking native Attempt telemetry are reconstructed/low-confidence rather than invented as precise H-level sequences.

### Gate 20 — v1 survives pilot
No v1 database/field/history is deleted or irreversibly rewritten before dual-run acceptance.

### Gate 21 — Rollback is executable
There is a documented path to stop v2 writes and continue v1 operation without losing the learner's history.

### Gate 22 — Private learner data stays out of public GitHub
Fixtures are synthetic or appropriately de-identified; production learner records remain in the private operational system.

---

## Runtime readiness

### Gate 23 — Every session starts from a TrainingMove purpose
The tutor knows what node/evidence condition it is trying to establish, fade, automate, transfer-test or review.

### Gate 24 — Question eligibility is checked
A selected Question must not be used when a demonstrated hard prerequisite makes the intended target unobservable.

### Gate 25 — First response is preserved
The runtime must not overwrite the first answer with a polished second answer.

### Gate 26 — One causal layer per intervention by default
F2 should normally repair one earliest actionable gap before proceeding.

### Gate 27 — Expression-only failure does not trigger comprehension reteaching
If reasoning is already sound, intervention targets written expression conversion.

### Gate 28 — High support is not independent evidence
H6/H7/model exposure cannot be interpreted as H0/H1 mastery or automation evidence.

### Gate 29 — Retry creates new evidence
Tutor explanation alone cannot count as learner improvement; the learner normally answers again.

### Gate 30 — Profile update is auditable
Every accepted M/A/C or verification change has a ProfileUpdateDecision/equivalent rationale linked to evidence.

### Gate 31 — Write failure cannot silently mutate Profile
If persistence fails, the system must not claim a Profile update succeeded.

---

## Pilot readiness

### Gate 32 — Minimum native evidence volume
At least 20 native TrainingAttempts are captured before operational MVP declaration; 30 is preferred for normal-use confidence.

### Gate 33 — Pilot spans multiple domains
At least four task/domain families are represented, including modern reading and classical Chinese.

### Gate 34 — Pilot contains repair sequences
At least five first-answer → intervention → retry sequences are observed.

### Gate 35 — Scaffold fade/automation is exercised
At least one TrainingMove tests reduced cue dependence or fluent self-invocation.

### Gate 36 — Review policy is exercised or explicitly deferred by time
At least one real re-verification occurs when naturally due, or the pilot records that elapsed time was insufficient rather than manufacturing a fake review.

### Gate 37 — Evidence chain is complete
Each Attempt used for Profile evidence can be traced to Question, target node, support condition, observation and Profile update decision.

### Gate 38 — Same-variant repetition cannot fabricate transfer
M3/transfer claims require meaningful novelty/diversity, not repeated isomorphic items.

### Gate 39 — Recommendation supply gaps are surfaced
If no eligible Question exists, F1 reports a content-supply gap instead of repeatedly selecting an unsuitable item.

### Gate 40 — No unresolved release blocker remains
Any defect that causes unexplained Profile upgrades, false prerequisite blocking, evidence loss, destructive migration, or systematically wrong intervention is a NO-GO until repaired and re-tested.

---

# Definition of done for R1

- [x] explicit v2 MVP operational definition;
- [x] #10→#37 sequential merge train documented;
- [x] all #29–#36 gaps classified by launch criticality;
- [x] production data gates defined;
- [x] runtime tutoring gates defined;
- [x] pilot volume/domain/evidence gates defined;
- [x] rollback/dual-run requirement explicit;
- [x] clear v2 MVP finish line defined;
- [ ] release-readiness PR opened;
