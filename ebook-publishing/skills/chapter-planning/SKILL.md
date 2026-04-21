---
name: chapter-planning
description: "Use when story structure is complete and the author needs a scene-by-scene chapter outline with POV, goals, conflict, and word count targets per scene."
---

# Chapter Planning

Turn your beat sheet into a chapter-by-chapter scene list with goals, conflicts, POV assignments, and word count targets.

## Step 1 — Load book context

Read `book-profile.md` and `planning/story-development.md` if present. Extract: genre, word count target, protagonist(s), beat sheet if available.

## Step 2 — Gather structure preferences

Ask:

1. **Chapter count** — Approximately how many chapters? (Rough is fine — e.g., "about 30" or "I want chapters around 3,000 words each")

2. **POV structure** — Single POV throughout, multiple POVs alternating, or omniscient?
   - If multiple: which characters get POV chapters?
   - Do you want to alternate in a fixed pattern or let story need dictate?

3. **Existing chapter ideas** — Do you have any chapters already drafted or firmly planned? Note them so the outline can anchor to them.

## Step 3 — Generate chapter list

Produce a chapter-by-chapter outline. For each chapter:

| Chapter | Title/label | POV | Beat (if applicable) | Scene goal | Conflict/obstacle | Disaster or resolution | Word count target |
|---|---|---|---|---|---|---|---|

Rules for filling this table:
- **Scene goal**: What does the POV character want to achieve in this scene? Be specific — not "learn about the antagonist" but "get the ledger from the solicitor's office before it's destroyed"
- **Conflict/obstacle**: What stops them?
- **Disaster or resolution**: Does the scene end with a setback (disaster — links to next scene) or a win (resolution — raises stakes for what comes next)? Vary the pattern but prefer more disasters than resolutions in Acts 1–2
- **Word count target**: Based on total word count ÷ chapter count, adjust for pacing (action scenes shorter, emotional scenes longer)

Map the beats from `story-development.md` to specific chapters. Flag if any beats are unplaced.

## Step 4 — POV balance check

If multiple POV: count chapters per POV character. Flag large imbalances. Suggest if any POV feels underused or overused relative to their role in the story.

## Step 5 — Save output

Save `planning/chapter-planning.md`:

```markdown
---
chapter_count: <number>
pov_structure: "<single/multiple/omniscient>"
pov_characters: ["<name>", "<name>"]
word_count_target: <number>
words_per_chapter_target: <number>
---

## Chapter Outline

| Chapter | Title/label | POV | Beat | Scene goal | Conflict | Disaster/resolution | Words |
|---|---|---|---|---|---|---|---|
<one row per chapter>

## POV Balance

<summary of chapters per POV character>

## Unplaced Beats

<any beats from story-development.md not yet mapped to a chapter>
```
