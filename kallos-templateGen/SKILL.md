---
name: kallos-templateGen
description: Use when converting existing JPG design images into editable SVG templates for Illustrator — e.g. old designs only saved as flattened images, or designs found online worth reusing as a layout starting point.
---

# kallos-templateGen

## Overview
Reverse-engineers a flat JPG design into a pure, semantic SVG that opens fully editable in Adobe Illustrator — same hard SVG rules as [kallos-contentGen](../kallos-contentGen/SKILL.md), but the source is an existing image instead of a brand brief. Produces reusable template material, not a finished on-brand piece.

## When to Use
- User has JPGs of designs (their own old work, or things found online they like) that only exist as flattened images, and wants them back as editable Illustrator layers.
- User wants to build up a library of layout templates to later use as references with kallos-contentGen.

## Project Structure
```
plantillas/
  entrada/   # JPGs the user drops in
  salida/    # generated SVGs land here
```

**Scaffolding:** if `plantillas/` doesn't exist yet when asked to convert a JPG, create `entrada/` and `salida/` (empty) and tell the user to drop JPGs into `entrada/`. The `/kallos-templates` command (`commands/kallos-templates.md`) runs this same scaffolding/status-check on demand, without converting anything.

## Process
1. Read the JPG(s) in `plantillas/entrada/`. Identify layout structure (background, graphic elements, text blocks and their hierarchy), palette, and typographic style.
2. Transcribe any text seen in the image verbatim — this is a layout template, not a finished piece, so real transcribed copy is the right placeholder (never lorem ipsum).
3. Pick colors and fonts:
   - If `kallos/brief.md` exists in this project (from kallos-contentGen) and looks filled in, adapt the palette/typography to that brand automatically.
   - Otherwise, reproduce the colors and font style detected in the JPG as faithfully as possible.
   - If the user asks for a specific adjustment (a different palette, a specific font, brand colors even without a brief present), that request overrides the default.
4. Build the SVG per Output Rules below.
5. Save to `plantillas/salida/nombre-descriptivo_YYYY-MM-DD.svg`, with zero extra commentary.

## Output Rules (hard constraints)
Same as kallos-contentGen:
- Pure SVG code only.
- Semantic layers via `<g id="...">`: `Fondo`, `Graficos`, `Textos`, plus others as needed (`Logo`, `CTA`).
- All typography as real `<text>` elements — never converted to `<path>`.
- `viewBox="0 0 W H"` matching the source image's proportions (or the piece type if known).

## Handoff to kallos-contentGen
This skill never writes into `kallos/referencias/` on its own. Once you like a generated template, copy it there yourself, into `kallos/referencias/designs/<tipo>/` (e.g. `posters/`, or `carruseles/{portada,internas,final}/`) matching its piece type — creating that subfolder if it doesn't exist yet. That's the deliberate curation step that decides what becomes real reference material.

## Common Mistakes
- Outlining text to paths — breaks editability; keep `<text>`.
- Using lorem ipsum instead of the image's actual text — defeats the point of a faithful template.
- Ignoring an existing `kallos/brief.md` and always reproducing the source JPG's original colors.
- Auto-copying output into `kallos/referencias/` — that step is manual, by design.
