---
name: content-line-editing
description: "Use when a fiction draft chapter needs grammar, consistency, continuity, POV, style, or locale review. Does not address plot or structure."
---

# Content and Line Editing

Review and edit draft chapters for grammar, consistency, continuity, POV enforcement, style adherence, and locale accuracy. Output saved as `.md` with diff markup so the author can review each suggestion.

**Save all output files immediately on completion. Never ask the user for permission to save.**

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

Invoke the `folder-structure` skill. If chapter files are found outside `drafts/` or edited files outside `editing/`, resolve misplacements before requesting files from the user.

Check for `planning/writing-style.md`:
- If it **does not exist**: invoke the `writing-style` skill before proceeding. Pre-fill from `book-profile.md` context (locale, genre, tone). The line-editing that follows will reference the resulting style guide.

Ask:
> "Which chapter(s) would you like me to line edit? Provide the file path(s)."

Read the provided chapters.

Identify named characters in the chapters. For any character who speaks, acts, or is described with meaningful detail, check `planning/character-development.md`:
- If a profile is missing: invoke the `character-development` skill. Pre-fill appearance, name, and role from what the chapters reveal. Continuity checks in Step 2 will reference these profiles.

## Step 2 — Line edit

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

## Step 3 — Produce output

Write `editing/line-edit-<chapter-name>.md`:
- Format deletions as `~~deleted text~~` and insertions as `**inserted text**`
- List continuity errors and POV violations at end of file under `## Continuity / POV Issues`

Tell the user the output path and which format was used.

If `planning/writing-style.md` exists: offer to update it — patterns noticed during line editing (recurring POV choices, dialogue conventions, locale usage) may warrant additions or corrections.
