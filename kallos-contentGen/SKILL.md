---
name: kallos-contentGen
description: Use when generating a new marketing graphic (single post, banner, story, or carousel) as an SVG that must open fully editable in Adobe Illustrator, driven by a brand brief (.md with colors/fonts/tone) and reference design files.
---

# kallos-contentGen

## Overview
Turns a brand brief plus reference designs into new marketing graphics as pure, semantic SVG that opens fully editable in Adobe Illustrator — layers as `<g>` groups, text as live `<text>` (never outlined to paths).

## When to Use
- User asks for a new post/banner/story/carousel as an SVG meant for Illustrator.
- User has provided (now or earlier in the project) a brand brief and/or reference files to drive colors, typography, and style.

## Project Structure
Every project this skill works in has (or gets) a `kallos/` folder at its root:
```
kallos/
  brief.md                    # brand brief — colors, typography, tone, company info
  output/                     # generated SVGs land here
  referencias/
    designs/
      carruseles/
        portada/              # cover-slide references
        internas/             # inner-slide references
        final/                # closing-slide references
      posters/                # and any other piece type (banners/, stories/...), created on demand
    assets/
      img/                    # your own images to embed in designs
        pexels/               # photos fetched from Pexels land here, kept apart from your own
      logo/                   # brand logo files
      contenido/              # raw content/copy sources — subfolders created by the user, one per project/topic
  .env                        # PEXELS_API_KEY — only needed if a reference calls for a photo
```

**Scaffolding:** if `kallos/` doesn't exist yet when asked to generate a design, create it: `output/`, `referencias/assets/img/`, `referencias/assets/img/pexels/`, `referencias/assets/logo/`, `referencias/assets/contenido/` (all empty), `.env` (with an empty `PEXELS_API_KEY=`), and copy `templates/brief.template.md` from this skill to `kallos/brief.md`. Ensure a `.gitignore` exists at the project root and lists `kallos/.env` (create the file if missing, append the line if it isn't already there) — the key must never get committed. Then tell the user to fill in the brief before continuing — don't generate a design from an empty/template brief. If `kallos/brief.md` already exists, never overwrite it. Do **not** pre-create `referencias/designs/<tipo>/` subfolders — those are created only the first time a piece of that type is generated or saved as a reference (see How References Are Used). The `/kallos-content` command (`commands/kallos-content.md`) runs this same scaffolding/status-check on demand, without generating a design.

## Inputs
1. **Brand brief** (`kallos/brief.md`) — hex colors, font names, tone/voice, company description, logo/assets path, do's and don'ts. Single source of truth for palette and typography — never invent colors or fonts outside it. If a needed color or font is missing, stop and ask; never guess.
2. **Reference designs** (`kallos/referencias/designs/<tipo>/`) — prior designs as images (PNG/JPG) or PDFs, organized by piece type (e.g. `posters/`, `banners/`, or `carruseles/{portada,internas,final}/`). `.ai`/`.eps` files are binary and can't be parsed directly — if that's all that exists, ask for a PNG/PDF/SVG export, or proceed from the brief plus any readable refs.
3. **Supporting assets** (`kallos/referencias/assets/`) — `img/` and `logo/` for material to embed in a design; `contenido/<subfolder>/` for raw copy/content the user has organized by project or topic — if the user names a subfolder, read content from there.
4. **Raw content** — alternatively, the copy the user wants to convey pasted directly, plus the piece type (single post, banner, story, carousel).

## How References Are Used
- **Default: inspiration, not a template.** Read the files in `referencias/designs/<tipo>/` matching the requested piece type for palette, typography, tone, and composition patterns, then design a new, original layout. Never clone a reference's exact layout unless asked.
- **Literal template — only on explicit request.** If the user names a specific reference and asks to use it as a base (e.g. "usa el diseño X de referencias como base"), replicate that layout's structure and only swap in the new copy, colors, and assets.
- **Feeding the library — only on explicit request.** After generating a piece, only copy it into `referencias/designs/<tipo>/` (creating that subfolder, and its `portada`/`internas`/`final` split for carousels, if it doesn't exist yet) if the user asks to save it as a future reference (e.g. "guarda este como referencia"). Never do this by default.

## Photos in Designs
- **Trigger:** only add a photo if the reference design being used as inspiration already has a photo slot (a person, product, or scene placed in the layout). Never add one just because a photo would look nice — no reference photo slot, no photo.
- **Source order:** first look for a fitting image already in `referencias/assets/img/` (including `img/pexels/`). Only if nothing fits, search the Pexels API using a query derived from the piece's subject matter and what the reference's photo slot depicts (e.g. "abogado sonriendo", "reunión de negocios").
- **Pexels API key:** read `PEXELS_API_KEY` from `kallos/.env`. If it's empty, stop and ask the user to fill it in — never call the API without it.
- **Caching:** every photo fetched from Pexels gets saved into `referencias/assets/img/pexels/` so it's available locally next time.
- **Embedding:** reference the image with `<image href="ruta/relativa/al/archivo.jpg">` inside the appropriate `<g>` layer — never inline as base64, it bloats the file and makes the SVG unreadable.
- **Licensing:** Pexels photos are free for commercial use without required attribution, but never imply the person in the photo endorses the brand.

## Process
1. Ensure `kallos/` exists (see Project Structure); read `brief.md`, the relevant `referencias/designs/<tipo>/` folder, and `referencias/assets/`.
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
- Photos as `<image href="...">` pointing at a relative file path — never base64, and never present unless the reference calls for one (see Photos in Designs).

## File Naming
- Single piece: `kallos/output/nombre-descriptivo_YYYY-MM-DD.svg`.
- Carousel: `kallos/output/nombre-carrusel_YYYY-MM-DD/` containing `slide-01.svg`, `slide-02.svg`, etc.

## Behavior
Zero chatter. No long explanations or redundant confirmations — analyze, code, save. Only ask when critical info (color, font, content) is genuinely missing.

## Common Mistakes
- Outlining text to paths — breaks editability; keep `<text>`.
- Flat SVG with no `<g>` structure — Illustrator won't show organized layers.
- Inventing colors/fonts not in the brief — breaks brand consistency.
- Treating `.ai` reference files as directly readable — they're binary; ask for an export instead.
- Adding a photo when the reference design has no photo slot — only replicate what the reference actually shows.
- Embedding photos as base64 instead of a relative `<image href>` — bloats the file and hurts editability.
- Calling the Pexels API without checking `kallos/.env` first, or fetching a photo without caching it into `referencias/assets/img/pexels/`.
