# Content and Line Editing Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the content-line-editing stub with a fully implemented SKILL.md that line edits fiction chapters for grammar, consistency, continuity, POV enforcement, and style — producing Word Track Changes output.

**Architecture:** Reads `book-profile.md` + `planning/writing-style.md` + draft chapters. Produces `editing/line-edit-<chapter>.docx` with Track Changes. Four-level tooling fallback.

**Tech Stack:** Markdown + YAML front-matter. Word output via MCP Office → python-docx → pandoc → .md fallback.

**Prerequisite:** Task 1 of `2026-04-19-ebook-publishing-plugin.md` must be complete (plugin scaffold).

---

## File Map

**Modify:**
- `ebook-publishing/skills/content-line-editing/SKILL.md` — replace stub with full implementation
- `ebook-publishing/README.md` — update status from 🚧 Stub to ✅ Ready

---

## Task 1: Implement content-line-editing skill

- [ ] **Step 1: Replace stub with full SKILL.md**

Write `ebook-publishing/skills/content-line-editing/SKILL.md`:

```markdown
---
name: content-line-editing
description: "Line edit fiction draft chapters: grammar, consistency, continuity, POV enforcement, style adherence, locale accuracy. Produces Word Track Changes output. Does NOT make structural changes. Triggers: 'line edit', 'content edit', 'edit my chapter', 'check consistency', 'copy edit'."
---

# Content and Line Editing

Review and edit draft chapters for grammar, consistency, continuity, POV enforcement, style adherence, and locale accuracy. Produces output with Track Changes so the author can accept or reject each suggestion.

## Scope — strictly enforced

**In scope:**
- Grammar and syntax errors
- Internal consistency (e.g., eye colour changing between chapters)
- Continuity errors (timeline, object placement, character knowledge)
- POV violations (knowing things the POV character can't know; describing the POV character's own face without a mirror)
- Style guide adherence (check against `planning/writing-style.md` if present)
- Locale accuracy (UK/US/AU spelling and idiom — from `book-profile.md` locale field)
- Sentence-level clarity and flow
- Word choice and repetition (same word 3+ times on one page)
- Dialogue punctuation

**Out of scope — do not comment on:**
- Plot holes or structural issues (structural-editing handles those)
- Basic spelling and punctuation (proofreading handles those)
- Whether the story is compelling

## Step 1 — Load book context

Read `book-profile.md` and `planning/writing-style.md` (if present). Note: genre, locale, POV mode, tense, voice characteristics.

Ask:
> "Which chapter(s) would you like me to line edit? Provide the file path(s)."

Read the provided chapters.

## Step 2 — Announce output method

Check and announce:
1. MCP Office tool (`mcp__office__*` or similar) → "Producing Track Changes .docx via Office MCP. Reviewer: Claude Edit."
2. `python -c "import docx"` succeeds → "Producing Track Changes .docx via python-docx. Reviewer: Claude Edit."
3. `pandoc --version` succeeds → "Producing .docx via pandoc. Edits shown as [DELETED: x][INSERTED: y] blocks — not native Track Changes."
4. None → "No Word tooling found. Saving as .md with diff markup."

## Step 3 — Line edit

Work through each chapter systematically. For every issue found, record:
- **Location**: chapter name/number, paragraph position
- **Type**: grammar / consistency / continuity / POV / style / locale / clarity / repetition
- **Original text**: exact quote
- **Suggested text**: the replacement
- **Reason**: one sentence (e.g., "POV violation — narrator can't see their own expression", "UK locale — 'realized' → 'realised'")

Track separately:
- **Continuity error log**: all consistency/continuity issues (character details that change, timeline problems, object tracking)
- **POV violation log**: all POV-related issues
- **Inline edits**: all grammar, style, clarity, locale edits

## Step 4 — Produce output

**MCP or python-docx (Track Changes):**
- Create `editing/line-edit-<chapter-name>.docx`
- Reviewer name: "Claude Edit"
- Mark deletions and insertions using Track Changes
- Add continuity errors and POV violations as Word Review Comments on the relevant passage

**pandoc:**
- Create `editing/line-edit-<chapter-name>.docx`
- Show edits as: `[DELETED: original text][INSERTED: suggested text]`
- Add continuity/POV issues as `[NOTE: issue description]` inline

**Fallback .md:**
- Write `editing/line-edit-<chapter-name>.md`
- Format: `~~deleted text~~` and `**inserted text**`
- List continuity/POV issues at end of file

Tell the user the output path and which format was used.
```

- [ ] **Step 2: Verify frontmatter**

```bash
python -c "
import re
with open('ebook-publishing/skills/content-line-editing/SKILL.md') as f:
    content = f.read()
match = re.match(r'^---\n(.*?)\n---', content, re.DOTALL)
print('Frontmatter found:', bool(match))
"
```

Expected: `Frontmatter found: True`

- [ ] **Step 3: Commit**

```bash
git add ebook-publishing/skills/content-line-editing/SKILL.md
git commit -m "feat(ebook-publishing): implement content-line-editing skill"
```

---

## Task 2: Update README status

- [ ] **Step 1: Update status badge**

In `ebook-publishing/README.md`, change:

```
| `content-line-editing` | 🚧 Stub | Grammar, consistency, POV, style (Word Track Changes) |
```

to:

```
| `content-line-editing` | ✅ Ready | Grammar, consistency, POV, style (Word Track Changes) |
```

- [ ] **Step 2: Commit**

```bash
git add ebook-publishing/README.md
git commit -m "docs(ebook-publishing): mark content-line-editing as ready"
```
