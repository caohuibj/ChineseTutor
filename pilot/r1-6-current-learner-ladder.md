# R1.6 — Current-learner minimum Knowledge / Task / authentic ladder

Status: **production pilot slice populated side-by-side; v1 remains source of truth**

Issue: #47

## 1. Purpose

R1.6 does not attempt exhaustive Gaokao Knowledge/TaskType population. It builds the smallest traceable content and recommendation slice needed to run the current learner through the native v2 evidence loop.

The starting evidence is deliberately narrow:

- `CN-A-evidence-to-character-judgment`: M1 / A1 / verified C2, weak legacy evidence;
- `CN-A-establish-comparison-dimension`: M1 / A1 / verified C2, weak legacy evidence;
- `CN-A-explain-comparison-significance`: M1 / A1, weak mixed legacy evidence, no verified complexity.

Unknown nodes are not interpreted as M0/A0.

## 2. Source boundary

The pilot spine uses the existing school learning design `八上-第二单元-“人物故事会” 学习设计.pdf`.

Traceable school materials include:

- 《藤野先生》;
- 《回忆鲁迅先生》;
- 《天上有颗“南仁东星”》;
- 《美丽的颜色》;
- 《周亚夫军细柳》;
- supplement 《最后一位戴罪的将军》;
- the Zhang Zhongxing genre-identification practice paragraph;
- the core writing task “让我难忘的他/她”.

No generated item is labeled authentic. Two pilot Questions are explicitly `adapted` because they isolate a diagnostic operation from an authentic school task.

## 3. Minimum canonical Knowledge addition

R1.5 already had memoir genre knowledge but the current school task requires memoir-vs-biography judgment. R1.6 therefore adds exactly one new canonical Knowledge node:

- `CN-K-biography-genre-features`

Production page: `3dcd2763-5a38-8154-b855-f9463404779c`

It softly `supports` `CN-A-judge-modern-genre-from-evidence`; it is not a global hard prerequisite.

Broader Knowledge population remains Issue #35.

## 4. Pilot TaskType inventory

R1.5 supplied:

1. `TT-character-analysis`
2. `TT-typical-event-characterization`
3. `TT-comparison-foil-function`

R1.6 adds eight reusable environments:

4. `TT-detail-effect-analysis`
5. `TT-structural-function`
6. `TT-genre-evidence-judgment`
7. `TT-title-meaning-function`
8. `TT-evidence-based-evaluation`
9. `TT-classical-contextual-word-sense`
10. `TT-writing-material-selection`
11. `TT-revision-choice-effect`

This is a compact task inventory, not one TaskType per question stem. Broader higher-order TaskType expansion remains Issue #34.

## 5. Material ladder

### Familiar / baseline

- `MAT-zhouyafu-school-occurrence`

Used for native low-hint baselines on already observed character/comparison operations.

### Same unit, varied surface

- `MAT-tengye-school-occurrence`
- `MAT-huiyiluxun-school-occurrence`
- `MAT-memoir-pair-school-bundle`

These vary event structure, detail density and comparison surface while preserving person-centered reading operations.

### Integrated biography

- `MAT-nanrendong-school-occurrence`
- `MAT-beautiful-colors-school-occurrence`
- `MAT-biography-pair-school-bundle`

These support two-text evidence/effect/title tasks.

### Short diagnostic / transfer surface

- `MAT-zhangzhongxing-genre-practice`
- `MAT-last-condemned-general-school-occurrence`

The first isolates genre judgment; the second provides a less familiar title/evaluation surface.

### Four-text complexity extension

- `MAT-memoir-biography-fourtext-bundle`

Used only after lower-complexity evidence exists.

### Reading → writing transfer

- `MAT-unforgettable-person-writing-prompt-school`

Preserves the actual school writing prompt/support while allowing an explicitly adapted material-selection diagnostic Question.

## 6. Question ladder

R1.6 adds 19 concrete Questions on top of the two R1.5 Zhou Yafu Questions, giving **21 pilot Questions**.

Representative progression:

```text
known operation / familiar material
  -> Zhou Yafu 4.6 H0 baseline

same operation / changed object or text surface
  -> Zhou Yafu 4.4 Han Wendi character
  -> Tengye 1.1
  -> Huiyi Luxun 2.1

new related operation / low-noise diagnostic
  -> Zhang Zhongxing genre judgment
  -> Zhou Yafu 4.2 selected contextual words (adapted)
  -> Huiyi Luxun 2.2 detail effect

integrated authentic school tasks
  -> biography 3.2 / 3.3 / 3.4
  -> Tengye 1.2 / 1.3 / 1.4(2)

transfer surface
  -> Lin Zexu title paradox
  -> memoir-pair comparison-significance adaptation

complexity extension
  -> four-text 3.5 evaluation

reading -> writing transfer
  -> “让我难忘的他/她” material-selection diagnostic (adapted)
```

### Authentic vs adapted

`school_provided + verbatim` is used only when the task is copied from the school learning design.

The following are intentionally `adapted`:

- `Q-zhouyafu-42-selected-contextual-words`: an eight-word subset of the much larger 4.2 annotation task;
- `Q-writing-unforgettable-person-material-selection`: isolates selection logic from the full 800-word composition;
- `Q-memoir-pair-compare-significance-adapted`: fixes one comparison dimension and adds a significance requirement to isolate the known comparison-significance gap.

Adapted items never count as verbatim authentic exam/school items.

## 7. ActiveRequirements

Seven production requirements were created.

### Known gaps

- character judgment -> target M2 / A2 / C2, moderate evidence, transfer required;
- comparison significance -> target M2 / A2 / C2, moderate evidence, transfer required;
- comparison dimension -> target M2 / A2 / C2, moderate evidence, transfer required.

### Unknown-state diagnostics

- detail effect;
- classical contextual word sense;
- genre judgment;
- writing material selection.

These are `diagnostic` or `school_sync` requirements because absence of evidence is not evidence of failure.

## 8. Training queue

Seven explainable TrainingMoves were created.

### Selected first move

`TM-r16-01-comparison-significance-h0-baseline`

- target: `CN-A-explain-comparison-significance`;
- secondary: `CN-A-establish-comparison-dimension`;
- Question: existing school Question `Q-zhouyafu-46-comparison-function`;
- move: `fade_scaffold`;
- target complexity: C2;
- max intended hint: H0;
- familiar material;
- **not** a transfer probe.

Reason: the legacy Attempt already shows correct comparison facts and terminology but an incomplete “difference -> highlighted target -> concrete significance” warrant. Repeating the familiar item once under native H0 conditions is useful for automation/dependence calibration, but cannot be counted as transfer evidence.

### Next proposed moves

1. comparison-significance transfer on memoir-pair variant;
2. independent character judgment with Han Wendi as the changed target;
3. detail-effect diagnostic;
4. classical word-sense diagnostic;
5. genre-judgment diagnostic;
6. writing material-selection diagnostic.

The queue is recomputed after native evidence; it is not a fixed worksheet order.

## 9. R1.6 exit state

```text
current_learner_content_slice = executable
selected_training_move = ready
question_supply = 21
active_requirements = 7
training_moves = 7
source_of_truth_switch = not_started
v1 = rollback-safe
```

R1.7 may now connect the current Chat tutoring interaction to the production Attempt / Intervention / Profile-update write path.

## 10. Non-closure of broad I1 gaps

R1.6 is only the MVP minimum slice. It does not close:

- #34 full higher-order TaskType inventory;
- #35 full Gaokao-supporting Knowledge families;
- #36 full authentic complexity ladders.

Those remain post-MVP/evidence-driven expansion work.