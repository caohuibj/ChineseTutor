# QA gates — F2 adaptive tutoring intervention engine

These are normative acceptance checks for Issue #20.

## Gate 1 — Diagnose before intervene
Fail if tutor sees wrong answer and immediately gives a generic method/explanation without identifying what is already correct and the likely causal gap.

## Gate 2 — Preserve correct work
A partial answer with valid evidence/conclusion must not be restarted from zero.

## Gate 3 — Earliest causal layer beats visible downstream symptom
If key word meaning is wrong and therefore translation fails, primary intervention targets K before sentence-level I/E.

## Gate 4 — Same final error can receive different help
Two equally wrong final answers may receive different interventions when one is Q, another R, another I.

## Gate 5 — One turn normally advances one layer
A tutor turn may acknowledge + isolate + ask, but should not simultaneously solve location, evidence, reasoning and wording.

## Gate 6 — One key question means one cognitive operation
Fail if a “single prompt” contains three independent questions that effectively dump the whole solution path.

## Gate 7 — Reasoning-correct / expression-weak routes to E
Do not reteach comprehension when oral/arrow reasoning is already valid.

## Gate 8 — Oral/arrow reasoning is valid diagnostic evidence
Informal wording must not be scored as failed reasoning solely because it is not exam prose.

## Gate 9 — Written normalization remains an endpoint
The system must eventually train complete written expression; oral scaffolding cannot become a permanent bypass.

## Gate 10 — Second attempt is default after repair
Tutor explanation alone does not count as learner learning. Normally request another learner response.

## Gate 11 — H-level is causal, not blanket penalty
H4 location help invalidates independent location evidence but can preserve downstream reasoning evidence.

## Gate 12 — Escalation is not H1→H7 ritual
Tutor may begin at the lowest directly relevant level and should re-check diagnosis before increasing support.

## Gate 13 — F1 hint ceiling is not a help prohibition
If learner needs more help than planned, tutor can exceed ceiling, logs it, and does not pretend move success criterion was met.

## Gate 14 — Fade after successful scaffold
A supported success should normally be followed by comparable demand with reduced support when independence/automation is the purpose.

## Gate 15 — Do not raise support and complexity together accidentally
Fade-scaffold move should not simultaneously raise text load, reasoning depth, openness and remove hints unless explicitly justified.

## Gate 16 — H6/H7 cannot masquerade as independent evidence
When tutor supplies the missing reasoning/near answer, same-item independent evidence for that supplied node is unrecoverable.

## Gate 17 — Model exposure can still teach
After H7, learner may explain/compare/repair, but independent verification requires a new sufficiently distinct item.

## Gate 18 — Productive struggle has a stopping rule
Do not repeat low-value prompts indefinitely. Explicitly teach when continued guessing/frustration has lower value than instruction.

## Gate 19 — K intervention stays local
If one missing word/rule blocks the task, teach or cue that prerequisite and return to the original operation; avoid unnecessary broad lecture.

## Gate 20 — C is not a garbage category
“粗心” requires evidence of a slip/self-correction pattern. Repeated failures trigger re-diagnosis as K/M/A/etc.

## Gate 21 — Diagnostic uncertainty uses a discriminator
When cause is unclear, ask a cheap question that separates hypotheses rather than choosing a heavy intervention prematurely.

## Gate 22 — Downstream not-observed is allowed
If prerequisite failure blocks the target operation, downstream node evidence can be `not_observed` rather than negative.

## Gate 23 — InterventionDecision links to next Attempt
Native repaired Attempts should reference the preceding F2 decision or retain equivalent audit linkage.

## Gate 24 — Actual support is snapshotted
C2 Attempt records actual hint level used, not only F1 planned ceiling.

## Gate 25 — Standardization timing is correct
Tutor-generated polished answer normally comes after learner logic is complete, unless explicit H6/H7 teaching mode is reached.

## Gate 26 — Standardization does not silently enrich meaning
A normalized answer may improve structure/terminology but may not introduce unsupported interpretation as though learner produced it.

## Gate 27 — Expression evidence reflects support
Learner-generated complete sentence can support E; tutor-supplied wording cannot count as independent E evidence.

## Gate 28 — Long writing is repaired locally when possible
Do not require/replace a full essay when one paragraph/scene/transition provides the needed training evidence.

## Gate 29 — Current Zhou Yafu scenario passes
Given correct special context + behavior + quality but missing warrant, choose one I-level guiding question, not a model answer.

## Gate 30 — Classical translation prerequisite case passes
If lexical meaning failure blocks translation, do not give negative translation-reasoning evidence until the sentence operation is actually observable.

## Gate 31 — Poetry label-only answer is diagnosed before teaching emotion
Ask for textual/imagery evidence first to distinguish R from I.

## Gate 32 — Intervention is auditable
Decision record must include after_attempt, diagnosis, preserved strengths, missing link, action family, H-level, target layer, rationale, expected next evidence.

## Gate 33 — F2 does not mutate Profile directly
Only C2 Attempts feed C3 Profile projection.

## Gate 34 — F2 does not re-rank global training targets
Changing to a prerequisite micro-probe inside a question is local tutoring control; cross-node next-best-training selection remains F1.

---

# Definition of done

- [x] InterventionDecision schema defined;
- [x] causal diagnosis policy defined;
- [x] state machine defined;
- [x] H0-H7 selection/escalation/fade policy defined;
- [x] F1 ceiling override semantics defined;
- [x] second-attempt and stop rules defined;
- [x] oral/arrow→written normalization policy defined;
- [x] model-exposure evidence boundary defined;
- [x] C2 audit-link amendment defined;
- [x] modern/classical/poetry/language/writing scenarios added;
- [ ] semantic self-review completed;
- [ ] stacked PR opened.