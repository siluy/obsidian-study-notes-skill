---
name: study-notes
description: Generate exam-ready review notes from source materials (PDFs, PPTs, slides, books, technical docs). Produces a multi-file, Obsidian-flavored Markdown vault with a master overview, per-chapter notes, Mermaid diagrams, worked examples, and a problem set. Use this whenever the user asks to "make/generate review notes", "summarize lectures", "整理复习笔记", "做笔记", or hands over slide decks/PDFs and asks for revision/exam prep — even if they don't say "skill". Default behavior is exhaustive (deep, illustrated, problem-set included); a fast mode exists but should be confirmed before use.
---

# Study Notes

A workflow for turning lecture material (PDFs, PPTs, books, technical docs) into a complete, exam-ready review vault. The output is a directory of linked Markdown files written in Obsidian-flavored Markdown, designed so that a student who has never attended the class can still pass the exam by reading them.

## Core principle

PDF → text extraction is **lossy compression**. The slides have diagrams, B+-tree examples, ER charts, schedule traces, and architectural figures that disappear in the text. The skill's job is to **recover and exceed** that lost information — by re-deriving algorithms, drawing Mermaid diagrams, citing standard textbook examples (looked up online when needed), and adding worked problems. A faithful transcript of slide bullets is a failure mode; a synthesized, illustrated, exam-targeted reference is the goal.

## When to use

Trigger this skill when the user provides source materials (PDF lecture notes, slide decks, textbook chapters, course readings) and asks for:

- 复习笔记 / 整理笔记 / 做笔记 / 期末复习 / 考试复习
- review notes / revision notes / exam prep / study guide / cheat sheet
- "summarize these slides for me to study"
- "I have a final exam on X, help me prepare"
- "turn this PPT into notes I can revise from"

Also trigger when the user hands over a folder full of `note01.pdf`, `lecture_*.pdf`, `chapter_*.pdf`, etc. and clearly intends to study from it, even without explicit notes language.

**Do NOT use** for one-off summarization of a single article ("summarize this for me"), or for non-study contexts (meeting notes, code review notes). Those need different shapes.

## Decide the mode up front

Two modes. Confirm with the user briefly which one before starting — the cost difference is large.

| Mode | Output | When |
| --- | --- | --- |
| **Deep (default)** | Per-chapter notes + master overview + problem set. Mermaid diagrams. Worked examples derived from textbook standards. Multi-pass refine. **5-15× the cost of fast mode.** | The user wants exam-grade material; mention final exam, comprehensive prep, "深入"/"详尽"/"深入浅出". |
| **Fast** | Per-chapter notes only. Less aggressive figure recovery. Single pass. | User explicitly says "quick", "rough", "just give me the bullets", or has seen the cost and prefers it. |

Ask once: "Two modes — deep with diagrams + worked examples + problem set, or fast outline. Deep is the default; want me to go deep?" Don't over-confirm; one question is enough.

## Workflow

### Stage 1 — Survey the source material

