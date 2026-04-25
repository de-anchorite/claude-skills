---
name: book-profile
description: "Use when starting a new fiction book project, or when another ebook-publishing skill cannot find book-profile.md. Run first — all other ebook-publishing skills depend on the output. Also use this skill when updating a book profile."
---

# Book Profile

This is the foundation skill. Run it first. Every other ebook-publishing skill reads `book-profile.md` that this skill produces.

**Save all output files immediately on completion. Never ask the user for permission to save.**

## Step 1 — Scan existing files

Before asking any questions, read all `.md`, `.txt`, and `.docx` files in the current directory tree. Extract what you can infer:

- **Title**: from filenames, document headers, or front matter
- **Genre/subgenre**: from tone, setting, character types, vocabulary
- **Characters**: named characters appearing in the text
- **Setting**: time period and location
- **Tone/mood**: dark, hopeful, tense, romantic, etc.
- **Core conflict**: central tension driving the plot

Present what you found:
> "I found these details in your existing files — confirm or correct each one, or press enter to accept:"

List each field with your extracted value. Only ask about fields you could not infer.

If no existing files found, skip to Step 2.

## Step 2 — Standalone or series?

Ask this before anything else:

> "Is this a standalone book, or part of a series?"

If series, also ask:
- Series name
- Approximate number of books planned
- This book's number in the series (e.g., Book 1 of 3)

If a series, or converting the single book to a series, make sure you invoke the series planner and store the series planner output.

## Step 3 — Gather remaining metadata

Ask only for fields not already confirmed in Step 1. Use these questions:

- **Title** — working title is fine
- **Genre** — one of: romance, thriller, fantasy, sci-fi, horror, literary fiction
- **Subgenre** — e.g., paranormal romance, cozy mystery, epic fantasy, space opera
- **Word count target** — approximate (e.g., 80,000 words)
- **Protagonist(s)** — name and one-sentence description for each
- **Antagonist** — name and one-sentence description (person, force, or internal)
- **Core conflict** — one sentence: what does the protagonist want, what stops them?
- **Setting** — time period and primary location(s)
- **Tone** — e.g., dark and gritty, cosy and warm, tense and claustrophobic
- **Mood** — the emotional experience you want the reader to have
- **Target audience** — age range and reader expectations
- **Comparable titles (comps)** — 2–3 published books similar in feel and market position
- **Target locale** — UK English, US English, or AU English (affects editing and spelling conventions)

## Step 4 — Scaffold directory structure

Create directories that do not already exist. Never overwrite or delete existing files.

**Standalone book layout** (create inside current directory):

```
planning/
drafts/
editing/
publishing/
covers/
covers/prompts/
```

**Series layout** (ask user where series root should be if not obvious):

```
<series-root>/
  book-01/
    planning/
    drafts/
    editing/
    publishing/
    covers/
    covers/prompts/
```

For series: if the current directory contains existing book files, offer to move them into `book-01/`. Confirm with user before moving anything.

Tell the user what you created.

## Step 5 — Write book-profile.md

Save `book-profile.md` to the book's root directory (either project root for standalone, or `book-NN/` for series).

```markdown
---
title: "<title>"
genre: "<genre>"
subgenre: "<subgenre>"
series: <series name as string, or null>
series_number: <integer or null>
word_count_target: <integer>
protagonists:
  - name: "<name>"
    description: "<one sentence>"
antagonist:
  name: "<name>"
  description: "<one sentence>"
core_conflict: "<one sentence>"
setting:
  time: "<time period>"
  place: "<location>"
tone: "<tone>"
mood: "<mood>"
target_audience: "<description>"
comps:
  - "<Title by Author>"
  - "<Title by Author>"
locale: "<UK|US|AU>"
---

## Book Summary

<Write 2–3 paragraphs: who is the protagonist and what do they want; what world do they inhabit and what are its rules; what is the central conflict and what are the stakes; what is the emotional tone of the journey. This is a working document for the author, not a blurb — be concrete and specific.>
```

## Step 6 — Close

Invoke the `folder-structure` skill to audit the directory. This catches any existing files that landed outside the expected layout before further work begins.

For each significant character extracted in Step 1 (protagonists, antagonists, and named characters with meaningful roles), check `planning/character-development.md`:
- If a profile is missing: invoke the `character-development` skill. Pre-fill from what was extracted in Step 1 and confirmed in the book-profile metadata.

Check for `planning/writing-style.md`:
- If it **does not exist**: invoke the `writing-style` skill now. Pre-fill from context already gathered: locale, genre, tone, and any POV, tense, or voice patterns visible in existing draft files scanned in Step 1.
- If it **exists**: offer to update it — files scanned in Step 1 may have revealed style details not yet captured.

Tell the user:
- Path where `book-profile.md` was saved
- Directories created
- Suggested next skill based on where they are in the process:
  - Early planning → `story-development` or `character-development`
  - Visual work → `cover-design`
  - Series planning → `series-planner`
