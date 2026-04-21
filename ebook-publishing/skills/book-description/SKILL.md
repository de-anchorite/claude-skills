---
name: book-description
description: "Use when writing blurbs, back-cover copy, or Amazon descriptions for a fiction book. Output is used by the marketing-copy skill."
---

# Book Description

Write compelling blurbs and back-cover descriptions. Produces three variants using different hook styles, each in short and long form.

## Step 1 — Load book context

Read `book-profile.md`. Extract: title, genre, subgenre, protagonist(s), antagonist, core conflict, setting, tone, comps, target audience.

## Step 2 — Gather additional context

Ask:

1. **Target platform** — Where will this description be used?
   - Amazon KDP product page
   - Kobo / other retailer
   - Pitch to literary agent or publisher
   - Back cover of print book

2. **What to tease vs. reveal** — How much of the plot should the description hint at? Any specific things to avoid spoiling?

3. **Emotional promise** — What feeling do you want the reader to have after reading the description that makes them buy? (e.g., "heart-pounding tension", "cosy warmth", "delicious anticipation")

## Step 3 — Write three variants

### Variant A — Hook-led
Opens with a high-stakes situational hook. Gets the reader into the conflict immediately before introducing the protagonist.

### Variant B — Character-led
Opens by establishing the protagonist's want or wound. Makes the reader care about the person before revealing the conflict.

### Variant C — Question-led
Opens with a provocative question or statement that creates immediate intrigue. Works well for thrillers and mysteries.

For each variant, write:
- **Short version** (~150 words): for also-bought widgets, social media, or a one-paragraph pitch
- **Long version** (~400 words): for Amazon product page or back cover

### Genre-specific guidance:

**Romance:** Convey emotional stakes of the relationship, not just the external plot. End with what the reader is promised. Signal heat level if appropriate.

**Thriller:** Lead with threat and ticking clock. Convey pace and stakes. End on an open question that creates urgency.

**Fantasy/Sci-fi:** Establish the world's pull and the protagonist's unique position. One or two specific world-building details, not a geography lesson.

**Horror:** Build unease in the description itself. Use the language of dread. Don't give away the monster.

**Literary Fiction:** Convey voice and atmosphere over plot. The description should feel like the book.

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
