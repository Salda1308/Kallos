---
name: kallos-igScheduler
description: Use when scheduling Kallos-generated designs to publish automatically to Instagram at specific future dates/times — writing captions and hashtags, building a publishing calendar, and creating one-time cloud triggers that call the Instagram Graph API.
---

# kallos-igScheduler

## Overview
Takes Kallos designs, writes captions/hashtags, builds a publishing calendar, and schedules each piece to actually publish to Instagram at its date/time — via one-time cloud triggers (`run_once_at`), since the Instagram Graph API has no native "publish later" parameter. Cloud triggers run in complete isolation from the local machine, so every publish needs a public image URL and credentials embedded directly in its trigger — never a local file path.

## When to Use
- User wants to schedule one or more finished Kallos designs to post to Instagram automatically on future dates.
- User has many designs and needs to pick a subset without typing every filename.

## Project Structure
```
publicaciones/
  cola/                   # designs (SVGs) the user wants to schedule — drag/copy them in
  calendario.csv           # source of truth: archivo, fecha, hora, caption, estado, id_publicacion
  calendario_preview.html  # visual preview, regenerated on demand — never the source of truth
  publicados/              # archived here after a confirmed successful publish
  fallidos/                # archived here after failing twice — needs manual reprogramming
```
Credentials live in `kallos/.env` (same file as Pexels): `META_ACCESS_TOKEN` (must be scoped to content-publish only — see Security), `IG_BUSINESS_ACCOUNT_ID`, `CLOUDINARY_CLOUD_NAME`, `CLOUDINARY_API_KEY`, `CLOUDINARY_API_SECRET`. If any are missing when needed, stop and ask — never proceed without them.

**Scaffolding:** if `publicaciones/` doesn't exist yet, create `cola/`, `publicados/`, `fallidos/` (empty) and an empty `calendario.csv` with header `archivo,fecha,hora,caption,estado,id_publicacion`. The `/kallos-ig` command runs this scaffolding, reports queue/calendar status, and reconciles pending entries against Instagram's real status (see Status Check).

## Selecting Pieces
- If `cola/` has few files, process all of them.
- If the user wants a subset of a larger folder, list every file with a number and let them reply with numbers/ranges (e.g. "1, 4, 7-9") — never make them type full filenames one by one.

## Writing Captions
- Tone comes from `kallos/brief.md`. Length and hashtag-count follow a standard defined once in the brief (e.g. "copy: 2-3 líneas, 8-10 hashtags"); if the brief has no such standard, ask for it once rather than guessing per batch.

## Building the Calendar
1. Ask for a schedule pattern (e.g. "lun/mié/vie 9am") and auto-distribute the selected pieces across it.
2. Write `calendario.csv` and regenerate `calendario_preview.html` from it for the user to review.
3. Let the user adjust any individual date/time before confirming — `calendario.csv` stays the single source of truth; the HTML is only ever a snapshot for review.

## Publishing Pipeline (on calendar confirmation)
For each confirmed entry:
1. Rasterize the final SVG to PNG (e.g. `rsvg-convert`) — Instagram doesn't accept SVG.
2. Upload the PNG to Cloudinary to get a public HTTPS URL — Instagram's API fetches the image from a URL, it cannot take a local file.
3. Carousels (multiple slides) publish as **one native Instagram carousel post**: create a child container per slide (`is_carousel_item=true`), then a parent container (`media_type=CAROUSEL`, `children=[...]`), then publish the parent. Never publish carousel slides as separate individual posts.
4. Create a one-time cloud trigger (`run_once_at`, converted from the user's local time to UTC — confirm the conversion) via `RemoteTrigger`. Its prompt must be fully self-contained: the Cloudinary URL(s), caption, `IG_BUSINESS_ACCOUNT_ID`, and `META_ACCESS_TOKEN`, plus instructions to create the media container(s), poll until `FINISHED`, call `media_publish`, retry once on failure, and stop (no second retry).
5. Update `calendario.csv`'s `estado` to `programado` and record the trigger/routine ID.

## Status Check
Cloud triggers cannot write back to local files, so `calendario.csv` does not update itself. The `/kallos-ig` command reconciles it: for every `programado` entry whose scheduled time has passed, query the Instagram Graph API directly for that media's real status, then update `estado` to `publicado` (moving the file to `publicados/`) or `fallido` (moving it to `fallidos/`). This manual check is the reliable source of truth — don't rely on the cloud trigger notifying anyone on its own.

## Security
- `META_ACCESS_TOKEN` must be scoped to content-publishing only (no page management, no ads, no messaging). It travels embedded in each cloud trigger's prompt, which is less private than a local `.env` — least privilege is what bounds the risk, not encryption (a cloud trigger has no secure place to keep a decryption key that wouldn't be just as exposed).
- Never embed a broad/high-privilege token to save a step.

## Common Mistakes
- Publishing each carousel slide as its own individual post instead of one native carousel post.
- Passing a local file path as `image_url` — Instagram needs a real public URL; upload to Cloudinary first.
- Assuming the cloud trigger can read `kallos/.env` or any local file at publish time — everything it needs must be embedded in its prompt when created.
- Embedding a high-privilege Meta token instead of one scoped to content-publish only.
- Waiting for an automatic failure notification instead of running the `/kallos-ig` status check.
- Polling on a recurring schedule instead of a one-time `run_once_at` trigger per post — wastes cycles when the exact publish time is already known.
