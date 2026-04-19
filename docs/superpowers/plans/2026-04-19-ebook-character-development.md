# Character Development Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the character-development stub with a fully implemented SKILL.md that builds deep character profiles including wound, want, need, arc, and voice.

**Architecture:** Reads `book-profile.md`. Appends to or creates `planning/character-development.md`. Supports multiple characters in one session.

**Tech Stack:** Markdown + YAML front-matter. No build step.

**Prerequisite:** Task 1 of `2026-04-19-ebook-publishing-plugin.md` must be complete (plugin scaffold).

---

## File Map

**Modify:**
- `ebook-publishing/skills/character-development/SKILL.md` — replace stub with full implementation
- `ebook-publishing/README.md` — update status from 🚧 Stub to ✅ Ready

---

## Task 1: Implement character-development skill

**Files:**
- Modify: `ebook-publishing/skills/character-development/SKILL.md`

- [ ] **Step 1: Replace stub with full SKILL.md**

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

Accept any level of detail. Use it as the foundation; fill gaps with specific suggestions.

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
- **Thriller**: Note the competence level — how capable is this character, and what is their specific blind spot the antagonist can exploit?
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

- [ ] **Step 2: Verify frontmatter is valid YAML**

```bash
python -c "
import re
with open('ebook-publishing/skills/character-development/SKILL.md') as f:
    content = f.read()
match = re.match(r'^---\n(.*?)\n---', content, re.DOTALL)
print('Frontmatter found:', bool(match))
print(match.group(1) if match else 'MISSING')
"
```

Expected: `Frontmatter found: True` followed by the frontmatter fields.

- [ ] **Step 3: Commit**

```bash
git add ebook-publishing/skills/character-development/SKILL.md
git commit -m "feat(ebook-publishing): implement character-development skill"
```

---

## Task 2: Update README status

- [ ] **Step 1: Update status badge**

In `ebook-publishing/README.md`, change:

```
| `character-development` | 🚧 Stub | Character profiles with arc and voice |
```

to:

```
| `character-development` | ✅ Ready | Character profiles with arc and voice |
```

- [ ] **Step 2: Commit**

```bash
git add ebook-publishing/README.md
git commit -m "docs(ebook-publishing): mark character-development as ready"
```
