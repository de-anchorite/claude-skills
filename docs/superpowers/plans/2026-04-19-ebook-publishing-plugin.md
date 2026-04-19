# Ebook Publishing Plugin Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build the `ebook-publishing` Claude Code plugin — 13 skills covering the full fiction ebook lifecycle from book profiling through cover design, planning, editing, and publishing.

**Architecture:** Independent skills sharing a `book-profile.md` file as a common context store. Each skill reads that file at start and produces its own output file. No orchestration between skills — all composable and order-independent. Genre-awareness embedded in each skill's prompt logic.

**Tech Stack:** Markdown + YAML front-matter (SKILL.md files), JSON (plugin manifests). No build step, no runtime. Word output (`.docx`) produced at skill execution time via python-docx / pandoc / Office MCP — not baked into the plugin files themselves.

---

## File Map

**Create:**
- `ebook-publishing/.claude-plugin/plugin.json`
- `ebook-publishing/README.md`
- `ebook-publishing/skills/book-profile/SKILL.md`
- `ebook-publishing/skills/cover-design/SKILL.md`
- `ebook-publishing/skills/story-development/SKILL.md`
- `ebook-publishing/skills/character-development/SKILL.md`
- `ebook-publishing/skills/chapter-planning/SKILL.md`
- `ebook-publishing/skills/writing-style/SKILL.md`
- `ebook-publishing/skills/series-planner/SKILL.md`
- `ebook-publishing/skills/structural-editing/SKILL.md`
- `ebook-publishing/skills/content-line-editing/SKILL.md`
- `ebook-publishing/skills/proofreading/SKILL.md`
- `ebook-publishing/skills/book-description/SKILL.md`
- `ebook-publishing/skills/publishing-metadata/SKILL.md`
- `ebook-publishing/skills/marketing-copy/SKILL.md`

**Modify:**
- `.claude-plugin/marketplace.json` — add `ebook-publishing` entry

---

## Task 1: Plugin scaffold and marketplace entry

**Files:**
- Create: `ebook-publishing/.claude-plugin/plugin.json`
- Modify: `.claude-plugin/marketplace.json`

- [ ] **Step 1: Create directory structure**

```bash
mkdir -p ebook-publishing/.claude-plugin
mkdir -p ebook-publishing/skills/book-profile
mkdir -p ebook-publishing/skills/cover-design
mkdir -p ebook-publishing/skills/story-development
mkdir -p ebook-publishing/skills/character-development
mkdir -p ebook-publishing/skills/chapter-planning
mkdir -p ebook-publishing/skills/writing-style
mkdir -p ebook-publishing/skills/series-planner
mkdir -p ebook-publishing/skills/structural-editing
mkdir -p ebook-publishing/skills/content-line-editing
mkdir -p ebook-publishing/skills/proofreading
mkdir -p ebook-publishing/skills/book-description
mkdir -p ebook-publishing/skills/publishing-metadata
mkdir -p ebook-publishing/skills/marketing-copy
```

- [ ] **Step 2: Write plugin.json**

Write `ebook-publishing/.claude-plugin/plugin.json`:

```json
{
  "name": "ebook-publishing",
  "version": "1.0.0",
  "description": "Fiction ebook writing and publishing skills: book profiling, cover design, story planning, editing (structural/line/proofreading), and publishing.",
  "author": {
    "name": "David Else"
  },
  "skills": ["./skills/"]
}
```

- [ ] **Step 3: Validate plugin.json**

```bash
python -m json.tool ebook-publishing/.claude-plugin/plugin.json
```

Expected: valid JSON printed with no errors.

- [ ] **Step 4: Add marketplace entry**

Read `.claude-plugin/marketplace.json`. Add after the existing `software-engineering` entry in the `plugins` array:

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

- [ ] **Step 5: Validate marketplace.json**

```bash
python -m json.tool .claude-plugin/marketplace.json
```

Expected: valid JSON printed with no errors.

- [ ] **Step 6: Commit**

```bash
git add ebook-publishing/.claude-plugin/plugin.json .claude-plugin/marketplace.json
git commit -m "feat(ebook-publishing): add plugin scaffold and marketplace entry"
```

---

## Task 2: book-profile skill

**Files:**
- Create: `ebook-publishing/skills/book-profile/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Write `ebook-publishing/skills/book-profile/SKILL.md`:

```markdown
---
name: book-profile
description: "Foundation skill for ebook projects. Sets up directory structure and saves book-profile.md for all other ebook skills to read. Triggers: 'set up my book', 'create book profile', 'new book project', 'start my novel', or when another ebook skill cannot find book-profile.md."
---

# Book Profile

This is the foundation skill. Run it first. Every other ebook-publishing skill reads `book-profile.md` that this skill produces.

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

Tell the user:
- Path where `book-profile.md` was saved
- Directories created
- Suggested next skill based on where they are in the process:
  - Early planning → `story-development` or `character-development`
  - Visual work → `cover-design`
  - Series planning → `series-planner`
```

- [ ] **Step 2: Commit**

```bash
git add ebook-publishing/skills/book-profile/SKILL.md
git commit -m "feat(ebook-publishing): add book-profile skill"
```

---

## Task 3: cover-design skill

**Files:**
- Create: `ebook-publishing/skills/cover-design/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Write `ebook-publishing/skills/cover-design/SKILL.md`:

```markdown
---
name: cover-design
description: "Generate AI image prompts for fiction book cover design, with MCP image generation if available. Iterates from multiple concepts to a refined final cover with typography spec. Triggers: 'design my cover', 'create book cover', 'generate cover prompts', 'cover art'."
---

# Cover Design

Generate a book cover through structured concept generation and iteration. Produces image prompts for AI generation tools, executes them via MCP if available, and iterates to a final approved cover with typography specification.

## Step 1 — Load book context

Read `book-profile.md` from the current directory or any parent `book-NN/` directory. If not found, ask the user to run the `book-profile` skill first, or ask these essential questions inline:
- Title
- Genre
- Core conflict (one sentence)
- Protagonist description
- Overall tone

## Step 2 — Gather visual brief

Ask these questions one at a time:

1. **Mood** — What emotion should the cover evoke in a reader browsing a bookshop? (e.g., dread, excitement, longing, intrigue)

2. **Colour palette** — Any strong preferences? (e.g., dark jewel tones, warm sunset colours, cold blues and greys, high-contrast black and white)

3. **Visual style** — Which feels right for this book?
   - Photographic / photorealistic
   - Illustrated / painterly
   - Graphic / typographic (minimal imagery, bold type)
   - Abstract / textural (mood through colour and pattern)

4. **Focal element** — What should the eye land on first?
   - Character (face visible, or silhouette only?)
   - Setting / environment (no character)
   - Symbolic object or motif
   - Abstract composition

5. **Style references** — Any covers you love or want to echo? Any to avoid? ("Like the early Joe Abercrombie covers", "no illustrated faces", "similar to dark romance Kindle covers")

6. **Genre conventions** — Note these automatically based on genre from book-profile, and ask if the user wants to lean into or subvert them:
   - Romance: couple, clinch, or symbolic (flower, key, silhouette)
   - Thriller: dark palette, fragmented imagery, bold sans-serif title
   - Fantasy: landscape, magical elements, character in environment
   - Sci-fi: spacecraft, alien world, tech elements, cool palette
   - Horror: dark, unsettling imagery, high contrast
   - Literary fiction: abstract, typographic, painterly

## Step 3 — Generate 3–5 concept prompts

Generate distinct concept variations — not tweaks of one idea. Cover each of these angles (skip any that clearly don't fit after the brief):

**Concept A — Character-led:** Protagonist (silhouette or partial figure) against a dramatic backdrop relevant to the story's world.

**Concept B — Setting-led:** Atmospheric environment with no visible character. Let the world carry the mood.

**Concept C — Symbolic:** A key object or motif from the story, isolated, high contrast. Bold space for title typography.

**Concept D — Abstract:** Mood through colour, light, and texture. No literal representation of story elements. Genre-coded palette.

**Concept E — Scene/action:** A specific dramatic moment from the plot rendered cinematically.

For each concept, write a complete image generation prompt in this structure:

```
[Concept X — Label]
Subject: <what is shown>
Style: <photorealistic / oil painting / digital illustration / etc.>
Lighting: <golden hour / cold overcast / dramatic chiaroscuro / neon glow / etc.>
Colour: <specific palette description>
Mood: <emotional quality>
Composition: <rule of thirds / central subject / wide establishing / close crop / etc.>
Aspect ratio: 6:9 (portrait, standard book cover)
Negative prompt: <what to exclude: text, watermarks, multiple figures if unwanted, etc.>

