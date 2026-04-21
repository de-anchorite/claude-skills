---
name: story-development
description: "Use when developing plot structure and a genre-aware beat sheet for a fiction book, after completing the book profile."
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

| Beat | ~% | What happens in THIS book |
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
