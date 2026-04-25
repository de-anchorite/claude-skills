---
name: series-planner
description: "Use when planning a multi-book fiction series arc, per-book structure, character trajectories, and world-building continuity, or when converting a standalone project to series layout."
---

# Series Planner

Plan a multi-book series and optionally convert a standalone project to a series directory layout.

**Save all output files immediately on completion. Never ask the user for permission to save.**

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
