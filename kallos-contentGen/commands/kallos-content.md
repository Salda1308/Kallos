---
description: Initialize or check the status of the kallos/ project folder (brief.md, referencias/designs, referencias/assets, output/) in the current directory
---

Check the current working directory for a `kallos/` folder, as defined by the kallos-contentGen skill (`~/.claude/skills/kallos-contentGen/SKILL.md`).

- **If `kallos/` does not exist:** create `kallos/output/`, `kallos/referencias/assets/img/`, `kallos/referencias/assets/logo/`, and `kallos/referencias/assets/contenido/` (all empty), and copy `~/.claude/skills/kallos-contentGen/templates/brief.template.md` to `kallos/brief.md`. Do not pre-create `kallos/referencias/designs/` subfolders — those are created on demand per piece type. Report that it was created and that the user needs to fill in `brief.md`, drop assets into `referencias/assets/`, and organize any content sources into `referencias/assets/contenido/`.
- **If `kallos/` already exists:** report its status — whether `brief.md` still looks unfilled (matches the template placeholders), what piece-type subfolders exist under `referencias/designs/` and how many files each has, what's in `referencias/assets/img/` and `referencias/assets/logo/`, what subfolders exist under `referencias/assets/contenido/`, and how many pieces already exist in `output/`.

Do not generate any design as part of this command — only initialize or report status. No long explanations beyond the status report itself.