Full prompt (copy this into your image tool):
"<assembled single-paragraph prompt combining all elements above>"
```

## Step 4 — Execute or output prompts

**Check for MCP image generation tool:**

If an image generation MCP tool is available (check for tools named `mcp__image__*`, `mcp__dalle__*`, `mcp__openai__*`, or similar), call it with each concept prompt and display the results inline. Tell the user which tool you are using.

If no MCP image tool is available, output all prompts formatted for three platforms:

```
=== For ChatGPT / DALL-E 3 ===
[paste the full prompt above]

=== For Midjourney ===
/imagine prompt: [full prompt] --ar 2:3 --style raw --v 6

=== For Stable Diffusion / ComfyUI ===
Positive prompt: [full prompt]
Negative prompt: [negative prompt from above]
Aspect ratio: 512x768 or 832x1216
```

Tell the user to generate images for each concept and return here when ready to iterate.

## Step 5 — Concept selection

Ask the user which concept (or concepts) they want to develop further. Ask what they liked and what they'd change.

## Step 6 — Iteration loop

Refine the selected concept. Offer these refinement levers:

- **Lighting** — warmer/cooler, harder/softer, time of day
- **Framing** — closer crop, wider establishing, different angle
- **Colour grading** — shift palette (e.g., more desaturated, deeper shadows, highlight colour)
- **Element changes** — add/remove/replace focal elements
- **Aspect ratio variants** — generate versions for: 6:9 (standard), 1:1 (social), 16:9 (banner/wide)

Generate an updated prompt for each refinement request. Execute via MCP if available.

Continue until the user approves a final image.

## Step 7 — Typography specification

Once the final image is approved, produce a typography specification:

```
## Cover Typography Spec

Title: <book title>
Subtitle (if any): <subtitle>
Author name: <name>

### Title treatment
Font style: <serif / sans-serif / script / display>
Suggested typefaces: <2–3 options, e.g., "Playfair Display, Cormorant Garamond, or similar high-contrast serif">
Weight: <light / regular / bold / extra-bold>
Size: <dominant / medium / small relative to cover>
Placement: <top / bottom / centre / overlaid on subject>
Colour: <specific colour with contrast reasoning, e.g., "off-white #F5F0E8 — sufficient contrast against dark sky area">
Treatment: <solid / outlined / drop shadow / embossed / foil effect to simulate>

### Author name treatment
Font: <same as title / smaller / different weight>
Placement: <bottom third, centred>
Colour: <colour>

### Notes
<Any specific guidance: avoid placing text over busy areas of the image, use negative space at top, etc.>
```

## Step 8 — Save cover brief

Save `covers/cover-brief.md` in the book directory:

```markdown
---
final_concept: "<concept letter and label>"
image_prompt: "<final approved prompt>"
typography: "<summary of typography spec>"
---

## Visual Brief

<paste the complete brief from Steps 2–3>

## Final Prompt

<paste the final approved prompt>

## Typography Specification

<paste the full typography spec from Step 7>
```
```

- [ ] **Step 2: Commit**

```bash
git add ebook-publishing/skills/cover-design/SKILL.md
git commit -m "feat(ebook-publishing): add cover-design skill"
```

---

## Task 4: story-development skill

**Files:**
- Create: `ebook-publishing/skills/story-development/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Write `ebook-publishing/skills/story-development/SKILL.md`:

```markdown
---
name: story-development
description: "Create a genre-aware story beat sheet and plot structure for a fiction book. Reads book-profile.md. Triggers: 'develop my plot', 'create beat sheet', 'plan my story', 'story structure', 'outline my novel'."
---

# Story Development

Build a complete story structure — beat sheet, act breakdown, key plot points, and pacing notes — tailored to your genre's conventions.

## Step 1 — Load book context

Read `book-profile.md`. Extract: genre, subgenre, protagonist(s), antagonist, core conflict, setting, tone, word count target.

If not found, ask the user to run `book-profile` first.

## Step 2 — Choose story framework

Ask:
> "Which story structure framework do you want to use as a base — or would you like me to recommend one for your genre?"

Options:
- **Three-Act Structure** — classic beginning/middle/end; good for all genres
- **Save the Cat** (Blake Snyder) — 15-beat Hollywood structure; popular for commercial fiction
- **Hero's Journey** (Campbell) — transformation arc; strong for fantasy and sci-fi
- **Story Grid** (Shawn Coyne) — value-shift driven; strong for thrillers and literary fiction
- **Let Claude recommend** — based on genre

Recommendations by genre:
- Romance → Save the Cat (clear obligatory scenes)
- Thriller → Story Grid (value shifts and tension escalation)
- Fantasy / Sci-fi → Hero's Journey or Three-Act
- Literary Fiction → Three-Act or Story Grid

## Step 3 — Gather existing plot ideas

Ask:
> "What plot ideas, scenes, or turning points do you already have? Even rough notes are useful — list anything you know happens."

Accept anything from a single scene idea to a detailed outline. Use this to anchor the beats.

## Step 4 — Build the beat sheet

Produce a beat sheet using the chosen framework, filled with specifics from the book profile and the user's existing ideas. Map existing ideas to beats where possible.

### Three-Act / Save the Cat hybrid (use for Save the Cat or Three-Act choice):

