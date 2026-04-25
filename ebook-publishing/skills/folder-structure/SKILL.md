---
name: folder-structure
description: Use automatically whenever book-profile.md is found in the working directory; when starting any ebook-publishing session; when files appear in unexpected locations; when a skill cannot find expected files in standard directories; or when the user asks to tidy, audit, or reorganise project files.
---

# Folder Structure Enforcement

Audits the book or series directory against the expected layout, warns about misplaced files, and offers to move them with user confirmation. Never deletes files.

## Expected Layout

**Standalone book** (rooted at the book directory):

```
<book-root>/
  book-profile.md
  planning/         ← beat sheets, outlines, character notes, world-building
  drafts/           ← chapter files (chapter-01.md, ch-01.md, etc.)
  editing/          ← edited chapters, editor feedback docs
  publishing/       ← blurbs, metadata, formatted output (epub/mobi/pdf)
  covers/           ← draft cover images and cover-brief.md
  covers/cover-final.{ext}  ← approved final cover image (single file, named exactly)
  covers/prompts/   ← AI cover generation prompt files
```

**Series** (rooted at the series directory):

```
<series-root>/
  series-profile.md       ← optional
  book-01/
    book-profile.md
    planning/
    drafts/
    editing/
    publishing/
    covers/
    covers/prompts/
  book-02/ ...
```

## Step 1 — Detect layout type

Read `book-profile.md` if present. Check the `series` frontmatter field:
- `series: null` → standalone book layout
- `series: "<name>"` → series layout; scan for `book-NN/` subdirectories

If no `book-profile.md` exists, scan for `book-NN/` subdirectories to infer series layout. If still ambiguous, ask the user before proceeding.

## Step 2 — Scan and classify

Walk the full directory tree. Classify every file:

| Status | Meaning |
|--------|---------|
| **Expected** | Correct location for its type — skip |
| **Misplaced** | Wrong subdirectory — report with suggested destination |
| **Unknown** | Doesn't match known patterns — ask user where it belongs |

**File-to-folder mapping:**

| File pattern | Belongs in |
|---|---|
| `book-profile.md` | book root |
| `series-profile.md` | series root |
| Beat sheets, outlines, story structure `.md/.txt` | `planning/` |
| Character sheets, world-building, research notes | `planning/` |
| `chapter-*.md`, `ch-*.md`, prose draft files | `drafts/` |
| Edited chapter files, editor feedback `.md/.txt` | `editing/` |
| Blurbs, descriptions, keywords, metadata `.md/.txt` | `publishing/` |
| Formatted output: `.epub`, `.mobi`, `.pdf` | `publishing/` |
| `.docx` files (read-only, from external tools) | `publishing/` |
| `cover-final.png` / `cover-final.jpg` / `cover-final.webp` | `covers/` — this is the approved final cover; only one should exist |
| `cover-brief.md` | `covers/` |
| Draft cover images (any other `.png`, `.jpg`, `.jpeg`, `.webp`) | `covers/` |
| Cover prompt `.md/.txt` files | `covers/prompts/` |

## Step 3 — Report findings

Present a summary before taking any action:

```
Folder structure audit — <book title or directory name>

Expected folders:
  planning/       ✓
  drafts/         ✓
  editing/        ✗ (missing)
  publishing/     ✗ (missing)
  covers/         ✓
  covers/prompts/ ✓

Misplaced files:
  chapter-01.md      (root)      → drafts/
  my-outline.md      (root)      → planning/
  cover-draft.png    (drafts/)   → covers/

Unknown files (destination unclear):
  notes-from-editor.docx  (root)
  old-version.txt          (planning/)
```

If no issues found, confirm structure is clean and stop.

## Step 4 — Offer actions

Present options:

> "For misplaced files I can:
> a) Move all to their suggested locations
> b) Decide case-by-case
> c) Move some or all outside the book folder (specify destination)
> d) Leave everything as-is
>
> For unknown files I'll ask you individually where each should go."

Wait for user response before moving anything.

## Step 5 — Execute moves

- Move only files the user approved
- Create destination directories if they don't exist (report each one created)
- Report each move: `Moved: chapter-01.md → drafts/chapter-01.md`
- If a file already exists at the destination, warn and skip — never overwrite

## Core rules

1. **Never create or write `.docx` files** — `.docx` is read-only input from external tools (e.g., Word, Scrivener). All plugin output uses `.md`.
2. **Never delete** — only move, and only with explicit user approval
3. **Report first, act second** — show full audit before touching anything
4. **Create missing folders** — if `planning/` doesn't exist, create it before moving files into it
5. **Unknown files always ask** — never assume a destination for files that don't match known patterns
6. **Run proactively** — other ebook-publishing skills should invoke this if they encounter misplaced files or missing expected directories
