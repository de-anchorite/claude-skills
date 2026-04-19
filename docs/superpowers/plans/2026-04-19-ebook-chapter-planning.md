# Chapter Planning Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the chapter-planning stub with a fully implemented SKILL.md that produces a scene-by-scene chapter outline with goals, conflicts, POV, and word count targets.

**Architecture:** Reads `book-profile.md` and `planning/story-development.md` if present. Saves `planning/chapter-planning.md`.

**Tech Stack:** Markdown + YAML front-matter. No build step.

**Prerequisite:** Task 1 of `2026-04-19-ebook-publishing-plugin.md` must be complete (plugin scaffold).

---

## File Map

**Modify:**
- `ebook-publishing/skills/chapter-planning/SKILL.md` — replace stub with full implementation
- `ebook-publishing/README.md` — update status from 🚧 Stub to ✅ Ready

---

## Task 1: Implement chapter-planning skill

- [ ] **Step 1: Replace stub with full SKILL.md**

Write `ebook-publishing/skills/chapter-planning/SKILL.md`:

```markdown
---
name: chapter-planning
description: "Create a scene-by-scene chapter outline with goals, conflicts, POV, and word count targets. Reads book-profile.md and story-development.md. Triggers: 'plan my chapters', 'chapter outline', 'scene list', 'outline my chapters', 'scene by scene breakdown'."
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
```

- [ ] **Step 2: Verify frontmatter**

```bash
python -c "
import re
with open('ebook-publishing/skills/chapter-planning/SKILL.md') as f:
    content = f.read()
match = re.match(r'^---\n(.*?)\n---', content, re.DOTALL)
print('Frontmatter found:', bool(match))
"
```

Expected: `Frontmatter found: True`

- [ ] **Step 3: Commit**

```bash
git add ebook-publishing/skills/chapter-planning/SKILL.md
git commit -m "feat(ebook-publishing): implement chapter-planning skill"
```

---

## Task 2: Update README status

- [ ] **Step 1: Update status badge**

In `ebook-publishing/README.md`, change:

```
| `chapter-planning` | 🚧 Stub | Scene-by-scene chapter outline |
```

to:

```
| `chapter-planning` | ✅ Ready | Scene-by-scene chapter outline |
```

- [ ] **Step 2: Commit**

```bash
git add ebook-publishing/README.md
git commit -m "docs(ebook-publishing): mark chapter-planning as ready"
```