| Beat | ~Page / % | What happens in THIS book |
|---|---|---|
| Opening image | 1% | |
| Theme stated | 5% | |
| Set-up | 1–10% | |
| Catalyst / Inciting incident | 10% | |
| Debate | 10–20% | |
| Break into Two | 20% | |
| B Story introduced | 22% | |
| Fun and Games (promise of premise) | 20–50% | |
| Midpoint | 50% | |
| Bad guys close in | 50–75% | |
| All is Lost | 75% | |
| Dark Night of the Soul | 75–80% | |
| Break into Three | 80% | |
| Finale | 80–99% | |
| Final image | 99–100% | |

Fill in "What happens in THIS book" for every row using known plot ideas and inference from the premise. Do not leave rows blank — make specific suggestions the author can accept or revise.

### Genre-specific obligatory scenes (add to beat sheet):

**Romance:**
- Meet cute / first meeting (Act 1)
- First kiss or near-miss (midpoint or Act 2)
- Dark moment / black moment (All is Lost — the relationship appears doomed)
- Grovel / grand gesture (Break into Three)
- HEA (Happily Ever After) or HFN (Happy For Now) resolution

**Thriller:**
- Ticking clock established (Act 1)
- False resolution / protagonist thinks they've won (midpoint)
- Escalating stakes — each act raises the cost of failure
- Twist or reveal (Act 2 climax)

