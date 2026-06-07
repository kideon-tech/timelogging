# TimeLog — Design Spec

**Date:** 2026-06-07
**Status:** Approved-pending-review

## Purpose

Personal, browser-only time tracking. Sit down, work, get pinged every 15 minutes
for what you're working on, build up the day as labeled blocks, scroll through past
days in a Google-Calendar-style view, export to Excel. No backend, no login, runs as
a single static page on GitHub Pages (and locally by double-clicking the file).

Mindset: Pomodoro/Pomodoro-Timer-adjacent, but passive — for tracking and review,
not active steering.

## Constraints

- Pure static, single `index.html`. HTML + CSS + JS inline.
- Deployable to GitHub Pages; also works opened directly from disk (`file://`).
- Survives reload (data in `localStorage`).
- No backend, no auth.
- MIT licensed, open source.

## Tech

- One `index.html`, everything inline.
- `localStorage` for persistence.
- SheetJS (xlsx) via CDN for `.xlsx` export.
- No build step, no framework.

## Data Model

`localStorage` key `timelog.v1`:

```jsonc
{
  "blocks": [
    { "id": "uuid", "start": "2026-06-07T09:00:00", "end": "2026-06-07T09:15:00", "label": "Instalon" }
  ],
  "recentLabels": ["Instalon", "Email", "Meeting"],   // most-recent-first, capped ~12
  "settings": { "intervalMin": 15, "soundOn": true, "notifyOn": true }
}
```

- Each block = one 15-minute slot, aligned to slot boundaries (`:00/:15/:30/:45`).
- Empty slots are simply absent from `blocks` (= pause/AFK).
- Color is derived from the label via deterministic hash → same activity, same color.
  Not stored.

## Timer / Ping

- Real-time `setInterval`, fires on the next slot boundary, then every `intervalMin`.
- Runs continuously regardless of focus/idle (user choice).
- Ping = sound + in-tab modal + OS notification (Notification API, permission
  requested on first start).
- Modal "Woran arbeitest du gerade?":
  - Text input.
  - Quick-pick buttons for `recentLabels`.
  - "Weiter wie eben" button → reuses the label of the most recent block.
  - Submitting writes a block for the slot that just ended and refreshes `recentLabels`.

## Catch-up (missed pings)

- On tab return (`visibilitychange`) and on load, compute empty slots between the last
  recorded block and now.
- **Cap at the last ~2 hours** — older gaps are ignored (assumed off / break).
- Catch-up flow lets the user:
  - Label each gap slot, or
  - "Alle = X" bulk-fill, or
  - "Leer lassen" (leave empty = pause/AFK).

## 3-Day View (main screen)

- 3 day-columns, Google-Calendar style, vertical time axis.
- Blocks rendered in their slots, colored by label.
- Navigation: ◀ / ▶ to shift the 3-day window, "Heute" button, date picker to jump.
- Current slot highlighted; a "now" line marks current time.
- Click a block → edit label / delete.
- Reasonable visible time range (e.g. 06:00–22:00) with scroll for the rest.

## Export

- Button opens export dialog.
- Default scope: **all data**, with optional from–to date filter.
- Output: `.xlsx` with columns `Datum | Start | Ende | Dauer | Label`.

## Out of Scope (YAGNI)

- Login / multi-user / sync across devices.
- Drag-to-resize / drag-to-create blocks (edit via click is enough for v1).
- Reporting / charts / aggregation dashboards.
- Configurable interval UI beyond the stored setting (default 15).

## License

MIT.
