# F2 response normalization policy

Status: **normative draft**

ChineseTutor explicitly separates:

```text
想对没有
vs
说得好不好
```

The system should allow oral, fragmentary and arrow-style reasoning while diagnosing understanding, then deliberately train conversion into Grade-appropriate written Chinese.

This policy prevents two symmetric errors:

1. treating weak prose as weak thinking;
2. accepting good thinking forever without training written expression.

---

## 1. When to switch into normalization mode

Use `expression_conversion` when all are sufficiently true:

- the core conclusion is valid;
- evidence is relevant/sufficient for the current task;
- the reasoning relation is conceptually present;
- remaining weakness is organization, explicit connective logic, terminology, concision or sentence completeness.

Do **not** normalize yet when the reasoning itself is still missing or invalid.

---

## 2. Oral / arrow diagnostic mode

When prose quality obscures thinking, ask for the cheapest representation of the logic:

```text
证据 → 为什么特别 → 品质
```

or:

```text
原文事实
→ 发生在什么情境
→ 人物做了什么选择
→ 为什么有意义
```

or:

```text
关键词
→ 词义
→ 句内关系
→ 整句意思
```

This is a diagnostic and thinking scaffold, not the final endpoint.

---

## 3. Normalization algorithm

Use the learner's own correct content as source material.

```text
1. 保留结论
2. 保留最关键证据
3. 补出被省略的逻辑连接
4. 调整主语/指代/因果关系
5. 必要时加入准确术语
6. 删除重复和口头填充
7. 回扣题目要求
```

Do not add a more sophisticated interpretation merely because the tutor knows one.

---

## 4. Student-first conversion levels

### N0 — ask learner to restate one complete sentence
No sentence frame yet.

### N1 — minimal logical skeleton
Example:

```text
因为……，所以……，这说明……
```

or:

```text
作者写……，放在这里与……形成……，从而……
```

### N2 — provide ordering, learner fills content
Example:

```text
先结论 → 再证据 → 再解释
```

### N3 — tutor offers normalized version after learner has already supplied the full reasoning
This is model comparison / consolidation, not first-response substitution.

Use the lowest level that enables the learner to produce a valid answer.

---

## 5. Standard answer timing

A polished answer may be shown when:

- the learner has completed the missing logic themselves; or
- H6/H7 teaching mode has been reached and the instructional purpose requires a model; or
- the task is being closed after sufficient attempts.

If the tutor shows a model before the target reasoning is independently attempted, that content cannot later count as independent evidence on the same item.

---

## 6. What “规范表达” means

It does not mean making language artificially ornate.

For reading short answers, prioritize:

- answer the exact question;
- retain textual evidence;
- make the reasoning relation explicit;
- use accurate terminology;
- use complete, concise sentences.

For current learner adaptation, an acceptable progression is:

```text
口语逻辑正确
→ 箭头逻辑完整
→ 一句完整得分句
→ 多句有层次答案
```

Final evaluation remains at the current school/Grade-8 answer standard; scaffolding does not lower the endpoint.

---

## 7. Specific feedback rule

When normalizing, feedback should identify the exact conversion achieved.

Good:

> 这次不是观点变了，而是你把“天子亲自劳军”这个特殊情境补进来，所以“拒绝天子”为什么能说明刚正不阿就成立了。

Good:

> 你的推理刚才已经完整，现在只把口语里的三层关系合成一句书面答案。

Avoid generic praise that hides the learning action.

---

## 8. Expression evidence

Normalization creates different evidence depending on support.

- learner independently converts oral logic to complete written answer → positive written-expression evidence;
- learner fills a minimal skeleton → partial/guided expression evidence;
- tutor supplies final wording → model exposure, not independent written-expression evidence.

The underlying reasoning Attempt can remain valid even when expression support is added later, if the reasoning was already observed before wording was supplied.

---

## 9. Writing artifacts

For composition, normalize locally rather than rewriting the whole essay by default.

Prefer:

- one paragraph restructuring;
- one scene expansion;
- one transition repair;
- one evidence-analysis paragraph;
- one sentence cluster revision.

Use versioned artifacts so improvement is visible as learner-produced change rather than tutor replacement.

---

## 10. Invariants

1. Diagnose reasoning before judging prose quality.
2. Oral/arrow form is allowed as a thinking representation, not as the final endpoint.
3. Correct thinking must eventually be trained into independent written expression.
4. Normalization uses learner-derived meaning; it does not silently enrich the answer.
5. Minimal sentence skeleton precedes full model when possible.
6. Model wording is teaching input, not same-item independent expression evidence.
7. Feedback names the exact logical/expression improvement.