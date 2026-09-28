# End City Room Locator

A Minecraft (The End) themed web tool that finds empty classrooms and labs from class timetables. Built for the **VibeCraft Nether Round 2 - "The Free Class Locator"** problem statement.

Rooms are shulker boxes stacked as End City floors: **open and glowing = free**, **closed and dim = class in session**.

## Features

- **Floor Grid** - every room shown floor by floor (Ground to Floor 6), with live free/busy status and how long each state lasts.
- **Ender Oracle** - a natural-language bar. Type *"I need an AC room on the ground floor for the next 2 hours"* and it extracts day, time, duration, floor, AC and lab/classroom, then lists matching rooms that stay free the whole time, longest first. If nothing matches, it shows the closest alternatives.
- **Room Log** - click a room to see its full day, period by period, with the class and section in each slot.
- **Filters** - day, time, floor, room type, AC only, minimum free time.
- **Live mode** - follows the real clock and refreshes every 30 seconds.
- **Ender Pearl** - teleports you to a random room that is free for at least an hour.

## Run locally

No build step and no dependencies. Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

Fonts (Press Start 2P, VT323) load from Google Fonts; the app falls back to monospace offline.

## Deploy to GitHub Pages

**Option A - included workflow:** push to `main`, then go to *Settings -> Pages -> Source* and choose **GitHub Actions**. `.github/workflows/pages.yml` deploys on every push.

**Option B - no workflow:** *Settings -> Pages -> Deploy from a branch -> main / (root)*. You can delete the `.github` folder.

## How the data works

The timetables are transcribed into the `SEC` array in `index.html`, one entry per section:

```js
{n:'II BME', h:'602', y:2, s:'subject A|subject B|...|subject F', d:[ /* Mon..Fri */ ]}
```

Each day is 9 space-separated period tokens:

| Token | Meaning |
|-------|---------|
| `E` (A-I) | slot class in the section's home room `h` |
| `.` | no class |
| `625:G` | slot G held in room 625 |
| `107,309:Lab` | lab session using rooms 107 and 309 |
| `P` | project hour in the home room |

Period times: P1 9:00, P2 9:50, P3 10:50, P4 11:40, P5 lunch 12:30, P6 1:20, P7 2:10, P8 3:10, P9 4:00, day ends 4:50pm.

**Sections included (2026-27 odd sem, 9):** II BME, II ECE-DS A/B, III BME, III ECE A/B/DS, IV ECE A/B.
The I-year sheets are 2024-25 and list no rooms, so they are excluded.

### Assumptions

- A room's **floor is the first digit** of its number (1xx = ground).
- A section uses its home room only in the half of the day marked on its sheet (FN or AN), so two sections can share one room (e.g. III ECE-A and B in 518).
- **AC rooms are placeholders.** The timetables contain no AC data. Edit the `AC` set in `index.html`. Labs are in the `LAB` set.
- Room capacity is not in the data, so team size is ignored.
- The Oracle is a local rule-based parser (no API key, works offline).

## Updating for a new semester

Edit the `SEC` array (and the `AC` / `LAB` sets if rooms change). Nothing else needs to change.

## Tech

Plain HTML, CSS and JavaScript in a single file.

## License

MIT - see [LICENSE](LICENSE).
