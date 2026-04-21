---
name: structural-editing
description: "Big-picture developmental edit of a fiction draft: plot holes, pacing, structure, character arc consistency, scene necessity. No prose or grammar feedback. Produces Word report. Triggers: 'structural edit', 'big picture edit', 'developmental edit', 'review my plot structure', 'story edit'."
---

# Structural Editing

Review your draft for big-picture issues. This skill does not comment on prose, grammar, or spelling — those belong to content-line-editing and proofreading.

## Scope — strictly enforced

**In scope:**
- Plot logic and consistency
- Story structure and act balance
- Pacing (too fast, too slow, uneven)
- Character arc consistency across chapters
- Scene necessity (does every scene move the plot or deepen character?)
- Subplot integration and resolution
- POV consistency at scene level
- Thematic coherence

**Out of scope — do not comment on:**
- Grammar, spelling, punctuation
- Sentence-level prose quality
- Word choice or dialogue tags

If you notice a prose issue, note it only as "this chapter may benefit from line editing" — do not make the specific correction.

## Step 1 — Load book context

Read `book-profile.md`, `planning/story-development.md` (if present), and `planning/chapter-planning.md` (if present).

Ask:
> "Which draft files should I review? You can give me specific chapter files, a full manuscript, or your planning documents."

Read all provided files.

## Step 2 — Announce output method

Check tooling availability and announce to the user:
1. MCP Office tool (`mcp__office__*` or similar) → "Producing `editing/structural-edit-report.docx` via Office MCP."
2. `pandoc --version` succeeds → "Producing `editing/structural-edit-report.docx` via pandoc."
3. None → "No Word tooling found. Saving as `editing/structural-edit-report.md`."

## Step 3 — Structural analysis

Work through this checklist against the draft:

### Act structure
- Where does Act 1 end? Does it fall at approximately 20–25%?
- Where is the midpoint? Does it genuinely shift the story's direction?
- Where does Act 2 end (all is lost moment)? Does it fall at approximately 75%?
- Is Act 2 mid-section sagging (lack of escalation between 30–60%)?

### Plot logic
- Events requiring information or ability the protagonist doesn't have at that point
- Coincidences that conveniently solve a problem for the protagonist
- Plot threads introduced and dropped without resolution

### Character arcs
- Does the protagonist's wound/lie manifest clearly in Act 1?
- Is there a moment where the lie is genuinely challenged?
- Does the climax require the protagonist to act on their arc?
- Are supporting character arcs resolved?

### Scene necessity
For each chapter or scene, check: does it do at least one of these?
1. Move the external plot forward
2. Deepen or complicate character relationships
3. Establish essential world-building
4. Create or escalate tension

Flag any scene that fails all four tests.

### Pacing
- Note where pace is too slow (multiple consecutive scenes with no escalation)
- Note where pace is too fast (important emotional beats skipped)
- Note if the climax is proportionally short relative to setup

### Genre-specific checks

**Romance:** Is the black moment genuinely devastating? Is the resolution earned? Does the HEA/HFN feel like a real ending?

**Thriller:** Is the ticking clock consistent? Does each act raise the personal cost of failure? Is the antagonist's plan logical from their perspective?

**Fantasy/Sci-fi:** Are the world-building rules consistent? Is the protagonist's power level consistent?

**Horror:** Is dread built gradually? Is the resolution appropriately ambiguous or costly?

## Step 4 — Produce report

Write a structural edit report with these sections:

1. **Summary** — 2–3 sentences on overall structural strengths and main areas to address
2. **Critical issues** (must address before the book works) — prioritised list
3. **Significant issues** (will noticeably weaken the book if left) — prioritised list
4. **Minor issues / suggestions** — lower priority
5. **Chapter-level notes** — specific structural problems per chapter with one possible approach each
6. **Questions for the author** — things that might be intentional; ask before flagging as problems

Save to `editing/structural-edit-report.docx` (or `.md` fallback per Step 2).
