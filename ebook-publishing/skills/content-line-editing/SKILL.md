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
