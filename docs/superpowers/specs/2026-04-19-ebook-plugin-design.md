# Ebook Publishing Plugin — Design Spec

**Date:** 2026-04-19  
**Status:** Approved

## Overview

A Claude Code plugin (`ebook-publishing`) providing 13 skills covering the full fiction ebook lifecycle: planning, writing support, editing, cover design, and publishing. Skills are independent and composable. `book-profile` is the foundation — all other skills read `book-profile.md` from the project root.

Genre-awareness (romance, thriller, fantasy, sci-fi, horror, literary fiction) is embedded inside each skill's prompt logic, not in the directory structure.

## Architecture

**Approach:** Independent skills with shared profile file.

- `book-profile` skill gathers all book metadata once and saves `book-profile.md` to the project root
- Every other skill reads `book-profile.md` at start; if absent, skill prompts user to run `book-profile` first (or gathers essential fields inline for simple tasks)
- Skills can be invoked in any order and used independently
- No orchestration or chaining between skills

## Plugin Structure

```
ebook-publishing/
  .claude-plugin/plugin.json
  skills/
    book-profile/SKILL.md
    cover-design/SKILL.md
    story-development/SKILL.md
    character-development/SKILL.md
    chapter-planning/SKILL.md
    writing-style/SKILL.md
    series-planner/SKILL.md
    structural-editing/SKILL.md
    content-line-editing/SKILL.md
    proofreading/SKILL.md
    book-description/SKILL.md
    publishing-metadata/SKILL.md
    marketing-copy/SKILL.md
  README.md
```

## Skills

### book-profile (foundation)

**Trigger:** "set up my book", "create book profile", or invoked automatically when another skill finds no `book-profile.md`.

**Gathers:**
- Title, genre, subgenre
- Word count target
- Protagonist(s), antagonist
- Core conflict
- Setting (time and place)
- Tone and mood
- Target audience
- Comparable titles ("comps")

**Produces:** `book-profile.md` in project root — YAML front-matter with all fields, plus a prose summary paragraph.

---

### cover-design

**Trigger:** "design my cover", "generate cover prompts", "create book cover".

**Reads:** `book-profile.md`

**Phase 1 — Visual brief gathering:**
- Mood / emotional tone (dark, hopeful, tense, romantic, etc.)
- Colour palette (warm, cool, monochrome, specific colours)
- Visual style (photographic, illustrated, painterly, typographic)
- Key focal element (character, silhouette, setting, object, abstract symbol)
- Genre cover conventions and reader expectations
- Style references or exclusions ("like Mistborn covers", "avoid faces")

**Phase 2 — Concept generation (3–5 prompts):**
Generates distinct concept variations — not tweaks of one idea. Typical concepts:
- Character-led (protagonist silhouette against backdrop)
- Setting-led (atmospheric environment, no visible character)
- Symbolic (key object/motif, bold typography focus)
- Abstract (mood through colour and texture)
- Action/scene (specific plot moment, cinematic composition)

Each prompt includes: subject, style, lighting, colour, mood, negative prompt.

**Phase 3 — Image execution:**
- If MCP image generation tool is configured: executes all prompts, returns images inline
- If no MCP: outputs formatted prompts for ChatGPT/DALL-E 3, Midjourney (`/imagine` syntax), and Stable Diffusion/ComfyUI

**Phase 4 — Iteration loop:**
User selects a concept. Skill refines: adjusts lighting, framing, colour grading, crops/aspect ratios (6×9, square, wide). Outputs typography overlay suggestions (font pairing, placement, colour contrast). Loop continues until user approves.

**Produces:** Final image prompt, image (if MCP), typography specification.

---

### story-development

**Trigger:** "develop my plot", "create beat sheet", "plan my story structure".

**Reads:** `book-profile.md`

