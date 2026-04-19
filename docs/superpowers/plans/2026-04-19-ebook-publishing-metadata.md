# Publishing Metadata Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the publishing-metadata stub with a fully implemented SKILL.md that generates BISAC categories, Amazon browse categories, 7 keywords, series fields, content rating, and ISBN guidance.

**Architecture:** Reads `book-profile.md`. Saves `publishing/publishing-metadata.md` (structured YAML).

**Tech Stack:** Markdown + YAML front-matter. No build step.

**Prerequisite:** Task 1 of `2026-04-19-ebook-publishing-plugin.md` must be complete (plugin scaffold).

---

## File Map

**Modify:**
- `ebook-publishing/skills/publishing-metadata/SKILL.md` — replace stub with full implementation
- `ebook-publishing/README.md` — update status from 🚧 Stub to ✅ Ready

---

## Task 1: Implement publishing-metadata skill

- [ ] **Step 1: Replace stub with full SKILL.md**

Write `ebook-publishing/skills/publishing-metadata/SKILL.md`:

```markdown
---
name: publishing-metadata
description: "Generate publishing metadata for fiction ebooks: BISAC categories, Amazon browse categories, 7 keywords, series fields, content rating, ISBN guidance. Reads book-profile.md. Triggers: 'publishing metadata', 'Amazon categories', 'kdp metadata', 'book keywords', 'set up my metadata'."
---

# Publishing Metadata

Generate the metadata needed to publish your fiction ebook on Amazon KDP, Draft2Digital, IngramSpark, and other platforms.

## Step 1 — Load book context

Read `book-profile.md`. Extract: title, genre, subgenre, series info, target audience, comps, locale.

## Step 2 — Gather publishing details

Ask:

1. **Target platforms** — Which are you publishing on?
   - Amazon KDP (Kindle and/or print)
   - Draft2Digital (wide: Kobo, Apple Books, Barnes & Noble, etc.)
   - IngramSpark (print distribution)
   - All of the above

2. **Series info** — Is this part of a series? If yes: series name and volume number.

3. **Content advisory** — Does the book contain any of the following?
   - Explicit sexual content
   - Graphic violence
   - Strong language throughout

## Step 3 — Generate metadata

### BISAC categories

Provide the primary BISAC code and label, plus one secondary.

Common fiction BISAC codes:
- Romance (General) → `FIC027000`
- Romance (Contemporary) → `FIC027020`
- Romance (Paranormal) → `FIC027100`
- Thrillers (General) → `FIC031000`
- Thrillers (Psychological) → `FIC031010`
- Fantasy (General) → `FIC009000`
- Fantasy (Epic) → `FIC009010`
- Science Fiction (General) → `FIC028000`
- Horror → `FIC015000`
- Literary → `FIC019000`
- Mystery & Detective (General) → `FIC022000`

Select the most specific applicable code for this book's genre and subgenre.

### Amazon KDP browse categories

Amazon uses its own category system. Provide two category paths — the most specific paths available. Choose paths where this book can realistically rank (not top-level hyper-competitive categories).

Format: `Fiction > Genre > Subgenre`

Examples:
- `Romance > Paranormal > Vampires`
- `Mystery, Thriller & Suspense > Thrillers > Psychological`
- `Science Fiction & Fantasy > Fantasy > Epic`

### 7 keywords

Generate exactly 7 keyword phrases optimised for Amazon and retailer search:
- Use phrases, not single words
- Include: subgenre terms, popular tropes, setting descriptors, mood words, comp author names where permitted
- Do NOT include: book title, author name, or category names already selected
- Each phrase max 50 characters

### Series metadata fields (if applicable)

- Series name: `<name>`
- Volume: `<number>`
- Series description (1–2 sentences for series page on Amazon)

### Content rating

Based on content advisory answers:
- **Clean / Sweet**: no explicit content
- **Teen / YA**: mild content acceptable
- **Adult**: intended for adult readers
- **Adult — Explicit**: contains explicit sexual content (required disclosure on KDP)

### ISBN guidance

Brief guidance based on platforms:
- Kindle: KDP assigns a free ASIN — no ISBN required
- KDP Print: KDP assigns a free ISBN (with limitations) or author provides their own
- IngramSpark and wide distribution: own ISBN strongly recommended
- Purchase from: Bowker (US — myidentifiers.com), Nielsen (UK), Thorpe-Bowker (AU)
- One ISBN per format per edition (ebook, paperback, hardcover = separate ISBNs)

## Step 4 — Save output

Save `publishing/publishing-metadata.md`:

```markdown
---
title: "<title>"
series: <series name or null>
series_number: <number or null>
---

## BISAC Categories
Primary: <code> — <label>
Secondary: <code> — <label>

## Amazon Browse Categories
1. <path>
2. <path>

## Keywords
1. <phrase>
2. <phrase>
3. <phrase>
4. <phrase>
5. <phrase>
6. <phrase>
7. <phrase>

## Series Metadata
Series name: 
Volume: 
Series description: 

## Content Rating
<rating>

## ISBN Notes
<platform-specific guidance>
```
```

- [ ] **Step 2: Verify frontmatter**

```bash
python -c "
import re
with open('ebook-publishing/skills/publishing-metadata/SKILL.md') as f:
    content = f.read()
match = re.match(r'^---\n(.*?)\n---', content, re.DOTALL)
print('Frontmatter found:', bool(match))
"
```

Expected: `Frontmatter found: True`

- [ ] **Step 3: Commit**

```bash
git add ebook-publishing/skills/publishing-metadata/SKILL.md
git commit -m "feat(ebook-publishing): implement publishing-metadata skill"
```

---

## Task 2: Update README status

- [ ] **Step 1: Update status badge**

In `ebook-publishing/README.md`, change:

```
| `publishing-metadata` | 🚧 Stub | BISAC categories, Amazon categories, keywords |
```

to:

```
| `publishing-metadata` | ✅ Ready | BISAC categories, Amazon categories, keywords |
```

- [ ] **Step 2: Commit**

```bash
git add ebook-publishing/README.md
git commit -m "docs(ebook-publishing): mark publishing-metadata as ready"
```
