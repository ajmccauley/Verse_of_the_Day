# Verse of the Day

A tiny dashboard that shows one Bible verse per day. The page at
https://ajmccauley.github.io/Verse_of_the_Day/ reads `message.json` and
displays it — updating that file is all it takes to change the dashboard.

## Files

| File | What it is |
|---|---|
| `index.html` | The dashboard page. Fetches `message.json` and renders the verse. |
| `message.json` | The current verse: `{"message": "...", "reference": "..."}`. This is what the dashboard shows. |
| `messages.md` | Log of every verse ever posted, newest first, with the date each went live. |
| `skill/` | Backup of the automation skill (see below), so it can be rebuilt if anything is lost. |

## How the daily update works

Every morning at **7:00 AM Central**, an automated job:

1. Reads the family's shared **Family Board** calendar plus the user's and
   Cynthia's personal calendars for the day.
2. Picks a verse that fits what's going on — strength for game days, peace
   for hectic days, joy for celebrations, and so on. Every verse is quoted
   from the **ESV** translation.
3. Writes the verse to `message.json` and prepends a dated entry to
   `messages.md`, committed together as one commit.
4. Never repeats a verse until the whole collection has been used, and never
   posts twice for the same date.

If the calendar is empty or nothing fits the day, it falls back to the next
unused verse from the curated pool (`skill/references/verses.json`, 127
verses, all ESV).

The pieces behind the automation:

- **Skill** `verse-of-the-day` — the instructions and scripts that do the
  choosing and committing. A backup lives in `skill/` in this repo
  (`SKILL.md`, `bin/update-verse`, `references/verses.json`); it is
  refreshed with `bin/update-verse --sync-skill` whenever the skill changes.
- **Schedule** — a daily 7:00 AM America/Chicago cron job that runs the
  skill. It works silently; the dashboard updates itself.

## Making changes

- **Add verses**: append `{"reference": "...", "text": "..."}` (ESV) to
  `skill/references/verses.json`, then run `bin/update-verse --sync-skill`
  to refresh the backup.
- **Change the time**: edit the `verse-of-the-day` cron schedule.
- **Change how verses are chosen**: edit the skill's `SKILL.md`, sync the
  backup, and update this README so it keeps describing the system
  accurately.
