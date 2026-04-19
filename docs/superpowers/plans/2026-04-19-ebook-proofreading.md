# Proofreading Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the proofreading stub with a fully implemented SKILL.md that does a final spelling, punctuation, and formatting pass — producing Word Track Changes output with no content opinions.

**Architecture:** Reads `book-profile.md` (for locale). Produces `editing/proofread-<chapter>.docx`. Actively refuses content/style suggestions even if noticed.

**Tech Stack:** Markdown + YAML front-matter. Word output via MCP Office → python-docx → pandoc → .md fallback.

**Prerequisite:** Task 1 of `2026-04-19-ebook-publishing-plugin.md` must be complete (plugin scaffold).

---

## File Map

**Modify:**
- `ebook-publishing/skills/proofreading/SKILL.md` — replace stub with full implementation
- `ebook-publishing/README.md` — update status from 🚧 Stub to ✅ Ready

---

## Task 1: Implement proofreading skill

- [ ] **Step 1: Replace stub with full SKILL.md**

Write `ebook-publishing/skills/proofreading/SKILL.md`:

```markdown
---
name: proofreading
description: "Final proofread of near-complete fiction draft chapters: spelling, punctuation, typos, formatting consistency only. No content, style, or structural suggestions — actively declines if asked. Produces Word Track Changes output. Triggers: 'proofread', 'final proofread', 'check for errors', 'proof my chapter', 'spelling and punctuation check'."
---

# Proofreading

Final-pass error correction: spelling, punctuation, typos, formatting consistency. This skill makes no content, style, or structural suggestions — even if it notices them.

## Scope — strictly enforced

**In scope:**
- Spelling errors (including locale-specific: UK/US/AU from `book-profile.md`)
- Punctuation errors (missing/extra commas, incorrect apostrophes, inconsistent ellipsis format)
- Typos and transpositions
- Missing or doubled words
- Capitalisation errors
- Formatting inconsistencies (e.g., chapter headings styled differently, inconsistent italic usage for internal thought)

**Out of scope — actively decline if asked:**
- Grammar rewrites
- Sentence restructuring
- Content suggestions
- Style improvements
- Structural feedback
- POV corrections

If the author asks for content feedback during a proofreading session, respond:
> "That's outside the scope of proofreading. Run the `content-line-editing` skill for that feedback."

## Step 1 — Load book context

Read `book-profile.md` for locale (UK/US/AU). This determines correct spelling conventions.

Ask:
> "Which chapter(s) would you like me to proofread? Provide the file path(s)."

## Step 2 — Announce output method

Check and announce:
1. MCP Office tool (`mcp__office__*` or similar) → "Producing Track Changes .docx via Office MCP. Reviewer: Claude Proof."
2. `python -c "import docx"` succeeds → "Producing Track Changes .docx via python-docx. Reviewer: Claude Proof."
3. `pandoc --version` succeeds → "Producing .docx via pandoc. Corrections shown as [DELETED: x][INSERTED: y] blocks."
4. None → "No Word tooling found. Saving as .md with diff markup."

## Step 3 — Proofread

Work through the chapter. Record every error:
- **Location**: paragraph number or approximate position
- **Type**: spelling / punctuation / typo / missing word / doubled word / capitalisation / formatting
- **Original**: exact text
- **Correction**: corrected text

Do not add explanations beyond the correction type. No opinions on content.

## Step 4 — Produce output

**MCP or python-docx (Track Changes):**
- Create `editing/proofread-<chapter-name>.docx`
- Reviewer name: "Claude Proof"
- Mark corrections using Track Changes only — no comments

**pandoc:**
- Create `editing/proofread-<chapter-name>.docx`
- Show corrections as: `[DELETED: original][INSERTED: corrected]`

**Fallback .md:**
- Write `editing/proofread-<chapter-name>.md`
- Format: `~~original~~` `**corrected**`

At end of session, report:
- Total errors found
- Breakdown by type (spelling: N, punctuation: N, typos: N, etc.)
- Output file path
- No content opinions
```

- [ ] **Step 2: Verify frontmatter**

```bash
python -c "
import re
with open('ebook-publishing/skills/proofreading/SKILL.md') as f:
    content = f.read()
match = re.match(r'^---\n(.*?)\n---', content, re.DOTALL)
print('Frontmatter found:', bool(match))
"
```

Expected: `Frontmatter found: True`

- [ ] **Step 3: Commit**

```bash
git add ebook-publishing/skills/proofreading/SKILL.md
git commit -m "feat(ebook-publishing): implement proofreading skill"
```

---

## Task 2: Update README status

- [ ] **Step 1: Update status badge**

In `ebook-publishing/README.md`, change:

```
| `proofreading` | 🚧 Stub | Final spelling and punctuation pass (Word Track Changes) |
```

to:

```
| `proofreading` | ✅ Ready | Final spelling and punctuation pass (Word Track Changes) |
```

- [ ] **Step 2: Commit**

```bash
git add ebook-publishing/README.md
git commit -m "docs(ebook-publishing): mark proofreading as ready"
```
