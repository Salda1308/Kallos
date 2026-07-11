# kallos-igScheduler

Part of the [Kallos](../README.md) family. Schedules finished Kallos designs to publish automatically to Instagram at specific future dates/times.

Drop your designs in a queue folder, get captions and hashtags written for you, review an auto-distributed calendar, confirm — and each piece publishes itself later without you needing to be online, via one-time cloud triggers that call the Instagram Graph API directly.

## What it does

- Lets you pick a subset of many designs via a numbered list — no typing full filenames one by one
- Writes captions + hashtags per piece, in the tone and length/hashtag-count standard set once in `kallos/brief.md`
- Auto-distributes selected pieces across a schedule pattern you give it (e.g. "lun/mié/vie 9am"), and shows a visual HTML preview to review/adjust before confirming
- For each confirmed post: rasterizes the SVG, uploads it to Cloudinary for a public URL, and creates a one-time cloud trigger (`run_once_at`) that fires exactly at the scheduled time — no polling
- Carousels publish as one native Instagram carousel post, not separate individual posts
- `/kallos-ig` reconciles the calendar against Instagram's real status on demand — reliable, since cloud triggers can't write back to your local files

## Requirements

- Instagram Business/Creator account linked to a Facebook Page, a Meta developer app, and an access token **scoped to content-publishing only** (least privilege — this token is embedded in cloud triggers, which is less private than a local `.env`)
- A free [Cloudinary](https://cloudinary.com/) account (Instagram's API needs a public image URL, not a local file)
- `kallos/.env`: `META_ACCESS_TOKEN`, `IG_BUSINESS_ACCOUNT_ID`, `CLOUDINARY_CLOUD_NAME`, `CLOUDINARY_API_KEY`, `CLOUDINARY_API_SECRET`

## Install

```bash
git clone https://github.com/Salda1308/Kallos.git ~/.claude/skills/Kallos
ln -sfn ~/.claude/skills/Kallos/kallos-igScheduler ~/.claude/skills/kallos-igScheduler
mkdir -p ~/.claude/commands
ln -sfn ~/.claude/skills/Kallos/kallos-igScheduler/commands/kallos-ig.md ~/.claude/commands/kallos-ig.md
```

## Use

Run `/kallos-ig` to initialize `publicaciones/` (or check its status / reconcile against Instagram). Then, in the session:

```
programa lo que hay en publicaciones/cola/, lunes/miércoles/viernes a las 9am
```

See [SKILL.md](SKILL.md) for the full rules Claude follows, including why cloud triggers need everything embedded up front instead of reading local files.