**Fantasy / Sci-fi:**
- World-building integration points — where does the reader learn the rules?
- Magic system / tech reveal pacing — spread reveals, don't dump at start
- Mentor figure introduction and departure (Hero's Journey)

**Horror:**
- Dread established before explicit threat
- False safety moment (midpoint — protagonist thinks danger has passed)
- Isolation of protagonist

## Step 5 — Subplot threads

Identify 1–3 subplot threads that complement the main plot:

For each subplot:
- What is it about?
- Which characters drive it?
- How does it intersect with the main plot at midpoint and climax?
- How does it resolve?

## Step 6 — Pacing notes

Based on word count target:
- Calculate approximate words per act
- Flag any beats that feel rushed or padded given the premise
- Note where the genre expects acceleration vs. breathing room

## Step 7 — Save output

Save `planning/story-development.md` in the book directory:

```markdown
---
framework: "<chosen framework>"
---

## Beat Sheet

<full beat sheet table>

## Genre Obligatory Scenes

<list of genre-specific beats and where they land>

## Subplot Threads

<subplot descriptions>

## Pacing Notes

<pacing analysis>
```
```

- [ ] **Step 2: Commit**

```bash
git add ebook-publishing/skills/story-development/SKILL.md
git commit -m "feat(ebook-publishing): add story-development skill"
```

---

## Task 5: character-development skill

**Files:**
- Create: `ebook-publishing/skills/character-development/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Write `ebook-publishing/skills/character-development/SKILL.md`:

```markdown
---
name: character-development
description: "Build deep character profiles including wound, want, need, arc, and dialogue voice for fiction characters. Reads book-profile.md. Triggers: 'develop my character', 'character profile', 'build character arc', 'who is my protagonist', 'character development'."
---

# Character Development

Build a complete character profile — psychological depth, arc trajectory, relationship map, and distinctive voice.

## Step 1 — Load book context

Read `book-profile.md`. Note genre, tone, protagonist(s), antagonist, core conflict.

## Step 2 — Which character?

Ask:
> "Which character do you want to develop? You can do multiple — tell me who to start with."

Typical priority: protagonist first, then antagonist, then major supporting characters.

## Step 3 — Gather what the author knows

Ask:
> "What do you already know about [character name]? Tell me anything — appearance, personality, backstory, quirks, role in the story."

Accept any level of detail. Use it as the foundation; fill gaps with suggestions.

## Step 4 — Build the psychological profile

For each character, work through this framework. Make specific suggestions for each field based on the genre, tone, and what the author has shared — don't leave items as questions for the author to answer alone.

### The core wound, want, and need

- **Wound**: What painful event or circumstance from their past shapes how they move through the world? (Be specific — not "loss" but "abandoned by their father at age 9 when the family business collapsed")
- **Lie they believe**: What false belief did the wound create? (e.g., "I am only valuable if I am useful to others", "Love always ends in abandonment")
- **Want**: What does the character consciously pursue in this story? Their external goal.
- **Need**: What do they actually need to be whole? This is often the opposite of what they think they want, and is what the story's theme is about.
- **Fear**: What does achieving their need require them to face or give up?

### Arc trajectory

- **Start state**: How does the wound/lie manifest in their behaviour at the story's opening?
- **Midpoint shift**: What cracks their worldview open? What forces them to question the lie?
- **Dark moment**: How does their flaw cause the worst moment of the story for them?
- **End state**: Do they overcome the lie (positive arc), succumb to it (tragedy arc), or refuse change (flat/steadfast arc)? Which fits this book?

### Genre-specific archetype alignment

- **Romance**: Identify the romance archetype (wounded hero, grumpy sunshine, enemies to lovers role, etc.) and note how this character's wound feeds the central relationship conflict
- **Thriller**: Note the competence level — how capable is this character, and what is their specific blind spot that the antagonist can exploit?
- **Fantasy/Sci-fi**: Note their relationship to the world's power system — do they embrace it, fear it, or reject it, and why does the wound explain that?
- **Horror**: Note what they stand to lose — horror works when the reader is invested in the character's survival of something specific

### Voice and dialogue

- **Speech patterns**: Do they speak in complete sentences or fragments? Formal or casual? Verbose or terse?
- **Vocabulary register**: Educated, street-smart, regional dialect, professional jargon?
- **What they never say directly**: What topic do they deflect, change the subject on, or joke about rather than address?
- **Distinctive phrase or habit**: One memorable verbal tic or phrase (use sparingly)

## Step 5 — Relationship map

For each significant relationship this character has:
- Other character's name
- Nature of the relationship (ally, rival, mentor, romantic interest, mirror)
- What does this character want from the other person?
- What does the other person's presence force this character to confront?

## Step 6 — Save output

Append to or create `planning/character-development.md`:

```markdown
## [Character Name]

### Core Profile
- **Wound**: 
- **Lie**: 
- **Want**: 
- **Need**: 
- **Fear**: 

### Arc
- **Start**: 
- **Midpoint shift**: 
- **Dark moment**: 
- **End state**: 

### Genre archetype notes

### Voice
- Speech pattern: 
- Register: 
- Avoids discussing: 
- Distinctive phrase: 

### Relationships
| Character | Relationship | Want from them | Forces confrontation of |
|---|---|---|---|
```

Ask the user if they want to develop another character before closing.
```

- [ ] **Step 2: Commit**

```bash
git add ebook-publishing/skills/character-development/SKILL.md
git commit -m "feat(ebook-publishing): add character-development skill"
```

---

## Task 6: chapter-planning skill

**Files:**
- Create: `ebook-publishing/skills/chapter-planning/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

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

- [ ] **Step 2: Commit**

```bash
git add ebook-publishing/skills/chapter-planning/SKILL.md
git commit -m "feat(ebook-publishing): add chapter-planning skill"
```

---

## Task 7: writing-style skill

**Files:**
- Create: `ebook-publishing/skills/writing-style/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Write `ebook-publishing/skills/writing-style/SKILL.md`:

```markdown
---
name: writing-style
description: "Create a prose style guide for a fiction book: POV rules, tense, voice, dialogue conventions, and genre dos/don'ts. Reads book-profile.md. Triggers: 'create style guide', 'define my writing style', 'establish my voice', 'writing style', 'prose guidelines'."
---

# Writing Style

Establish a clear prose style guide for your book — POV rules, tense, voice characteristics, dialogue conventions, and genre-specific dos and don'ts. The content-line-editing skill uses this guide when reviewing chapters.

## Step 1 — Load book context

Read `book-profile.md`. Note: genre, tone, mood, target audience, locale (UK/US/AU).

## Step 2 — Gather style preferences

Ask these questions:

1. **Point of view** — Which POV for this book?
   - First person singular ("I walked into the room")
   - Third person limited (one character's head at a time: "She walked into the room. She couldn't see him yet.")
   - Third person omniscient (narrator has access to all characters' thoughts)
   - Multiple close third (alternating limited POV per chapter/scene)

2. **Tense** — Past tense (standard) or present tense (immediate, more unusual)?

3. **Voice** — How would you describe the narrative voice?
   - Formal and literary
   - Conversational and accessible
   - Lyrical and descriptive
   - Tight and spare (Hemingway-esque)
   - Witty and ironic
   - Dark and atmospheric

4. **Sentence style** — Preference for?
   - Short, punchy sentences (especially in action)
   - Long, flowing sentences (especially in description)
   - Varied — mix deliberately
   
5. **Authors to emulate** — Who do you want this book to feel like? (Style, not plot)

6. **Things to avoid** — Any specific habits, clichés, or stylistic choices to actively avoid?

## Step 3 — Build the style guide

Produce a complete style guide with these sections:

### POV rules
- Exact POV mode and tense
- Head-hopping rules: when (if ever) is a POV shift permitted? (Scene break only? Chapter break only? Never?)
- Deep vs. shallow POV: how close to the character's interiority should the prose stay?
- What is permitted vs. forbidden in this POV? (e.g., in tight third limited: no knowledge the POV character doesn't have; no description of the POV character's own face without a mirror)

### Voice characteristics
- 3–5 defining qualities of the narrative voice in this book
- Sample sentence structures that exemplify the voice
- Sentence length guidance (average words per sentence target)

### Dialogue conventions
- Dialogue tag preference (said only? action beats? varied?)
- Dialect and accent: how much on the page? (e.g., "a light touch — suggest accent with one or two words per character, not phonetic spelling throughout")
- Internal thought: italics or no? Tagged as thought or free indirect discourse?
- Punctuation style: Oxford comma? Em dash or en dash for interruption?

### Locale and spelling
Based on locale from book-profile.md:
- **UK**: colour, favour, realise, whilst, towards, mum, flat, mobile, boot (car), full stops inside quotes only for complete sentences
- **US**: color, favor, realize, while, toward, mom, apartment, cell, trunk, punctuation inside quotes always
- **AU**: follows UK spelling mostly; some US influence; check per word

### Genre dos and don'ts
Based on genre from book-profile.md:

**Romance:**
- DO: stay in the emotional moment; let chemistry build through subtext; earn the physical escalation
- DON'T: skip the emotional beats to get to the external plot; use purple prose unironically; resolve conflict too easily

**Thriller:**
- DO: keep sentences tighter in action sequences; withhold information the POV character doesn't have; escalate pace as deadline approaches
- DON'T: over-explain the villain's plan; have the protagonist be inexplicably lucky; slow down at the climax

**Fantasy/Sci-fi:**
- DO: world-build through character action and perception, not exposition; trust the reader to infer rules from examples
- DON'T: info-dump; use real-world idioms that wouldn't exist in this world; explain the magic system in the opening chapter

**Horror:**
- DO: build dread through implication and absence; ground the supernatural in specific sensory detail; use sentence rhythm to control pace
- DON'T: over-describe the monster; undercut tension with humour at the wrong moment; resolve ambiguity too quickly

**Literary fiction:**
- DO: prioritise language over plot; let meaning emerge; use imagery consistently
- DON'T: sacrifice clarity for obscurity; ignore plot entirely; treat character interiority as a substitute for scene

### Sample passage
Write a 150–200 word sample passage in the target style. Use characters and a situation from `book-profile.md`. This serves as a style reference touchstone.

## Step 4 — Save output

Save `planning/writing-style.md`:

```markdown
---
pov: "<pov mode>"
tense: "<past|present>"
locale: "<UK|US|AU>"
---

## POV Rules

## Voice Characteristics

## Dialogue Conventions

## Locale and Spelling

## Genre Dos and Don'ts

## Sample Passage

<the sample passage>
```
```

- [ ] **Step 2: Commit**

```bash
git add ebook-publishing/skills/writing-style/SKILL.md
git commit -m "feat(ebook-publishing): add writing-style skill"
```

---

## Task 8: series-planner skill

**Files:**
- Create: `ebook-publishing/skills/series-planner/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Write `ebook-publishing/skills/series-planner/SKILL.md`:

```markdown
---
name: series-planner
description: "Plan a multi-book fiction series: series arc, per-book arcs, character development across books, world-building continuity, and series bible. Can convert an existing single-book project to a series directory layout. Triggers: 'plan my series', 'series arc', 'map out my series', 'series bible', 'series planner'."
---

# Series Planner

Plan a multi-book series: the overarching series arc, individual book arcs, character development trajectory, world-building continuity, and release strategy.

## Step 1 — Load book context

Read `book-profile.md` from the current book directory. This is the anchor book for the series (usually Book 1).

If no `book-profile.md` found, ask the user to run `book-profile` first.

## Step 2 — Detect project layout

Check whether the current project is already in a series layout (i.e., is the current directory named `book-01/` or similar, with a parent directory containing other book directories?).

If NOT in series layout, offer:
> "Your current project looks like a standalone book layout. I can convert it to a series layout — this means moving your current book files into a `book-01/` subdirectory and creating a series root directory. No files will be deleted or overwritten. Want me to do that?"

If yes: perform the conversion (create series root, move book files into `book-01/`, create `book-02/` skeleton). Tell the user what was moved.

## Step 3 — Gather series overview

Ask:

1. **Series name** — What is the series called? (Can be a working title)

2. **Series type** — Which best describes this series?
   - **Linear series**: one continuous story across all books (readers must read in order)
   - **Episodic series**: same characters, self-contained stories (can read in any order)
   - **Shared world**: different characters/books, same world and continuity

3. **Planned book count** — How many books planned? (Approximate is fine)

4. **Series-level conflict** — What is the overarching threat or question that spans the whole series, beyond the individual book conflicts?

5. **Series-level character arc** — How does the protagonist change across the full series? Where do they start (Book 1) and where do they end (final book)?

## Step 4 — Per-book arc summaries

For each planned book, produce a one-paragraph summary:
- Central conflict of this book (different from series conflict, but connected)
- How this book advances the series arc
- Where the character is in their series-long journey
- The emotional promise to the reader for this book (e.g., "romance readers get a second-chance romance with a satisfying HFN, while the series mystery deepens")

Flag if any book in the sequence feels like a "bridge book" with no strong standalone arc — this is a series structure problem to address.

## Step 5 — Character trajectory across books

For each major recurring character:
- How do they change from Book 1 to the final book?
- What do they know/have by the end of each book that they didn't at the start?
- Are there any character deaths or major exits planned? At which book?

## Step 6 — World-building continuity plan

List the major world-building elements that carry across books:
- Locations that recur
- Rules of the world (magic system, tech, political structure) — which aspects are revealed in which book?
- Factions, organisations, or power structures — how do they evolve?
- Timeline — how much time passes between books?

Note any continuity risks: things easy to get wrong across a multi-book series (character ages, established facts, world rules).

## Step 7 — Release strategy notes

Ask:
> "Are you planning to write all books before releasing, or release as you complete each one?"

Provide brief guidance:
- **Write first, release later**: Can plant seeds in Book 1 knowing the payoff; less reader wait time
- **Release as written**: Must ensure each book has a satisfying arc; series bibles are more important to keep updated

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
...

## Character Trajectory

## World-Building Continuity

## Continuity Risks

## Release Strategy

```
```

- [ ] **Step 2: Commit**

```bash
git add ebook-publishing/skills/series-planner/SKILL.md
git commit -m "feat(ebook-publishing): add series-planner skill"
```

---

## Task 9: structural-editing skill

**Files:**
- Create: `ebook-publishing/skills/structural-editing/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Write `ebook-publishing/skills/structural-editing/SKILL.md`:

```markdown
---
name: structural-editing
description: "Big-picture edit of a fiction draft: plot holes, structural weaknesses, pacing, character arc consistency, scene purpose. Does NOT comment on prose, grammar, or spelling. Reads book-profile.md and draft chapters or plan. Triggers: 'structural edit', 'big picture edit', 'developmental edit', 'review my plot structure', 'story edit'."
---

# Structural Editing

Review your draft for big-picture issues: plot holes, structural weaknesses, pacing problems, character arc consistency, and scene necessity. This skill does not comment on prose, grammar, or spelling — those belong to content-line-editing and proofreading.

## Scope — strictly enforced

**In scope:**
- Plot logic and consistency
- Story structure and act balance
- Pacing (too fast, too slow, uneven)
- Character arc consistency across chapters
- Scene necessity (does every scene move the plot or deepen character?)
- Subplot integration and resolution
- POV consistency at scene level (is the right character narrating each scene?)
- Thematic coherence

**Out of scope — do not comment on:**
- Grammar, spelling, punctuation
- Sentence-level prose quality
- Word choice
- Dialogue tags

If you notice a prose issue, note it only as "this chapter may benefit from line editing" — do not make the specific correction.

## Step 1 — Load book context

Read `book-profile.md`, `planning/story-development.md` (if present), and `planning/chapter-planning.md` (if present).

Ask:
> "Which draft files should I review? You can give me specific chapter files, a full manuscript, or your planning documents."

Read all provided files.

## Step 2 — Structural analysis

Work through this checklist against the draft:

### Act structure
- Where does Act 1 end (inciting incident locked in, protagonist committed)? Does it fall at approximately 20–25% of the manuscript?
- Where is the midpoint? Does it genuinely shift the story's direction?
- Where does Act 2 end (all is lost moment)? Does it fall at approximately 75%?
- Is Act 2 mid-section sagging (lack of escalation between 30–60%)?

### Plot logic
- List any events that require information or ability the protagonist doesn't have at that point
- List any coincidences that conveniently solve a problem for the protagonist
- List any plot threads introduced and then dropped without resolution

### Character arcs
- Does the protagonist's wound/lie (from book-profile or character-development) manifest clearly in Act 1?
- Is there a moment where the lie is genuinely challenged?
- Does the climax require the protagonist to act on their arc (change or fail to change)?
- Are supporting character arcs resolved, or do they disappear?

### Scene necessity
For each chapter or scene, check: does this scene do at least one of these things?
1. Move the external plot forward
2. Deepen or complicate character relationships
3. Establish essential world-building
4. Create or escalate tension

Flag any scene that fails all four tests.

### Pacing
- Note where the pace is too slow (multiple consecutive scenes with no escalation)
- Note where the pace is too fast (important emotional beats skipped)
- Note if the climax is proportionally short relative to the setup

### Genre-specific checks:

**Romance:**
- Is the black moment/dark night of the relationship genuinely devastating?
- Is the resolution earned, or does the conflict disappear too easily?
- Does the HEA/HFN feel like a real ending, not an afterthought?

**Thriller:**
- Is the ticking clock consistent and credible?
- Does each act raise the personal cost of failure for the protagonist?
- Is the antagonist's plan logical from their perspective?

**Fantasy/Sci-fi:**
- Are the world-building rules consistent? (Does anything contradict an established rule?)
- Is the protagonist's power level consistent? (No sudden competence without justification)

**Horror:**
- Is the dread built gradually, or does the threat appear too suddenly?
- Is the resolution too tidy? (Horror often benefits from ambiguity or cost)

## Step 3 — Produce report

Write a structural edit report with:

1. **Summary** — 2–3 sentences on the draft's overall structural strengths and main areas to address

2. **Critical issues** (must address before the book works) — prioritised list

3. **Significant issues** (will noticeably weaken the book if left) — prioritised list

4. **Minor issues / suggestions** (lower priority improvements)

5. **Chapter-level notes** — for any chapter with a specific structural problem, a brief note on what the problem is and one possible approach

6. **Questions for the author** — things that might be intentional choices; ask before flagging as problems

## Step 4 — Save output

Check for Word tooling availability in this order:
1. MCP Office tool (`mcp__office__*` or similar) — use to create `.docx` directly
2. `python` with `python-docx` — run a script to create the `.docx`
3. `pandoc` — convert markdown to `.docx`
4. Fallback — save as `editing/structural-edit-report.md` and tell the user Word tooling was unavailable

Save as `editing/structural-edit-report.docx` (or `.md` if fallback). Tell the user which format was used.
```

- [ ] **Step 2: Commit**

```bash
git add ebook-publishing/skills/structural-editing/SKILL.md
git commit -m "feat(ebook-publishing): add structural-editing skill"
```

---

## Task 10: content-line-editing skill

**Files:**
- Create: `ebook-publishing/skills/content-line-editing/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Write `ebook-publishing/skills/content-line-editing/SKILL.md`:

```markdown
---
name: content-line-editing
description: "Line edit fiction draft chapters: grammar, consistency, continuity, POV enforcement, style adherence, locale accuracy. Produces Word Track Changes output. Does NOT make structural changes. Triggers: 'line edit', 'content edit', 'edit my chapter', 'check consistency', 'copy edit'."
---

# Content and Line Editing

Review and edit draft chapters for grammar, consistency, continuity, POV enforcement, style adherence, and locale accuracy. Produces output with Track Changes so the author can accept or reject each suggestion.

## Scope — strictly enforced

**In scope:**
- Grammar and syntax errors
- Internal consistency (does the character have brown eyes in chapter 1 but blue in chapter 7?)
- Continuity errors (timeline, object placement, character knowledge)
- POV violations (knowing things the POV character can't know; describing the POV character's own face without a mirror)
- Style guide adherence (check against `planning/writing-style.md` if present)
- Locale accuracy (UK/US/AU spelling and idiom — from book-profile.md locale field)
- Sentence-level clarity and flow
- Word choice and repetition (same word used 3+ times on one page)
- Dialogue punctuation

**Out of scope — do not comment on:**
- Plot holes or structural issues (structural-editing handles those)
- Basic spelling and punctuation (proofreading handles those)
- Whether the story is compelling (that is not this skill's job)

## Step 1 — Load book context

Read `book-profile.md` and `planning/writing-style.md` (if present). Note: genre, locale, POV mode, tense, voice characteristics.

Ask:
> "Which chapter(s) would you like me to line edit? Provide the file path(s)."

Read the provided chapters.

## Step 2 — Check Word tooling

Announce to the user which output method you will use:

1. **MCP Office tool** — if `mcp__office__*` or equivalent tool available: "I'll produce a .docx file with Word Track Changes."
2. **python-docx** — if Python is available: run `python -c "import docx"` to check. If available: "I'll produce a .docx file with Track Changes using python-docx."
3. **pandoc** — if `pandoc --version` succeeds: "I'll produce a .docx file. Edits shown as [DELETED: x][INSERTED: y] blocks — not native Track Changes but visible in Word."
4. **Fallback** — "No Word tooling detected. I'll output edits as a markdown diff. Consider installing pandoc for .docx output."

## Step 3 — Line edit

Work through each chapter systematically. For every issue found, record:
- **Location**: chapter name/number, paragraph number or approximate position
- **Type**: grammar / consistency / continuity / POV / style / locale / clarity / repetition
- **Original text**: exact quote
- **Suggested text**: the replacement
- **Reason**: one-sentence explanation (e.g., "POV violation — the narrator can't see their own expression", "US spelling — 'realized' → 'realised' for UK locale")

Group issues:
- **Continuity error log**: all consistency/continuity issues in one place (character details that change, timeline problems, object tracking)
- **POV violation log**: all POV-related issues
- **Inline edits**: all grammar, style, clarity, locale edits

## Step 4 — Produce output

**If Track Changes available (methods 1 or 2):**
- Create `editing/line-edit-<chapter-name>.docx`
- Reviewer name: "Claude Edit"
- Mark deletions and insertions using Track Changes
- Add continuity errors and POV violations as Word Review Comments on the relevant passage

**If pandoc (method 3):**
- Create `editing/line-edit-<chapter-name>.docx`
- Show edits as: `[DELETED: original text][INSERTED: suggested text]`
- Add continuity/POV issues as `[NOTE: issue description]` inline

**If fallback (method 4):**
- Write `editing/line-edit-<chapter-name>.md`
- Format: `~~deleted text~~` and `**inserted text**`
- List continuity/POV issues at end of file

Tell the user the output path and which format was used.
```

- [ ] **Step 2: Commit**

```bash
git add ebook-publishing/skills/content-line-editing/SKILL.md
git commit -m "feat(ebook-publishing): add content-line-editing skill"
```

---

## Task 11: proofreading skill

**Files:**
- Create: `ebook-publishing/skills/proofreading/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Write `ebook-publishing/skills/proofreading/SKILL.md`:

```markdown
---
name: proofreading
description: "Final proofread of near-complete fiction draft chapters: spelling, punctuation, typos, formatting consistency only. No content, style, or structural suggestions. Produces Word Track Changes output. Triggers: 'proofread', 'final proofread', 'check for errors', 'proof my chapter', 'spelling and punctuation check'."
---

# Proofreading

Final-pass error correction: spelling, punctuation, typos, formatting consistency. This skill makes no content, style, or structural suggestions — even if it notices them.

## Scope — strictly enforced

**In scope:**
- Spelling errors (including locale-specific: UK/US/AU from book-profile.md)
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

If the author asks for content feedback during a proofreading session, respond: "That's outside the scope of proofreading. Run the content-line-editing skill for that feedback."

## Step 1 — Load book context

Read `book-profile.md` for locale (UK/US/AU). This determines correct spelling conventions.

Ask:
> "Which chapter(s) would you like me to proofread? Provide the file path(s)."

## Step 2 — Check Word tooling

Announce which output method will be used (same priority order as content-line-editing):
1. MCP Office tool → native Track Changes `.docx`
2. python-docx → Track Changes `.docx`
3. pandoc → `.docx` with `[DELETED:][INSERTED:]` markup
4. Fallback → `.md` with `~~deleted~~` / `**inserted**` markup

## Step 3 — Proofread

Work through the chapter. Record every error:
- **Location**: paragraph number or approximate position
- **Type**: spelling / punctuation / typo / missing word / doubled word / capitalisation / formatting
- **Original**: exact text
- **Correction**: corrected text

Do not add comments or explanations beyond the correction type. No opinions.

## Step 4 — Produce output

Same output approach as content-line-editing:
- Track Changes `.docx` → `editing/proofread-<chapter-name>.docx`
- Pandoc `.docx` → `editing/proofread-<chapter-name>.docx`
- Fallback `.md` → `editing/proofread-<chapter-name>.md`

Reviewer name (if Track Changes): "Claude Proof"

At end of session, report:
- Total errors found
- Breakdown by type
- Output file path
- No content opinions
```

- [ ] **Step 2: Commit**

```bash
git add ebook-publishing/skills/proofreading/SKILL.md
git commit -m "feat(ebook-publishing): add proofreading skill"
```

---

## Task 12: book-description skill

**Files:**
- Create: `ebook-publishing/skills/book-description/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Write `ebook-publishing/skills/book-description/SKILL.md`:

```markdown
---
name: book-description
description: "Write fiction book blurbs and back-cover descriptions. Produces three hook-style variants in short and long versions, genre-coded. Reads book-profile.md. Triggers: 'write my blurb', 'book description', 'back cover copy', 'write my synopsis', 'Amazon description'."
---

# Book Description

Write compelling blurbs and back-cover descriptions for your fiction book. Produces three variants using different hook styles, each in short and long form.

## Step 1 — Load book context

Read `book-profile.md`. Extract: title, genre, subgenre, protagonist(s), antagonist, core conflict, setting, tone, comps, target audience.

## Step 2 — Gather additional context

Ask:

1. **Target platform** — Where will this description be used?
   - Amazon KDP product page
   - Kobo / other retailer
   - Pitch to literary agent or publisher
   - Back cover of print book
   (Platform affects length and tone slightly)

2. **What to tease vs. reveal** — How much of the plot should the description hint at? (e.g., "mention there's a romance subplot" or "don't reveal the protagonist is a spy")

3. **Emotional promise** — What feeling do you want the reader to have after reading the description that makes them buy? (e.g., "heart-pounding tension", "cosy warmth", "delicious anticipation")

## Step 3 — Write three variants

Write three distinct blurbs using different opening hook strategies:

### Variant A — Hook-led
Opens with a high-stakes situational hook. Gets the reader into the conflict immediately before introducing the protagonist.

### Variant B — Character-led
Opens by establishing the protagonist's want or wound. Makes the reader care about the person before revealing the conflict.

### Variant C — Question-led
Opens with a provocative question or statement that creates immediate intrigue. Works well for thrillers and mysteries.

For each variant, write:
- **Short version** (~150 words): suitable for also-bought widgets, social media, or a one-paragraph pitch
- **Long version** (~400 words): suitable for Amazon product page or back cover

### Genre-specific guidance:

**Romance:** Must convey the emotional stakes of the relationship, not just the external plot. End with what the reader is promised (tension, heat, heartbreak, redemption). Signal heat level if appropriate.

**Thriller:** Lead with the threat and the ticking clock. Convey pace and stakes. End on an open question that creates urgency.

**Fantasy/Sci-fi:** Establish the world's pull and the protagonist's unique position within it. Don't get lost in world-building — one or two specific details, not a geography lesson.

**Horror:** Build unease in the description itself. Use the language of dread. Don't give away the monster.

**Literary Fiction:** Convey voice and atmosphere over plot. The description should feel like the book — not a thriller summary of a quiet interior novel.

## Step 4 — Save output

Save `publishing/book-description.md`:

```markdown
---
platform: "<platform>"
---

## Variant A — Hook-led

### Short (~150 words)

<text>

### Long (~400 words)

<text>

## Variant B — Character-led

### Short

<text>

### Long

<text>

## Variant C — Question-led

### Short

<text>

### Long

<text>
```
```

- [ ] **Step 2: Commit**

```bash
git add ebook-publishing/skills/book-description/SKILL.md
git commit -m "feat(ebook-publishing): add book-description skill"
```

---

## Task 13: publishing-metadata skill

**Files:**
- Create: `ebook-publishing/skills/publishing-metadata/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Write `ebook-publishing/skills/publishing-metadata/SKILL.md`:

```markdown
---
name: publishing-metadata
description: "Generate publishing metadata for fiction ebooks: BISAC categories, Amazon browse categories, 7 keywords, series fields, content rating, and ISBN guidance. Reads book-profile.md. Triggers: 'publishing metadata', 'Amazon categories', 'kdp metadata', 'book keywords', 'set up my metadata'."
---

# Publishing Metadata

Generate the metadata needed to publish your fiction ebook: BISAC categories, Amazon browse category paths, discoverability keywords, series fields, content rating, and ISBN guidance.

## Step 1 — Load book context

Read `book-profile.md`. Extract: title, genre, subgenre, series info, target audience, comps, locale.

## Step 2 — Gather publishing details

Ask:

1. **Target platforms** — Which platforms are you publishing on?
   - Amazon KDP (Kindle and/or print)
   - Draft2Digital (wide distribution: Kobo, Apple Books, Barnes & Noble, etc.)
   - IngramSpark (print distribution)
   - All of the above

2. **Series info** — Is this book part of a series? If yes:
   - Series name (for the series field)
   - Volume number

3. **Price tier intent** — Rough pricing plan? (Affects category strategy: premium vs. permafree)

4. **Content advisory** — Does the book contain any of the following? (Affects content rating and category eligibility)
   - Explicit sexual content
   - Graphic violence
   - Strong language throughout

## Step 3 — Generate metadata

### BISAC category

Provide the primary BISAC code and label. BISAC is the industry standard used by most non-Amazon platforms.

Examples by genre:
- Romance → `FIC027000 FICTION / Romance / General` (adjust for subgenre)
- Thriller → `FIC031000 FICTION / Thrillers / General`
- Fantasy → `FIC009000 FICTION / Fantasy / General`
- Sci-fi → `FIC028000 FICTION / Science Fiction / General`
- Horror → `FIC015000 FICTION / Horror`
- Literary → `FIC019000 FICTION / Literary`

Provide one primary and one secondary BISAC.

### Amazon KDP browse categories

Amazon uses its own category system (not BISAC). Provide two category paths — the most specific paths available for this book's genre and subgenre.

Format: `Fiction > Genre > Subgenre > Sub-subgenre`

Example: `Fiction > Romance > Paranormal > Vampires`

Note: Amazon allows two browse category selections. Choose paths where this book can realistically rank (not the most competitive top-level categories).

### 7 keywords

Generate exactly 7 keyword phrases optimised for Amazon and retailer search. Rules:
- Use phrases, not single words (Amazon treats the full phrase as one keyword slot)
- Include: subgenre terms, tropes, setting descriptors, comp author names (if permitted), mood words
- Do NOT include: the book's title, author name, or category names already selected (wasted slots)
- Each keyword phrase up to 50 characters

### Series metadata fields

If series:
- Series name: `<name>`
- Volume: `<number>`
- Series description (1–2 sentences for series page)

### Content rating / age rating

Based on content advisory answers:
- **Clean / Sweet**: no explicit content, suitable all ages
- **Teen / YA**: mild content, some violence or language acceptable
- **Adult**: intended for adult readers
- **Adult — Explicit**: contains explicit sexual content (required disclosure on KDP)

### ISBN notes

Brief guidance based on platforms selected:
- KDP assigns a free ASIN — no ISBN required for Kindle
- Print via KDP: KDP assigns a free ISBN (has limitations) or author provides their own
- IngramSpark and wide distribution: own ISBN strongly recommended; purchase from Bowker (US) or Nielsen (UK)
- One ISBN per format per edition (ebook, paperback, hardcover are separate ISBNs)

## Step 4 — Save output

Save `publishing/publishing-metadata.md`:

```markdown
---
title: "<title>"
series: <series name or null>
series_number: <number or null>
---

## BISAC Categories
Primary: 
Secondary: 

## Amazon Browse Categories
1. 
2. 

## Keywords
1. 
2. 
3. 
4. 
5. 
6. 
7. 

## Series Metadata

## Content Rating

## ISBN Notes
```
```

- [ ] **Step 2: Commit**

```bash
git add ebook-publishing/skills/publishing-metadata/SKILL.md
git commit -m "feat(ebook-publishing): add publishing-metadata skill"
```

---

## Task 14: marketing-copy skill

**Files:**
- Create: `ebook-publishing/skills/marketing-copy/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Write `ebook-publishing/skills/marketing-copy/SKILL.md`:

```markdown
---
name: marketing-copy
description: "Write fiction book marketing copy: Amazon/Facebook ad headlines and body, social media post set, newsletter announcement, and ARC request email. Reads book-profile.md and book-description.md. Triggers: 'write marketing copy', 'create ads', 'launch copy', 'social posts', 'newsletter announcement', 'ARC email'."
---

# Marketing Copy

Write the marketing materials needed to launch your fiction book: paid ad copy, social media posts, newsletter announcement, and ARC request email.

## Step 1 — Load book context

Read `book-profile.md` and `publishing/book-description.md` (if present). Extract: title, genre, tone, emotional promise, short blurb.

## Step 2 — Gather campaign context

Ask:

1. **Launch phase** — Which phase are you in?
   - Pre-order / cover reveal (building anticipation)
   - Launch week (driving sales)
   - Backlist (steady ongoing promotion)

2. **Target channels** — Which do you need copy for? (Select all that apply)
   - Amazon Ads (sponsored products)
   - Facebook / Meta Ads
   - Newsletter / email list
   - Social media organic (Instagram, TikTok, X/Twitter)
   - ARC (Advance Review Copy) request email

3. **Author brand voice** — How do you communicate with readers?
   - Professional and polished
   - Warm and personal ("friend of a friend" tone)
   - Witty and playful
   - Direct and no-nonsense

## Step 3 — Amazon Ads copy

Amazon Ads have strict character limits and no images in sponsored product text ads.

Write 5 headline variants (max 50 characters each):
- Lead with the genre hook
- Lead with a trope readers of this genre love
- Lead with a comp author ("Fans of [Author] will love…" — check Amazon's current policy on comp author naming)
- Lead with an emotional promise
- Lead with the stakes

Write 3 body copy variants (max 150 characters each):
- Conflict-focused
- Character-focused
- Emotional outcome-focused

## Step 4 — Facebook / Meta Ads copy

Facebook ads have more room and pair with images.

Write 3 ad variants, each with:
- **Hook** (first line — must stop the scroll): 1–2 sentences
- **Body** (develops the hook): 3–5 sentences; include genre signals, tropes, emotional promise
- **Call to action**: one sentence + "Available now on Amazon" or equivalent

Keep total ad copy under 300 words per variant. Write for the reader who has never heard of you or this book.

## Step 5 — Social media post set (launch week)

Write 7 posts for launch week — one per day. Vary the angle each day:

- **Day 1 (launch day)**: The book is live. Lead with excitement and the hook.
- **Day 2**: Share a quote or excerpt (pull a compelling 2–4 sentence passage from the book)
- **Day 3**: Introduce the protagonist — who they are, what they want
- **Day 4**: The trope/genre hook — what readers of [X] love about this book
- **Day 5**: Behind the book — one genuine personal note about writing it
- **Day 6**: Reader-focused — who this book is for (paint a picture of the ideal reader)
- **Day 7**: Gratitude + reminder — thank early readers, link still live

Each post: 3–5 sentences, platform-agnostic (adapt length as needed for Instagram vs. X). Include a CTA on each.

## Step 6 — Newsletter announcement

Write a newsletter email for the launch:

```
Subject line options (write 3):
1. 
2. 
3. 

Preview text (1 line visible before open):


Body:
[Personal greeting — 1–2 sentences: genuine connection with readers]

[The announcement — 2–3 sentences: what the book is, what makes it exciting]

[The hook — 3–5 sentences: the premise and emotional promise, written for people who love this genre]

[The ask — 1–2 sentences: direct CTA to buy or pre-order]

[Closing — warm, personal, author voice]

[Signature]
```

## Step 7 — ARC request email

Write a template email to send to ARC (Advance Review Copy) readers:

```
Subject: ARC available — [Title] by [Author Name]

Body:
[Short personal intro if cold outreach, or skip if existing list]

I'm looking for readers to read and review [Title] before/around launch on [date].

[2–3 sentence description of the book — genre, premise, emotional hook]

In exchange for a free copy, I'm asking for:
- An honest review posted on Amazon (and Goodreads if you use it) around the launch date
- No obligation to give a positive review — honest feedback only

If you're interested, [reply to this email / click here / fill out this form] by [date].

[Author name and contact]
```

## Step 8 — Save output

Save `publishing/marketing-copy.docx` (or `.md` if no Word tooling). Check tooling availability using the same priority order as the editing skills (MCP Office → python-docx → pandoc → .md fallback). Announce which method is used.

Organise the document with clear section headings for each channel.
```

- [ ] **Step 2: Commit**

```bash
git add ebook-publishing/skills/marketing-copy/SKILL.md
git commit -m "feat(ebook-publishing): add marketing-copy skill"
```

---

## Task 15: README

**Files:**
- Create: `ebook-publishing/README.md`

- [ ] **Step 1: Write README.md**

Write `ebook-publishing/README.md`:

```markdown
# ebook-publishing

A Claude Code plugin for fiction ebook authors. 13 skills covering the full lifecycle: book setup, planning, cover design, editing, and publishing.

## Install

Add this marketplace to Claude Code, then install the `ebook-publishing` plugin:

```
https://raw.githubusercontent.com/davidelse/claude-skills/main/.claude-plugin/marketplace.json
```

## Skills

| Skill | Purpose | Run when |
|---|---|---|
| `book-profile` | Create book profile and directory structure | First — all other skills depend on this |
| `cover-design` | Generate AI image prompts and iterate to final cover | Any time |
| `story-development` | Beat sheet and plot structure | Early planning |
| `character-development` | Character profiles with arc and voice | Early planning |
| `chapter-planning` | Scene-by-scene chapter outline | After story-development |
| `writing-style` | Prose style guide and sample passage | Before drafting |
| `series-planner` | Multi-book series arc and bible | When planning a series |
| `structural-editing` | Big-picture plot and structure review | After first draft |
| `content-line-editing` | Grammar, consistency, POV, style | After structural edit |
| `proofreading` | Final spelling and punctuation pass | Last before publication |
| `book-description` | Back-cover blurbs in three styles | Pre-publication |
| `publishing-metadata` | BISAC categories, Amazon categories, keywords | Pre-publication |
| `marketing-copy` | Ads, social posts, newsletter, ARC email | Launch |

## Recommended workflow

1. Run `book-profile` → creates directory structure and `book-profile.md`
2. Run planning skills in any order: `story-development`, `character-development`, `chapter-planning`, `writing-style`
3. Write your draft chapters in `drafts/` as `.docx` files
4. Run `structural-editing` on the complete draft
5. Run `content-line-editing` chapter by chapter
6. Run `proofreading` on the near-final draft
7. Run `book-description`, `publishing-metadata`, `marketing-copy` before launch
8. Run `cover-design` any time

## Word document output

Editing skills produce `.docx` files with Track Changes where tooling is available. Priority order:
1. Office MCP tool (if configured)
2. python-docx (if Python installed)
3. pandoc (if installed)
4. Markdown fallback

Install pandoc for reliable `.docx` output without additional configuration: https://pandoc.org/installing.html

## Series support

`book-profile` asks whether your project is standalone or a series and scaffolds the appropriate directory structure. `series-planner` can also convert an existing standalone project to a series layout.
```

- [ ] **Step 2: Commit**

```bash
git add ebook-publishing/README.md
git commit -m "feat(ebook-publishing): add README"
```

---
