# Install & Use

## Setup prompt

Paste into Claude Code (or any agent with filesystem access):

```
Set up https://github.com/Salda1308/Kallos for me. Clone it into ~/.claude/skills/Kallos
so the SKILL.md is picked up automatically. Then tell me it's ready and wait for me to
point you at a brand brief and reference folder.
```

## Manual install

```bash
git clone https://github.com/Salda1308/Kallos.git ~/.claude/skills/Kallos
```

Claude Code loads any `SKILL.md` under `~/.claude/skills/*` automatically — no build step, no dependencies.

## First use in a project

```bash
cd /path/to/your/project
claude
```

Then ask for a design, e.g.:

```
genera un post para el lanzamiento X
```

If this is the first design in this project, Kallos creates a `kallos/` folder with:

```
kallos/
  brief.md         # copied from the brief template — fill this in
  referencias/      # drop reference designs here (images, PDFs)
  output/          # generated SVGs land here
```

Fill in `kallos/brief.md` (colors, typography, tone, company info, logo path, do's/don'ts) and drop any reference designs into `kallos/referencias/`, then ask again. Kallos never overwrites an existing `brief.md`.

## Regular use

```
genera un carrusel de 4 slides para el lanzamiento X
```

Kallos reads `kallos/brief.md` and `kallos/referencias/`, plans the slides, and writes the SVGs to `kallos/output/` — no long explanations, no back-and-forth unless a color or font is genuinely missing from the brief.
