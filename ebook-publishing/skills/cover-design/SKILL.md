---
name: cover-design
description: "Use when creating AI image prompts for a fiction book cover, or when the author is ready to develop cover concepts and typography spec."
---

# Cover Design

Generate a book cover through structured concept generation and iteration. Produces image prompts for AI generation tools, executes them via MCP if available, and iterates to a final approved cover with typography specification.

**Save `covers/cover-brief.md` immediately at the end of Step 12. Never ask the user for permission to save.** The final cover image (`covers/cover-final.*`) must also be saved — Claude saves it directly if an MCP tool is used, or instructs the user to save it if an external tool was used.

## Step 1 — Load book context

Read `book-profile.md` from the current directory or any parent `book-NN/` directory. If not found, ask the user to run the `book-profile` skill first, or ask these essential questions inline:
- Title
- Genre
- Core conflict (one sentence)
- Protagonist description
- Overall tone
- Series name and book number (if applicable)

## Step 2 — Check existing covers

Scan the `covers/` directory:

1. **Final cover** — look for `cover-final.png`, `cover-final.jpg`, or `cover-final.webp`. If found, tell the user:
   > "A final approved cover already exists: `covers/cover-final.{ext}`. Would you like to iterate on it, or start fresh?"
   If iterating, carry this filename forward as the **base image** throughout Steps 6–8.

2. **Draft images** — any other `.png`, `.jpg`, `.jpeg`, `.webp` files are candidates to iterate from. List them.

3. **Cover brief** — check for `covers/cover-brief.md`. If present, read it and note the concept, prompt, and typography spec already on file.

If no images exist and no brief exists, tell the user and proceed to generate fresh concepts.

## Step 3 — Characters on the cover

Ask:
> "Do any characters appear on this cover? If so, name them."

For each named character:
1. Check `planning/character-development.md` for a profile section matching that name
2. If a profile exists, extract the **Appearance** block (hair, build, age, distinguishing features)
3. If no profile exists:
   > "I don't have a character profile for [Name]. I'll run the character-development skill now to build one — this ensures the cover accurately reflects the character."

   Invoke the `character-development` skill for the missing character before continuing. When the profile is saved, return here and extract the Appearance block.

Store extracted appearance data — it will be injected into character-led prompts in Step 5.

## Step 4 — Series cover continuity

Read the `series` field from `book-profile.md`.

If this is a series book, ask:
> "Should this cover share a consistent visual style with the other books in the series — palette, typography treatment, layout structure?"

If **yes**:
- In sibling `book-NN/covers/` directories (prioritise `book-01`), look for:
  1. `cover-final.*` — the approved final image; reference this as the visual anchor
  2. `cover-brief.md` — read for palette, style, focal element, composition, and typography spec
- Treat the anchor book's visual language as a constraint — new concepts must feel like they belong in the same set
- Tell the user: "I'll use `book-01/covers/cover-final.{ext}` as the visual anchor for series consistency." If no final image exists there, fall back to `cover-brief.md` and note the absence.

If **no** or standalone, proceed without series constraints.

## Step 5 — Cover dimensions

Ask before anything visual:
> "What format is this cover for? This determines the pixel dimensions and aspect ratio used in all prompts."

Options:

| Option | Format | Dimensions | Aspect ratio | Notes |
|---|---|---|---|---|
| A | Amazon KDP ebook (Kindle) | **1600 × 2560 px** | 5:8 | Standard for Kindle store |
| B | Wide ebook (Kobo, Apple Books, D2D) | **1600 × 2560 px** | 5:8 | Same as KDP ebook |
| C | KDP print — 6×9" trim | **1800 × 2700 px** | 2:3 | 300 DPI, no bleed (add bleed in cover design tool) |
| D | KDP print — 5×8" trim | **1500 × 2400 px** | 5:8 | 300 DPI, no bleed |
| E | IngramSpark print | varies | varies | Ask user for trim size; calculate: width × 300, height × 300 |
| F | Custom | user specifies | calculated | Ask for pixel dimensions |

Record the selected **dimensions** and **aspect ratio** — use them in every prompt generated in this session.

If the user needs both ebook and print covers, design for the ebook dimensions first, then offer to generate print-sized variants in Step 8.

## Step 6 — Gather visual brief

Ask these questions one at a time:

1. **Mood** — What emotion should the cover evoke in a reader browsing a bookshop? (e.g., dread, excitement, longing, intrigue)

2. **Colour palette** — Any strong preferences? (e.g., dark jewel tones, warm sunset colours, cold blues and greys, high-contrast black and white). If series continuity is required, propose a palette derived from the anchor book's palette.

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

