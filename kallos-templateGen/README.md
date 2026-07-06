# kallos-templateGen

Part of the [Kallos](../README.md) family. Turns flat JPG designs into editable SVG templates for Adobe Illustrator.

Drop in a JPG of a design you like — an old flattened export, or something you found online — and get back an SVG with real layers and real editable text, ready to become reference material for [kallos-contentGen](../kallos-contentGen/README.md).

## What it does

- Reads JPGs from `plantillas/entrada/`
- Reconstructs layout, palette, and typographic style as pure SVG with semantic `<g id="...">` layers
- Transcribes the image's actual text into live `<text>` elements — never outlined, never lorem ipsum
- If a `kallos/brief.md` already exists in the project, adapts colors/fonts to that brand automatically; otherwise reproduces the source image's palette faithfully. Either way, you can ask for specific adjustments (a different palette, a specific font) at request time.
- Saves to `plantillas/salida/nombre-descriptivo_YYYY-MM-DD.svg` — you decide which ones to move into `kallos/referencias/`

## Install

```bash
git clone https://github.com/Salda1308/Kallos.git ~/.claude/skills/Kallos
ln -sfn ~/.claude/skills/Kallos/kallos-templateGen ~/.claude/skills/kallos-templateGen
mkdir -p ~/.claude/commands
ln -sfn ~/.claude/skills/Kallos/kallos-templateGen/commands/kallos-templates.md ~/.claude/commands/kallos-templates.md
```

## Use

```bash
cd /path/to/your/project
claude
```

Run `/kallos-templates` to initialize — it scaffolds `plantillas/entrada/` and `plantillas/salida/` if missing, or reports current status. It never converts anything itself.

Then ask directly, e.g.:

```
convierte los jpg de plantillas/entrada en plantillas svg editables
```

See [SKILL.md](SKILL.md) for the full rules Claude follows.
