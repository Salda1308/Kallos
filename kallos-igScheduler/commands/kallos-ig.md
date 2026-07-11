---
description: Initialize or check the status of the publicaciones/ folder (cola/, calendario.csv, publicados/, fallidos/), and reconcile pending entries against Instagram's real publish status
---

Check the current working directory for a `publicaciones/` folder, as defined by the kallos-igScheduler skill (`~/.claude/skills/kallos-igScheduler/SKILL.md`).

- **If `publicaciones/` does not exist:** create `publicaciones/cola/`, `publicaciones/publicados/`, `publicaciones/fallidos/` (all empty), and `publicaciones/calendario.csv` with header `archivo,fecha,hora,caption,estado,id_publicacion`. Report that it was created and that the user should drop designs into `cola/` before scheduling.
- **If `publicaciones/` already exists:** first, for every row in `calendario.csv` with `estado=programado` whose scheduled date/time has already passed, query the Instagram Graph API (using `META_ACCESS_TOKEN` and `IG_BUSINESS_ACCOUNT_ID` from `kallos/.env`) for that post's real status; update `estado` to `publicado` or `fallido` accordingly, moving the corresponding file to `publicaciones/publicados/` or `publicaciones/fallidos/`. Then report full status: how many files are in `cola/`, and a breakdown of `calendario.csv` rows by `estado` (pendiente/programado/publicado/fallido).

Do not select pieces, write captions, build a calendar, or schedule anything as part of this command — only initialize or reconcile/report status. No long explanations beyond the status report itself.
