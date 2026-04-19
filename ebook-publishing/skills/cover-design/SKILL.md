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
