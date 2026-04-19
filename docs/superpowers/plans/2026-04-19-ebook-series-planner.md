# Series Planner Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the series-planner stub with a fully implemented SKILL.md that plans multi-book series arcs, character trajectories, world-building continuity, and can convert standalone projects to series directory layout.

**Architecture:** Reads `book-profile.md`. Saves `series-planner.md` at series root. Can scaffold/convert directory structure non-destructively.

**Tech Stack:** Markdown + YAML front-matter. No build step.

**Prerequisite:** Task 1 of `2026-04-19-ebook-publishing-plugin.md` must be complete (plugin scaffold).

---

## File Map

**Modify:**
- `ebook-publishing/skills/series-planner/SKILL.md` — replace stub with full implementation
- `ebook-publishing/README.md` — update status from 🚧 Stub to ✅ Ready

---

## Task 1: Implement series-planner skill

- [ ] **Step 1: Replace stub with full SKILL.md**

Write `ebook-publishing/skills/series-planner/SKILL.md`:

```markdown
---
name: series-planner
description: "Plan a multi-book fiction series: series arc, per-book arcs, character trajectory, world-building continuity, series bible. Can convert an existing standalone project to series directory layout. Triggers: 'plan my series', 'series arc', 'map out my series', 'series bible', 'series planner'."
---

# Series Planner

Plan a multi-book series and optionally convert a standalone project to a series directory layout.

## Step 1 — Load book context

Read `book-profile.md` from the current book directory (the anchor book — usually Book 1).

If no `book-profile.md` found, ask the user to run `book-profile` first.

## Step 2 — Detect project layout

Check whether the current project is already in a series layout (i.e., is the current directory named `book-01/` or similar, with a parent containing other book directories?).

If NOT in series layout, offer:
> "Your current project looks like a standalone book layout. I can convert it to a series layout — this moves your current book files into a `book-01/` subdirectory and creates a series root directory. No files will be deleted or overwritten. Want me to do that?"

If yes: create series root, move book files into `book-01/`, create `book-02/` skeleton. Tell the user what was moved.

## Step 3 — Gather series overview

Ask:

1. **Series name** — Working title is fine.

2. **Series type** — Which best describes this series?
   - **Linear**: one continuous story across all books (readers must read in order)
   - **Episodic**: same characters, self-contained stories (can read in any order)
   - **Shared world**: different characters/books, same world and continuity

3. **Planned book count** — Approximate is fine.

4. **Series-level conflict** — What is the overarching threat or question that spans the whole series, beyond individual book conflicts?

5. **Series-level character arc** — How does the protagonist change across the full series? Where do they start (Book 1) and where do they end (final book)?

## Step 4 — Per-book arc summaries

For each planned book, produce a one-paragraph summary:
- Central conflict of this book (different from series conflict, but connected)
- How this book advances the series arc
- Where the character is in their series-long journey
- The emotional promise to the reader for this book

Flag if any book feels like a "bridge book" with no strong standalone arc — this is a series structure problem to address.

## Step 5 — Character trajectory across books

For each major recurring character:
- How do they change from Book 1 to the final book?
- What do they know/have by end of each book that they didn't at the start?
- Are there character deaths or major exits planned? At which book?

## Step 6 — World-building continuity plan

List the major world-building elements that carry across books:
- Recurring locations
- Rules of the world — which aspects are revealed in which book?
- Factions, organisations, power structures — how do they evolve?
- Timeline — how much time passes between books?

Note continuity risks: things easy to get wrong across a multi-book series (character ages, established facts, world rules).

## Step 7 — Release strategy notes

Ask:
> "Are you planning to write all books before releasing, or release as you complete each one?"

Provide brief guidance:
- **Write first, release later**: Can plant seeds in Book 1 knowing the payoff; less reader wait time between books
- **Release as written**: Must ensure each book has a satisfying arc; series bible more critical to keep updated

## Step 8 — Save output

Save `series-planner.md` at the series root directory (parent of `book-01/`):

```markdown
---
series_name: "<name>"
series_type: "<linear|episodic|shared-world>"
planned_books: <number>
---

## Series Overview

## Series-Level Conflict

## Per-Book Arc Summaries

### Book 1: [Title]

### Book 2: [Title]

## Character Trajectory

## World-Building Continuity

## Continuity Risks

## Release Strategy
```
```

- [ ] **Step 2: Verify frontmatter**

```bash
python -c "
import re
with open('ebook-publishing/skills/series-planner/SKILL.md') as f:
    content = f.read()
match = re.match(r'^---\n(.*?)\n---', content, re.DOTALL)
print('Frontmatter found:', bool(match))
"
```

Expected: `Frontmatter found: True`

- [ ] **Step 3: Commit**

```bash
git add ebook-publishing/skills/series-planner/SKILL.md
git commit -m "feat(ebook-publishing): implement series-planner skill"
```

---

## Task 2: Update README status

- [ ] **Step 1: Update status badge**

In `ebook-publishing/README.md`, change:

```
| `series-planner` | 🚧 Stub | Multi-book series arc and bible |
```

to:

```
| `series-planner` | ✅ Ready | Multi-book series arc and bible |
```

- [ ] **Step 2: Commit**

```bash
git add ebook-publishing/README.md
git commit -m "docs(ebook-publishing): mark series-planner as ready"
```
