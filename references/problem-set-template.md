# Problem Set Template

The final file in deep-mode vaults. The point: **let the student walk in cold and pass the exam by working through this file**.

---

## Spine

1. **Frontmatter** (similar to chapter notes)
2. **Title + 使用方式 callout**
3. **题目分类索引** (table of contents by topic)
4. **题目主体** (problems organized by chapter)
5. **应试技巧** at the end (test-taking strategy section)

---

## Frontmatter

```yaml
---
title: 综合练习题与解答
chapter: 全书
tags:
  - <subject>
  - 练习题
  - 期末复习
prev: "[[<last-chapter>]]"
next: "[[00-<course-name>-复习总览]]"
---
```

---

## 使用方式 callout

```markdown
> [!abstract] 使用方式
> 本笔记按章节归类常见考题，**先尝试自己做，再看解答**。每道题标注章节链接，便于回查。
```

---

## 题目分类索引

```markdown
| 类别 | 题目 |
| --- | --- |
| 关系代数 | [Q1](#Q1) – [Q5](#Q5) |
| SQL | [Q6](#Q6) – [Q12](#Q12) |
| 索引（B+ 树/哈希） | [Q13](#Q13) – [Q18](#Q18) |
| ... | ... |
```

This lets the student jump to a topic quickly. Use HTML anchors `<a name="Q1"></a>` if Markdown anchors don't work in the target Obsidian setup.

---

## Problem format

Each problem follows this format:

```markdown
## Q<n>: <One-line topic>

> Problem statement, with concrete numbers.
> If based on a setup defined earlier (e.g. a schema), reference it via wikilink.

**解**:

Step-by-step solution. Show the math. Cite the relevant rule/formula.

If the problem has multiple parts, do (a), (b), (c) explicitly.
```

### Good problem traits

- **Numbers, not abstractions**. "Compute the cost of A2 with $h_i=4$, $b_r=4000$" beats "compute the cost in the general case".
- **Solution explains the why**. Don't just state the answer; show why the formula applies.
- **Multi-part where useful**. Real exam questions often build: (a) compute X, (b) compute Y, (c) compare.
- **At least one trap problem per chapter**. Something where the obvious answer is wrong.

### Bad problem traits

- Problems where the answer is just restating a definition. Test understanding, not memory.
- Problems where the solution doesn't use the chapter's key technique. Filler.
- "Trivia" problems (year of standard, name of inventor). Unless the course explicitly tests that, skip.

---

## Coverage targets

Aim for the following coverage in deep mode:

| Chapter type | # problems |
| --- | --- |
| Light/conceptual chapter | 3-5 |
| Algorithm-heavy chapter | 6-10 |
| The hardest chapter (often the exam's centerpiece) | 8-12 |

A typical 12-chapter course should have **30-50 problems** in this file. Fewer than 25 is too thin; more than 60 dilutes attention.

---

## Problem types to include

Mix problem types deliberately. A chapter that gets all of one type is a smell that you missed something.

| Type | Description |
| --- | --- |
| **Definition** | "What is X? Give one example." |
| **Computation** | "Compute the cost of …" with numbers. |
| **Algorithm trace** | "Show the state of the B+-tree after inserting Adams." |
| **Design** | "Convert this user story to an ER diagram." |
| **Judgment** | "Is the following schedule recoverable? Cascadeless?" |
| **Compare** | "Why is BCNF preferred over 3NF here?" |
| **Trap** | One per chapter — looks easy, has a gotcha. |

---

## Final section: 应试技巧 / Test-taking strategy

End with a coda that:

- Identifies the highest-yield 2-3 chapters
- Lists common pitfalls across chapters (e.g., "students always confuse multitable clustering with clustering index")
- Suggests time allocation for the actual exam
- Recommends a final-night-before drill: "30 min reading the overview + skim 必背 sections + check answers to 10 random Qs"

---

## Pitfalls in writing the problem set

- **Problems too easy.** If every problem can be solved in 30 seconds, you've written flashcards, not exam prep.
- **Solutions that handwave.** "By the formula, the answer is 204 ms" is bad. "$\text{Cost} = 1 \cdot t_S + (b_r/2) \cdot t_T = 1 \cdot 4 + 2000 \cdot 0.1 = 204$ ms" is good.
- **No traps.** Real exams have traps. So should the problem set.
- **No coverage of the chapter's key technique.** Each problem should exercise the chapter's main tool.
- **Forgetting the index at the top.** Without it, the file is a wall of text.
