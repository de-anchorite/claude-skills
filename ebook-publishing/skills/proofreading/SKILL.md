---
name: proofreading
description: "Use when a fiction chapter is near-final and needs spelling, punctuation, typo, and formatting checks only. Not for content or style feedback."
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
2. `pandoc --version` succeeds → "Producing .docx via pandoc. Corrections shown as [DELETED: x][INSERTED: y] blocks."
3. None → "No Word tooling found. Saving as .md with diff markup."

## Step 3 — Proofread

Work through the chapter. Record every error:
- **Location**: paragraph number or approximate position
- **Type**: spelling / punctuation / typo / missing word / doubled word / capitalisation / formatting
- **Original**: exact text
- **Correction**: corrected text

Do not add explanations beyond the correction type. No opinions on content.

## Step 4 — Produce output

**MCP (Track Changes):**
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
