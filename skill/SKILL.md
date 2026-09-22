---
name: "verse-of-the-day"
description: "Post the daily verse to the Verse_of_the_Day repo (message.json + messages.md) for the user's dashboard, chosen to fit the day's Family Board."
---

# Verse of the Day

## Purpose
Each morning, pick a verse suited to what's on the family's day and publish
it to the `ajmccauley/Verse_of_the_Day` repo, which GitHub Pages serves to the
user's dashboard. The dashboard page reads `message.json`, so updating that
file (and logging the choice in `messages.md`) is the entire update.

## Workflow
1. Read today's calendars (America/Chicago):
   - Family Board (Google):
     `hatch_gws_cli calendar +agenda --calendar "mccauleyfamilyboard@gmail.com" --today --format json`
   - Cynthia's calendar (Google):
     `hatch_gws_cli calendar +agenda --calendar "cynthiamccauley25@gmail.com" --today --format json`
   - The user's personal calendar (Outlook):
     `outlook-calendar list --page-size 50 --time-min "<today>T00:00:00-05:00" --time-max "<tomorrow>T00:00:00-05:00"`
     with `<today>`/`<tomorrow>` as dates like `2026-09-22`/`2026-09-23`.
   Consider all three together when choosing: the Family Board carries the
   kids' activities, the personal calendars add the adults' day (work trips,
   appointments, men's group, etc.).
2. Choose a verse that fits the day. It MUST be quoted in the **ESV**
   translation — no paraphrases, no other translations. If you are not sure
   of the exact ESV wording, pick a verse from the pool in
   `references/verses.json` instead (all 127 are ESV).
3. Post it:
   `bin/update-verse --reference "<reference>" --text "<verse text>"`
   The script refuses a reference already logged in `messages.md` and does
   nothing if today's date already has an entry.
4. If the board is empty or nothing fits the day, run `bin/update-verse`
   with no arguments: it falls back to the next unused pool verse.
5. Report the verse and commit.

### Choosing by the day
- Games / tournaments (soccer, basketball, volleyball, cross country):
  strength, courage, perseverance, running the race — e.g. Philippians 4:13,
  Joshua 1:9, Hebrews 12:1, 1 Corinthians 9:24, 2 Timothy 4:7, Galatians 6:9.
- Packed, hectic, or stressful days (appointments, repairs, deadlines):
  peace, rest, trust — e.g. Matthew 11:28, Philippians 4:6-7, Isaiah 26:3,
  Psalm 46:10.
- Church, Awana, community group, service projects: love, serving,
  fellowship — e.g. 1 Peter 4:10, Hebrews 10:24-25, John 13:34-35.
- School days, curriculum nights, tests: wisdom, diligence — e.g.
  James 1:5, Proverbs 22:6, Colossians 3:23.
- Medical visits: comfort, fear not — e.g. Isaiah 41:10, Psalm 56:3,
  1 Peter 5:7.
- Parties, celebrations, milestones: joy, thanksgiving — e.g. Psalm 118:24,
  1 Thessalonians 5:16-18, Psalm 100:4.
- Family time, potlucks, gatherings: unity, love — e.g. Psalm 133:1,
  1 Corinthians 13:4-7, Colossians 3:14.
- Hard or heavy days: hope, nearness of God — e.g. Romans 8:28,
  Psalm 34:18, John 16:33.
- Quiet days: general encouragement, morning mercies — e.g.
  Lamentations 3:22-23, Psalm 143:8, Psalm 19:14.

## Auth
Uses the `github` skill's `bin/gh-api` CLI, which carries the stored
`custom.github` credential. Never ask for or handle the raw token.

## Keeping the repo backup current
The repo holds a backup of this skill under `skill/` plus a `README.md`
explaining the system. Whenever this skill changes (workflow, script, or
verse pool):
1. Run `bin/update-verse --sync-skill` to refresh the backup.
2. Update `README.md` in the repo if the explanation changed, and commit it.

## Operating Rules
1. One verse per day; never post twice for the same date.
2. Every verse is ESV, always.
3. Never repeat a reference until every pool verse has been used once
   (the script enforces this for explicit picks too).
4. Keep `message.json` to exactly the `message`/`reference` schema the
   dashboard page expects.
5. Prepend new log entries at the top of `messages.md` (newest first),
   matching the existing `## YYYY-MM-DD` / `- reference` / `  - text` format.
6. Commit verse updates as `Verse of the day — YYYY-MM-DD (Reference)`;
   skill backups as `Sync skill backup`.
7. To add verses, append `{"reference", "text"}` objects (ESV) to
   `references/verses.json`.
