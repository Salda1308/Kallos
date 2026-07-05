# Kallos

Generate on-brand marketing graphics with Claude Code — as SVG files that open 100% editable in Adobe Illustrator.

Drop a brand brief and reference designs in a folder, tell Claude what you want (a post, a banner, a story, a 5-slide carousel), get an SVG back with real layers and real editable text — no outlined paths, no flattened artwork.

## What it does

- Reads a brand brief (`.md`: hex colors, fonts, tone, company description) as the single source of truth for palette and typography
- Reads reference designs (images, PDFs) to match existing style and composition
- Plans the content before touching pixels — splits a carousel into the right number of slides, or structures a single piece into headline / subhead / body / CTA
- Outputs pure SVG with semantic `<g id="...">` layers (`Fondo`, `Graficos`, `Textos`, ...) so Illustrator shows organized layers on open
- Keeps every piece of copy as a live `<text>` element — selectable and editable with Illustrator's Type tool, never outlined
- Matches canvas size to the format: square post, story/reel, horizontal, or custom

## Setup prompt

Paste into Claude Code (or any agent with filesystem access):

```
Set up https://github.com/Salda1308/Kallos for me. Clone it into ~/.claude/skills/Kallos
so the SKILL.md is picked up automatically, and symlink commands/kallos.md into
~/.claude/commands/ so /kallos works. Then tell me it's ready and wait for me to
point you at a brand brief and reference folder.
```

Full install steps and first-use walkthrough: [install.md](install.md).

## Use

```bash
cd /path/to/your/project
claude
```

Run `/kallos` to initialize the project — it scaffolds a `kallos/` folder (`brief.md` template, `referencias/`, `output/`) if missing, or reports its current status if it already exists. It never generates a design itself.

Then, in the session, ask for the design directly:

```
genera un carrusel de 4 slides para el lanzamiento X
```

Kallos reads `kallos/brief.md` and `kallos/referencias/`, plans the slides, and writes the SVGs to `kallos/output/` — no long explanations, no back-and-forth unless a color or font is genuinely missing from the brief.

## Output

- Single piece: `kallos/output/nombre-descriptivo_YYYY-MM-DD.svg`
- Carousel: `kallos/output/nombre-carrusel_YYYY-MM-DD/` with `slide-01.svg`, `slide-02.svg`, ...

## Design principles

- **The brief is the source of truth.** Colors and fonts come only from the brand `.md` — nothing invented.
- **Text stays text.** Outlining to paths breaks editability; `<text>` is non-negotiable.
- **Structure over flat art.** Semantic `<g>` layers are what make a file feel native to Illustrator instead of a flattened import.
- **Ask only when blocked.** A missing color, font, or piece of content stops the skill to ask; everything else it decides on its own.

See [SKILL.md](SKILL.md) for the full rules Claude follows.
