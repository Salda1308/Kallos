---
description: Initialize or check the status of the kallos/ project folder (brief.md, referencias/, output/) in the current directory
---

Check the current working directory for a `kallos/` folder, as defined by the Kallos skill (`~/.claude/skills/Kallos/SKILL.md`).

- **If `kallos/` does not exist:** create `kallos/referencias/` and `kallos/output/` (empty), and copy `~/.claude/skills/Kallos/templates/brief.template.md` to `kallos/brief.md`. Report that it was created and that the user needs to fill in `brief.md` and drop reference files (images/PDFs) into `referencias/` before asking for a design.
- **If `kallos/` already exists:** report its status — whether `brief.md` still looks unfilled (matches the template placeholders), how many files are in `referencias/`, and how many pieces already exist in `output/`.

Do not generate any design as part of this command — only initialize or report status. No long explanations beyond the status report itself.
