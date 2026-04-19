# Writing Style Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the writing-style stub with a fully implemented SKILL.md that produces a prose style guide covering POV, tense, voice, dialogue conventions, locale, and genre dos/don'ts.

**Architecture:** Reads `book-profile.md`. Saves `planning/writing-style.md`. Output is used by content-line-editing skill as reference.

**Tech Stack:** Markdown + YAML front-matter. No build step.

**Prerequisite:** Task 1 of `2026-04-19-ebook-publishing-plugin.md` must be complete (plugin scaffold).

---

## File Map

**Modify:**
- `ebook-publishing/skills/writing-style/SKILL.md` — replace stub with full implementation
- `ebook-publishing/README.md` — update status from 🚧 Stub to ✅ Ready

---

## Task 1: Implement writing-style skill

- [ ] **Step 1: Replace stub with full SKILL.md**

Write `ebook-publishing/skills/writing-style/SKILL.md`:

```markdown
---
name: writing-style
description: "Create a prose style guide for a fiction book: POV rules, tense, voice, dialogue conventions, locale, and genre dos/don'ts. Reads book-profile.md. Used by content-line-editing as reference. Triggers: 'create style guide', 'define my writing style', 'establish my voice', 'writing style', 'prose guidelines'."
---

# Writing Style

Establish a clear prose style guide for your book. The content-line-editing skill reads `planning/writing-style.md` when reviewing chapters.

## Step 1 — Load book context

Read `book-profile.md`. Note: genre, tone, mood, target audience, locale (UK/US/AU).

## Step 2 — Gather style preferences

Ask these questions one at a time:

1. **Point of view** — Which POV for this book?
   - First person singular ("I walked into the room")
   - Third person limited (one character's head per scene: "She walked into the room")
   - Third person omniscient (narrator accesses all characters' thoughts)
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
Based on locale from `book-profile.md`:
- **UK**: colour, favour, realise, whilst, towards, mum, flat, mobile, boot (car). Punctuation: full stops inside quotes only for complete sentences.
- **US**: color, favor, realize, while, toward, mom, apartment, cell, trunk. Punctuation: always inside quotes.
- **AU**: follows UK spelling mostly; some US influence — check per word.

### Genre dos and don'ts
Based on genre from `book-profile.md`:

**Romance:**
- DO: stay in the emotional moment; let chemistry build through subtext; earn the physical escalation
- DON'T: skip emotional beats to get to external plot; use purple prose unironically; resolve conflict too easily

**Thriller:**
- DO: tighten sentences in action sequences; withhold information the POV character doesn't have; escalate pace as deadline approaches
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
Write a 150–200 word sample passage in the target style using characters and a situation from `book-profile.md`. This serves as a style reference touchstone for the author and for the content-line-editing skill.

## Step 4 — Save output

Save `planning/writing-style.md`:

```markdown
---
pov: "<pov mode>"
tense: "<past|present>"
locale: "<UK|US|AU>"
emulate: ["<Author Name>"]
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

- [ ] **Step 2: Verify frontmatter**

```bash
python -c "
import re
with open('ebook-publishing/skills/writing-style/SKILL.md') as f:
    content = f.read()
match = re.match(r'^---\n(.*?)\n---', content, re.DOTALL)
print('Frontmatter found:', bool(match))
"
```

Expected: `Frontmatter found: True`

- [ ] **Step 3: Commit**

```bash
git add ebook-publishing/skills/writing-style/SKILL.md
git commit -m "feat(ebook-publishing): implement writing-style skill"
```

---

## Task 2: Update README status

- [ ] **Step 1: Update status badge**

In `ebook-publishing/README.md`, change:

```
| `writing-style` | 🚧 Stub | Prose style guide and sample passage |
```

to:

```
| `writing-style` | ✅ Ready | Prose style guide and sample passage |
```

- [ ] **Step 2: Commit**

```bash
git add ebook-publishing/README.md
git commit -m "docs(ebook-publishing): mark writing-style as ready"
```