## Step 7 — Generate 3–5 concept prompts

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
  [If character-led: inject character appearance from Step 3 — e.g., "tall woman with close-cropped silver hair and a scar across her jaw, wearing a long dark coat"]
  [If iterating from existing draft: "Based on [filename] — retain [element]; change [element]"]
Style: <photorealistic / oil painting / digital illustration / etc.>
Lighting: <golden hour / cold overcast / dramatic chiaroscuro / neon glow / etc.>
Colour: <specific palette description>
  [If series continuity: "Palette consistent with [anchor book cover-brief.md palette]"]
Mood: <emotional quality>
Composition: <rule of thirds / central subject / wide establishing / close crop / etc.>
  [If series continuity: "Composition mirrors [anchor book layout structure]"]
Dimensions: <use the dimensions selected in Step 5, e.g., 1600 × 2560 px>
Aspect ratio: <use the ratio selected in Step 5, e.g., 5:8 for KDP ebook>
Negative prompt: <what to exclude: text, watermarks, multiple figures if unwanted, etc.>

Full prompt (copy this into your image tool):
"<assembled single-paragraph prompt combining all elements above>"
```

## Step 8 — Execute or output prompts

**Check for MCP image generation tool:**

If an image generation MCP tool is available (check for tools named `mcp__image__*`, `mcp__dalle__*`, `mcp__openai__*`, or similar), call it with each concept prompt and display the results inline. Tell the user which tool you are using.

If no MCP image tool is available, output all prompts formatted for three platforms:

```
=== For ChatGPT / DALL-E 3 ===
[paste the full prompt above]

=== For Midjourney ===
/imagine prompt: [full prompt] --ar <ratio from Step 5, e.g. 5:8 for KDP ebook> --style raw --v 6

=== For Stable Diffusion / ComfyUI ===
Positive prompt: [full prompt]
Negative prompt: [negative prompt from above]
Width × Height: <dimensions from Step 5, e.g. 1600 × 2560 for KDP ebook>
(If your UI requires smaller sizes, use proportional: 640 × 1024 for 5:8, 512 × 768 for 2:3)
```

Tell the user to generate images for each concept and return here when ready to iterate.

## Step 9 — Concept selection

Ask the user which concept (or concepts) they want to develop further. Ask what they liked and what they'd change.

If the user selected a concept that was generated from an existing draft image (Step 2), confirm the base image filename to carry forward into the next iteration.

## Step 10 — Iteration loop

Refine the selected concept. Offer these refinement levers:

- **Lighting** — warmer/cooler, harder/softer, time of day
- **Framing** — closer crop, wider establishing, different angle
- **Colour grading** — shift palette (e.g., more desaturated, deeper shadows, highlight colour)
- **Element changes** — add/remove/replace focal elements
- **Character accuracy** — if character appearance from Step 3 was not fully captured, iterate with more specific appearance details from the profile
- **Format variants** — once approved, offer to generate for other formats:
  - Primary format (selected in Step 5) — already done
  - 1:1 square (1600 × 1600 px) — social media / thumbnail
  - 16:9 wide (2560 × 1440 px) — banner / wide promotional use
  - Alternate print size if ebook was primary (or vice versa)

Generate an updated prompt for each refinement request. Execute via MCP if available.

Continue until the user approves a final image.

**On approval — save the final cover:**
- If the image was generated via MCP and can be saved directly: save it as `covers/cover-final.{ext}` (use the file extension matching the generated format — prefer `.png`)
- If the image was generated externally (Midjourney, DALL-E, etc.): instruct the user:
  > "Please save your approved image as `covers/cover-final.png` (or `.jpg`/`.webp`). This filename is how other skills and future sessions identify the completed cover for this book."
- There should be exactly one `cover-final.*` file per book. If one already exists, ask before overwriting.

## Step 11 — Typography specification

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

If series continuity is required, align typeface choices and treatment with the anchor book's typography spec.

## Step 12 — Save cover brief

Save `covers/cover-brief.md` in the book directory:

```markdown
---
final_concept: "<concept letter and label>"
format: "<e.g., Amazon KDP ebook>"
dimensions: "<e.g., 1600 × 2560 px>"
aspect_ratio: "<e.g., 5:8>"
final_image: "covers/cover-final.<ext>"
base_image: "<filename of draft used as iteration base, or null>"
characters_on_cover: [<list of character names, or empty>]
series_continuity: <true / false>
image_prompt: "<final approved prompt>"
typography: "<summary of typography spec>"
---

## Visual Brief

<paste the complete brief from Steps 5–6>

## Character Appearance References

<for each character on cover: paste the Appearance block from their character profile>

## Series Anchor Reference

<if series continuity: note which book's cover-brief was used as anchor and key visual constraints derived from it>

## Final Prompt

<paste the final approved prompt>

## Typography Specification

<paste the full typography spec from Step 11>
```
