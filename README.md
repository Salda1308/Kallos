# svg-illustrator-designer

A [Claude Code](https://claude.com/claude-code) skill that turns a brand brief and reference designs into new marketing graphics (posts, banners, stories, carousels) as pure SVG — fully editable in Adobe Illustrator.

## What it does

Given:
- a brand brief (`.md` with hex colors, fonts, tone, company description)
- reference design files (images, PDFs)
- the copy/content you want to convey and the piece type

It plans the content (how to split a carousel into slides, or how to structure a single piece into headline/subhead/body/CTA), then generates an SVG with:

- Semantic `<g id="...">` layers (`Fondo`, `Graficos`, `Textos`, ...) so Illustrator shows organized layers
- Real `<text>` elements — never outlined to paths, so text stays editable with Illustrator's Type tool
- A `viewBox` matched to the target format (square post, story/reel, horizontal, or custom)
- Colors and fonts taken strictly from the brand brief — nothing invented

See [SKILL.md](SKILL.md) for the full rules the skill follows.

## Install

Copy (or clone) this folder into your Claude Code skills directory:

```bash
git clone <this-repo-url> ~/.claude/skills/svg-illustrator-designer
```

Claude Code picks up any `SKILL.md` under `~/.claude/skills/*` automatically.

## Use

In a Claude Code session, point it at your brand brief and reference folder and describe what you want (e.g. "genera un carrusel de 4 slides para el lanzamiento X, usando la carpeta de referencias"). The skill triggers automatically; no slash command needed.

## Output

- Single piece: `nombre-descriptivo_YYYY-MM-DD.svg` in the current working directory.
- Carousel: a subfolder `nombre-carrusel_YYYY-MM-DD/` with `slide-01.svg`, `slide-02.svg`, etc.
