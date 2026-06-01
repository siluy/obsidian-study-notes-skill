# obsidian-study-notes

A [Claude Code](https://claude.com/claude-code) **Skill** that turns lecture material (PDFs, slide decks, books, technical docs) into a complete, exam-ready review vault written in [Obsidian](https://obsidian.md)-flavored Markdown.

The output is a directory of linked Markdown files — master overview, per-chapter notes with Mermaid diagrams, worked examples, and a problem set — designed so a student who has never attended the class can still pass the exam by reading them.

## Why

PDF text extraction is **lossy compression**. Slides have B+-tree diagrams, ER charts, schedule traces, and architectural figures that disappear in the text. Bullet-by-bullet transcription produces unusable notes. This skill explicitly recovers and exceeds the lost information — by re-deriving algorithms, drawing Mermaid diagrams, citing standard textbook examples (looked up online when needed), and adding worked problems.

A faithful slide transcript is a failure mode; a synthesized, illustrated, exam-targeted reference is the goal.

## What it produces

```
notes/
├── 00-<course-name>-复习总览.md      # Master overview with Mermaid roadmap
├── 01-<chapter>.md                   # Each chapter has:
├── 02-<chapter>.md                   #   - chapter导图 (Mermaid topology)
├── ...                               #   - section bodies with worked examples
├── NN-<chapter>.md                   #   - 必背速查 quick-reference
└── (NN+1)-练习题与解答.md            # 30-50 worked problems by chapter
```

Each chapter file includes:

- YAML frontmatter (`title`, `chapter`, `pdf_ref`, `tags`, `prev`, `next`)
- `> [!abstract]` 一句话概括 (single-sentence chapter purpose)
- Chapter topology diagram (Mermaid `graph TB`)
- Section bodies with: definition → worked example → why/intuition
- 必练例题 with **numerical** worked solutions
- 易错与考点 (gotchas and exam targets)
- Wikilinks to adjacent chapters

## When the skill triggers

The skill auto-triggers when you ask Claude to:

- 复习笔记 / 整理笔记 / 做笔记 / 期末复习 / 考试复习
- "review notes" / "revision notes" / "exam prep" / "study guide"
- "summarize these slides for me to study"
- Hand over a folder of `note01.pdf`, `lecture_*.pdf`, etc.

It does **not** trigger for one-off article summarization or non-study contexts.

## Modes

| Mode | Output | Cost |
| --- | --- | --- |
| **Deep (default)** | Per-chapter notes + master overview + problem set + Mermaid diagrams + worked examples | high |
| **Fast** | Per-chapter notes only, single pass | low |

The skill confirms which mode you want before starting.

## Installation

### As a Claude Code skill

Clone into your `~/.claude/skills/` directory:

```bash
cd ~/.claude/skills/
git clone https://github.com/<your-username>/obsidian-study-notes.git study-notes
```

Then run `/reload-skills` in Claude Code, or restart Claude Code.

### As a directory

Drop the `study-notes/` directory anywhere your Claude Code session can see and reference it manually.

## What's inside

```
study-notes/
├── SKILL.md                              # The skill itself
└── references/
    ├── chapter-template.md               # Standard shape for one chapter
    ├── overview-template.md              # Standard shape for the 00- master file
    ├── problem-set-template.md           # Standard shape for the practice problem file
    ├── mermaid-cookbook.md               # Mermaid patterns for visual concepts
    └── checklist.md                      # Pre-handover quality checklist
```

The `references/` files are loaded on demand by Claude when drafting the relevant artifact.

## Example

Given a folder of 13 PDF lecture slides on Database Systems (~13,000 slides text, ~250 pages of figures), this skill produces:

- A **220 KB / 8000-line vault** of 14 linked Markdown files
- **Mermaid diagrams** for B+-tree structures, ER models, transaction state machines, recovery flows, etc.
- **45 worked problems** covering relational algebra, SQL, indexing, query processing, transactions, normalization, ER design
- **Quick-reference tables** for ACID, isolation levels, normal forms, equivalence rules
- **Cross-chapter wikilinks** so concepts are navigable

A student can study from the vault alone without ever touching the original PDFs.

## Acknowledgements

Built with [Claude Code](https://claude.com/claude-code)'s [skill-creator](https://github.com/anthropics/skills) workflow.

Inspired by reviewing the original notes flow developed for [Database Systems Concepts (Silberschatz)](https://www.db-book.com).

## License

MIT
