# Install & Use

## Setup prompt

Paste into Claude Code (or any agent with filesystem access):

```
Set up https://github.com/Salda1308/Kallos for me. Clone it into ~/.claude/skills/Kallos,
then symlink kallos-contentGen/ into ~/.claude/skills/kallos-contentGen and
kallos-contentGen/commands/kallos-content.md into ~/.claude/commands/ so /kallos-content
works. Then tell me it's ready and wait for me to point you at a brand brief and
reference folder.
```

## Manual install

```bash
git clone https://github.com/Salda1308/Kallos.git ~/.claude/skills/Kallos
ln -sfn ~/.claude/skills/Kallos/kallos-contentGen ~/.claude/skills/kallos-contentGen
mkdir -p ~/.claude/commands
ln -sfn ~/.claude/skills/Kallos/kallos-contentGen/commands/kallos-content.md ~/.claude/commands/kallos-content.md
```

Claude Code loads any `SKILL.md` under `~/.claude/skills/*` automatically — no build step, no dependencies. The symlink registers `/kallos-content` as a slash command.

## First use in a project

```bash
cd /path/to/your/project
claude
```

Run `/kallos-content` to initialize — it creates the `kallos/` folder if missing, or reports its current status (brief filled in? references present? pieces already generated?) if it already exists. It never generates a design itself.

Alternatively, just ask for a design directly, e.g.:

```
genera un post para el lanzamiento X
```

If this is the first design in this project, Kallos creates a `kallos/` folder with:

```
kallos/
  brief.md                    # copied from the brief template — fill this in
  output/                     # generated SVGs land here
  referencias/
    designs/                  # reference designs, organized by piece type (created on demand)
    assets/
      img/                    # supporting images
      logo/                   # brand logo files
      contenido/              # raw content/copy sources — you create the subfolders
```

Fill in `kallos/brief.md` (colors, typography, tone, company info, logo path, do's/don'ts), drop logo/images into `kallos/referencias/assets/`, and organize reference designs by type under `kallos/referencias/designs/` (e.g. `posters/`, `carruseles/{portada,internas,final}/`), then ask again. Kallos never overwrites an existing `brief.md`.

## Regular use

```
genera un carrusel de 4 slides para el lanzamiento X
```

Kallos reads `kallos/brief.md` and the matching `kallos/referencias/designs/<tipo>/` folder, plans the slides, and writes the SVGs to `kallos/output/` — no long explanations, no back-and-forth unless a color or font is genuinely missing from the brief.