1. List all source files. Verify page counts (`mdls -name kMDItemNumberOfPages` on macOS, or `pdfinfo`).
2. If PDFs are present, install `poppler-utils` if missing (`brew install poppler` on macOS, `apt-get install poppler-utils` on Linux). Use `pdftotext -layout` to extract text into a `text/` subdirectory parallel to the PDFs.
3. Read the extracted text of **every file** — at least the first 100 lines of each, preferably the whole thing if files are short. This is non-negotiable; you cannot make a faithful course map from titles alone.
4. Build a course map: what's the course about, what are the chapters, what's the scaffold the instructor uses (often there's a recurring "course overview" table on slide 2 of every deck — capture it).
5. Identify the **core algorithms / structures / theorems** that anchor each chapter. These are what the exam tests.

### Stage 2 — Choose layout and create the directory

Use a flat numbered layout in a `notes/` subdirectory:

```
<course-root>/notes/
├── 00-<course-name>-复习总览.md     (or "Overview" in English)
├── 01-<chapter-1-topic>.md
├── 02-<chapter-2-topic>.md
├── ...
├── NN-<last-chapter>.md
└── (NN+1)-练习题与解答.md           (deep mode only)
```

Numbering matches **logical** course order, not file order in the source — instructors often re-order chapters mid-course; follow the order they actually taught, not the chapter numbers in the textbook.

### Stage 3 — Draft each chapter (one pass)

See `references/chapter-template.md` for the full template. The shape per chapter:

1. **Frontmatter** with `title`, `chapter`, `pdf_ref` (which source file(s) this came from), `tags`, `prev`/`next` wikilinks.
2. **`> [!abstract]` 一句话概括** — the chapter's reason to exist, in one sentence.
3. **Section导图** — a Mermaid `graph TB` showing the topology of concepts in this chapter. This is the single most important figure; readers look at it first.
4. **Section bodies** — for each section, lead with a clear definition or theorem, follow with a worked example, end with the "why" (motivation/intuition).
5. **常考速查表** at chapter end.
6. **必练例题** — at least 2-3 worked examples with full solutions. In deep mode, prefer textbook-standard examples (look them up online if not in the slides).
7. **易错与考点** — the "trap" list. This is where you put gotchas the slides downplay.

### Stage 4 — Add diagrams and recover lost visuals (deep mode)

Slides contain figures that don't survive text extraction. **You must reconstruct them** — either by inferring from context, or by looking them up in standard references for that subject. See `references/mermaid-cookbook.md` for patterns:

- **Concept maps** → `graph TB` / `graph LR`
- **State machines** (transaction states, protocol states) → `stateDiagram-v2`
- **Data models / ER** → `erDiagram`
- **Algorithm flowcharts** → `flowchart`
- **Tree structures** (B+-trees, family trees) → `graph TB` with manual layout
- **Timelines** (history of standards, Turing awards) → `timeline`
- **Sequence interactions** (events, schedules) → `sequenceDiagram`

When the slide shows something visually that the text extraction can't capture (e.g., a B+-tree, an ER diagram, a query plan tree), **search online** for the standard textbook version — most courses use the same canonical figures (e.g., Silberschatz instructor name, Kurose-Ross, CLRS). Use the `general-purpose` Agent to do parallel web searches if many figures need recovery.

### Stage 5 — Refine pass (deep mode only)

After all chapters are drafted, do a refine pass:

1. **Walk every chapter again** with a critical eye. Replace ASCII art with Mermaid where it makes sense. Fill in algorithm pseudocode that was elided. Add cross-chapter wikilinks.
2. For the chapter most critical to the exam (often the densest one — indices, query optimization, concurrency control, etc.), add **completely worked examples with numbers** — the kind that show up on exams. Cite the source if it's a known textbook standard.
3. Build a **practice problem set** as the final file (`(NN+1)-练习题与解答.md`). Aim for 30-50 problems, organized by chapter, each with a full solution. Mix types: definition, computation, algorithm trace, design, judgment.
4. Update the overview (`00-...md`) with the final TOC and a Mermaid roadmap.

### Stage 6 — Hand over

When done, summarize for the user:

- **Where the notes live** (absolute path).
- **How many files / how many lines / approx KB**.
- **Suggested study order**: typically read the overview, then one chapter per day, then drill the problem set.
- **Any caveats** — e.g., "Chapter 7 only got partial coverage because the source slides ended there; the rest is from standard references."

## Output style guidelines

### Obsidian-flavored Markdown

The whole vault assumes Obsidian as a renderer. Use:

- `[[wikilinks]]` between files. Always link `prev`/`next` chapters and any cross-references.
- `> [!callout-type]` callouts: `important`, `warning`, `tip`, `info`, `success`, `danger`, `example`, `abstract`, `quote`.
- `tags:` array in frontmatter for cross-cutting topics.
- `#tag` inline if useful, but don't over-tag.
- Mermaid code blocks for all diagrams.

### Tone

Write to the student. Bilingual when the source is bilingual (e.g., Chinese course with English technical terms — keep both). Use ★ for "key concept" and 🔥 for "high-frequency exam target". Don't be afraid to use bold and tables liberally — this is reference material, not prose.

Explain the **why** behind each concept. Theorems without intuition are useless on an exam — students forget what they don't understand. Every algorithm should be paired with at least one motivating sentence ("This works because…").

### Things that mark a bad output

- Bullets that just paraphrase slide bullets without restructure.
- ASCII boxes where Mermaid would be clearer.
- Missing wikilinks between chapters.
- No worked examples with numbers.
- A "summary" at the end of each chapter that just lists section headers.
- Missing the chapter导图 (the per-chapter Mermaid topology).
- Theorem statements without intuition or why-it-matters.

## Common pitfalls

**Pitfall 1: Trusting the extracted text too much.** Text extraction loses tables, algorithms, and figures. Always cross-check with the actual PDF if a section's text feels incomplete or fragmented.

**Pitfall 2: Following source order blindly.** Slide decks often re-cap previous lectures or include filler "course overview" pages. Skip these in your output — the student doesn't need to see the same overview 13 times.

**Pitfall 3: Inventing details.** If you don't know a standard example or a figure looks ambiguous, look it up online (using general-purpose Agent). Don't make up textbook examples — cite them. If you can't verify, say so.

**Pitfall 4: Producing one giant file.** A 50,000-line single file is unusable. Split by chapter, link between them.

**Pitfall 5: Forgetting the master overview.** The 00- file is what the student opens first. It must contain: course map, chapter index, all key concepts, suggested study order, ACID-like quick-reference tables.

## Files in this skill

- `SKILL.md` — this file.
- `references/chapter-template.md` — the standard template for one chapter.
- `references/overview-template.md` — the standard template for the 00- master overview.
- `references/problem-set-template.md` — the standard template for the practice problem file.
- `references/mermaid-cookbook.md` — Mermaid patterns for common visual concepts.
- `references/checklist.md` — pre-handover quality checklist.

Read the relevant reference file before drafting that artifact.
