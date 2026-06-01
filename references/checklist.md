# Pre-handover Quality Checklist

Run through this list before telling the user the notes are done. Each item that fails is a real defect — don't ship if more than 2 fail.

---

## Vault-level

- [ ] **Master overview file (00-…) exists and contains:**
  - [ ] Course-level frontmatter
  - [ ] 使用方式 callout
  - [ ] Mermaid roadmap of all chapters
  - [ ] Quick-reference tables (1-3 of the highest-yield ones)
  - [ ] 期末考试速复习清单 with wikilinks
  - [ ] Full TOC linking every chapter file
- [ ] **Chapter files numbered 01-NN** in logical course order
- [ ] **Problem set file ((NN+1)-…) exists in deep mode** with 30+ problems
- [ ] **All wikilinks resolve** — no orphan `[[...]]` references
- [ ] **prev/next chains** form a complete linked list

## Chapter-level (sample at least 3 chapters)

- [ ] Frontmatter has `title`, `chapter`, `pdf_ref`, `tags`, `prev`, `next`
- [ ] `> [!abstract]` 一句话概括 at the top
- [ ] **章导图** Mermaid present and accurate
- [ ] Section headings descriptive (not just "其他考虑" or "讨论")
- [ ] At least one Mermaid diagram per chapter (besides the章导图)
- [ ] At least 2-3 worked examples with **numerical** solutions
- [ ] **必背速查 / 应试速查表** at chapter end
- [ ] **易错与考点** section at chapter end
- [ ] No raw ASCII boxes that could be Mermaid

## Algorithm-heavy chapters (indices, query processing, concurrency, recovery)

- [ ] Pseudocode for every key algorithm
- [ ] Trace example for each algorithm — show the algorithm executing on a small input
- [ ] Cost formulas with worked numerical examples
- [ ] Edge cases enumerated (empty input, full buffer, etc.)

## Design chapters (ER, normalization)

- [ ] At least one full design example (user story → ER → relations)
- [ ] All conversion rules documented as a table

## Concept chapters (intro, transactions)

- [ ] All key terms have definitions
- [ ] Historical context (inventors, years) where relevant
- [ ] Architecture diagram

---

## Common defects to look for

| Defect | How to spot |
| --- | --- |
| **Slide-bullet transposition** | Section reads like a list of phrases instead of prose |
| **Missing why** | A theorem/algorithm appears with no explanation of motivation |
| **Algorithm without trace** | Pseudocode but no worked example |
| **Computation without numbers** | Formula stated but never plugged in |
| **Bad headings** | "其他", "讨论", "总结" |
| **ASCII art that should be Mermaid** | Box drawings, tree drawings |
| **Orphan files** | Files not linked from 00- |
| **Lonely chapters** | No prev/next, no cross-references |
| **Empty 易错与考点** | Section header but only a few generic items |

---

## Final smoke test

Pretend you've never seen the source material. Read just the 00- file. Can you:

1. Recite the spine of the course?
2. Name the 3-5 most important concepts?
3. Pick the chapter you'd start with for exam prep?

If yes — ship it. If no — the overview is too thin.

Then open chapter 1 (or whatever is "first"). Can you:

1. Get a one-sentence answer to "what is this chapter about"?
2. See the chapter topology immediately?
3. Find at least one worked example with numbers?

If yes — ship it. If no — the chapter spine is broken.

---

## What to tell the user when handing off

```
笔记已生成完毕，位于 <absolute path>。

📊 规模:
- <N> 个章节笔记 + 1 份总览 + 1 份练习题集（如适用）
- 共 <total lines> 行 / <KB> KB

🗺️ 推荐学习顺序:
1. 先读 [[00-...]] 总览，建立全局认知
2. 按章节顺序精读
3. 用 [[(NN+1)-练习题与解答]] 自测

⚠️ 注意:
- <any caveats: chapters with thin coverage, missing source material, etc.>
```