**Gathers:** Preferred story structure (3-act, Save the Cat, Hero's Journey, Story Grid), existing plot ideas, known turning points.

**Produces:** `story-development.md` — genre-aware beat sheet, act breakdown, key plot points, subplot threads, pacing notes.

**Genre-specific additions:**
- Romance: meet cute, dark moment, black moment, HEA/HFN
- Thriller: ticking clock beats, false resolution, escalating stakes
- Fantasy: world-building integration points, magic system reveal pacing
- Sci-fi: concept introduction beats, tech revelation pacing

---

### character-development

**Trigger:** "develop my character", "create character profile", "build character arc".

**Reads:** `book-profile.md`

**Gathers:** Which character(s) to develop, known traits, role in story.

**Produces:** Character profile (wound, want, need, fear, lie the character believes, truth they must learn), arc trajectory, relationship map, dialogue quirks, genre-specific archetype alignment.

---

### chapter-planning

**Trigger:** "plan my chapters", "create scene list", "outline my chapters".

**Reads:** `book-profile.md` + `story-development.md` if present.

**Gathers:** Target chapter count, POV structure (single/multiple), scene goals.

**Produces:** Chapter-by-chapter scene list — for each: goal, conflict, disaster or resolution, POV character, word count target.

---

### writing-style

**Trigger:** "create style guide", "define my writing style", "establish my voice".

**Reads:** `book-profile.md`

**Gathers:** POV (1st/3rd limited/omniscient), tense (past/present), sentence style preferences, authors to emulate, things to avoid, target locale (UK/US/AU English).

**Produces:** Style guide — POV rules, prose guidelines, dialogue conventions, genre-specific dos/don'ts, sample passage written in target style.

---

### series-planner

**Trigger:** "plan my series", "develop series arc", "map out my series".

**Reads:** `book-profile.md` (the first book's profile, or whichever book anchors the series).

**Gathers:** Number of books planned, series arc vs. standalone-with-connections, recurring characters, world-building elements to develop across books, release strategy intent.

**Produces:** Series arc overview, per-book arc summaries, character development trajectory across books, world-building continuity plan, series bible outline, release cadence recommendations.

---

### structural-editing

**Trigger:** "structural edit", "review my plot structure", "big picture edit".

**Reads:** `book-profile.md` + draft plan and/or draft chapters.

**Scope (strict):** Plot holes, structural weaknesses, pacing, character arc consistency, scene purpose and necessity, act balance, story logic.

**Out of scope:** Line-level prose, grammar, spelling — skill does not comment on these.

**Produces:** Structural edit report — prioritised issues list, suggested restructures, chapter-level notes, questions for the author to consider.

---

### content-line-editing

**Trigger:** "line edit", "content edit", "edit my chapter(s)", "check consistency".

**Reads:** `book-profile.md` + draft chapter(s) or full manuscript.

**Scope (strict):** Grammar, internal consistency, continuity errors, POV enforcement, style guide adherence, locale accuracy (UK/US/AU English), sentence-level clarity and flow, word choice.

**Out of scope:** Structural changes (those belong in structural-editing), basic spelling/punctuation nitpicks (those belong in proofreading).

**Produces:** Inline edit suggestions with explanations, continuity error log, POV violation flags, style deviation notes.

---

### proofreading

**Trigger:** "proofread", "check for errors", "final proofread".

**Reads:** Near-final draft chapter(s) or full manuscript.

**Scope (strict):** Spelling, punctuation, formatting consistency, typos, missing or doubled words, capitalisation errors — nothing else.

**Out of scope:** Content, structure, style, clarity — skill actively declines to make these suggestions even if issues are noticed.

**Produces:** Error list with location references (chapter, paragraph), zero content opinions.

---

### book-description

**Trigger:** "write my blurb", "write book description", "create back cover copy".

**Reads:** `book-profile.md`

**Gathers:** Target platform (Amazon KDP, Kobo, pitch to agent), length preference, hook style preference.

**Produces:** `book-description.md` — three blurb variants (hook-led, character-led, question-led), each in short (≈150 words) and long (≈400 words) versions. Genre-coded emotional language and pacing appropriate to the genre.

---

### publishing-metadata

**Trigger:** "publishing metadata", "Amazon categories", "set up book metadata".

**Reads:** `book-profile.md`

**Gathers:** Target platforms (KDP, Draft2Digital, IngramSpark), series info, price tier intent.

**Produces:** BISAC categories, Amazon browse category paths (two), 7 keywords optimised for discoverability, series metadata fields, age/content rating guidance, ISBN notes.

---

### marketing-copy

**Trigger:** "write marketing copy", "create ads", "launch copy", "social posts".

**Reads:** `book-profile.md` + `book-description.md` if present.

**Gathers:** Launch phase (pre-order, launch week, backlist), target channels (Amazon Ads, Facebook/Meta, newsletter, social media).

**Produces:** Ad headline variants, ad body copy, social post set (launch week schedule), newsletter announcement, ARC request email template.

---

## Implementation Order

Build skills in this order (dependencies first):

1. `book-profile` — foundation; no dependencies
2. `cover-design` — high priority per user request; depends on book-profile
3. `story-development`
4. `character-development`
5. `chapter-planning`
6. `writing-style`
7. `series-planner`
8. `structural-editing`
9. `content-line-editing`
10. `proofreading`
11. `book-description`
12. `publishing-metadata`
13. `marketing-copy`

## Marketplace Entry

Add to `.claude-plugin/marketplace.json`:

```json
{
  "name": "ebook-publishing",
  "source": "./ebook-publishing",
  "version": "1.0.0",
  "description": "Fiction ebook writing and publishing skills: book profiling, cover design, story planning, editing (structural/line/proofreading), and publishing.",
  "author": { "name": "David Else" },
  "category": "writing",
  "keywords": ["ebook", "fiction", "writing", "publishing", "cover-design", "editing", "kdp"]
}
```

## Constraints

- Skills are Markdown + JSON — no build step, no runtime
- Cover design MCP integration: skill detects tool availability at runtime; graceful fallback to formatted text prompts
- Genre support: romance, thriller, fantasy, sci-fi, horror, literary fiction — embedded in skill prompt logic
- Locale support (content-line-editing, writing-style): UK / US / AU English
- `book-profile.md` format: YAML front-matter + prose — consistent across all skills that read it
