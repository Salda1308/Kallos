---
name: svg-illustrator-designer
description: Use when generating a new marketing graphic (single post, banner, story, or carousel) as an SVG that must open fully editable in Adobe Illustrator, driven by a brand brief (.md with colors/fonts/tone) and reference design files.
---

# SVG Illustrator Designer

## Overview
Turns a brand brief plus reference designs into new marketing graphics as pure, semantic SVG that opens fully editable in Adobe Illustrator — layers as `<g>` groups, text as live `<text>` (never outlined to paths).

## When to Use
- User asks for a new post/banner/story/carousel as an SVG meant for Illustrator.
- User has provided (now or earlier in the project) a brand brief and/or reference files to drive colors, typography, and style.

## Inputs
1. **Brand brief** — a `.md` with hex colors, font names, tone/voice, company description. Single source of truth for palette and typography — never invent colors or fonts outside it. If a needed color or font is missing, stop and ask; never guess.
2. **Reference files** — prior designs as images (PNG/JPG) or PDFs. `.ai`/`.eps` files are binary and can't be parsed directly — if that's all that exists, ask for a PNG/PDF/SVG export, or proceed from the brief plus any readable refs.
3. **Raw content** — the copy the user wants to convey, plus the piece type (single post, banner, story, carousel).

## Process
1. Read the brand brief and any reference files.
2. Plan content before touching SVG:
   - Carousel → decide how many slides and what text goes on each.
   - Single piece → break the copy into hierarchy (headline / subhead / body / CTA).
3. Pick canvas size: `1080x1080` (square post), `1080x1920` (story/reel), `1920x1080` (horizontal), or a custom size if given.
4. Build the SVG per Output Rules below.
5. Save per File Naming, with zero extra commentary.

## Output Rules (hard constraints)
- Pure SVG code only.
- Semantic layers via `<g id="...">`: `Fondo`, `Graficos`, `Textos`, plus others as needed (`Logo`, `CTA`) — this is what makes Illustrator show organized layers.
- All typography as `<text>` with real `font-family`/`font-weight` from the brief. Never convert text to `<path>` — it must stay selectable and editable with Illustrator's Type tool.
- `viewBox="0 0 W H"` matching the chosen canvas size.
- Colors and fonts strictly from the brief and references — no invented palette.

## File Naming
- Single piece: `nombre-descriptivo_YYYY-MM-DD.svg` saved directly in the current working directory.
- Carousel: subfolder `nombre-carrusel_YYYY-MM-DD/` containing `slide-01.svg`, `slide-02.svg`, etc.

## Behavior
Zero chatter. No long explanations or redundant confirmations — analyze, code, save. Only ask when critical info (color, font, content) is genuinely missing.

## Common Mistakes
- Outlining text to paths — breaks editability; keep `<text>`.
- Flat SVG with no `<g>` structure — Illustrator won't show organized layers.
- Inventing colors/fonts not in the brief — breaks brand consistency.
- Treating `.ai` reference files as directly readable — they're binary; ask for an export instead.
