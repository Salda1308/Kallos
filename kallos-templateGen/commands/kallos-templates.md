---
description: Initialize or check the status of the plantillas/ project folder (entrada/, salida/) in the current directory
---

Check the current working directory for a `plantillas/` folder, as defined by the kallos-templateGen skill (`~/.claude/skills/kallos-templateGen/SKILL.md`).

- **If `plantillas/` does not exist:** create `plantillas/entrada/` and `plantillas/salida/` (empty). Report that it was created and that the user should drop JPGs into `entrada/` before asking to convert them.
- **If `plantillas/` already exists:** report its status — how many JPGs are in `entrada/` and how many SVG templates already exist in `salida/`.

Do not convert any image as part of this command — only initialize or report status. No long explanations beyond the status report itself.
