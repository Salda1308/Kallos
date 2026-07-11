# Kallos

A family of [Claude Code](https://claude.com/claude-code) skills for going from brand identity to on-brand marketing graphics — as SVG files that stay 100% editable in Adobe Illustrator.

## Skills in this repo

- **[kallos-contentGen](kallos-contentGen/README.md)** — generates new posts, banners, stories, and carousels from a brand brief and reference designs.
- **[kallos-templateGen](kallos-templateGen/README.md)** — converts flat JPG designs (old exports, things found online) into editable SVG templates, to feed kallos-contentGen's reference library.
- **[kallos-igScheduler](kallos-igScheduler/README.md)** — writes captions/hashtags for finished designs and schedules them to actually publish to Instagram at future dates/times.

Each skill is self-contained: its own `SKILL.md`, its own slash command, its own README. Install the ones you need.

## Install everything

```bash
git clone https://github.com/Salda1308/Kallos.git ~/.claude/skills/Kallos
ln -sfn ~/.claude/skills/Kallos/kallos-contentGen ~/.claude/skills/kallos-contentGen
ln -sfn ~/.claude/skills/Kallos/kallos-templateGen ~/.claude/skills/kallos-templateGen
ln -sfn ~/.claude/skills/Kallos/kallos-igScheduler ~/.claude/skills/kallos-igScheduler
mkdir -p ~/.claude/commands
ln -sfn ~/.claude/skills/Kallos/kallos-contentGen/commands/kallos-content.md ~/.claude/commands/kallos-content.md
ln -sfn ~/.claude/skills/Kallos/kallos-templateGen/commands/kallos-templates.md ~/.claude/commands/kallos-templates.md
ln -sfn ~/.claude/skills/Kallos/kallos-igScheduler/commands/kallos-ig.md ~/.claude/commands/kallos-ig.md
```

See each skill's own README/install.md for setup prompts and detailed usage.

## License

MIT — see [LICENSE](LICENSE).
