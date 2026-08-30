# Timeliner

A single-file app for building visual timelines of your life: track any custom
metrics day by day, annotate days with short headlines or long journal entries,
and mark multi-day events — all on one scrollable, zoomable chart.

**The whole app is one file: `timeliner.html`.** Double-click it to run; no
install, no server, no build step. It works in any modern browser (Chrome,
Edge, Firefox, Safari) on any device.

## Features

- **Custom metrics, custom scales.** Each metric gets its own lane with its own
  scale — numeric ranges (0–5, 1–6, whatever) or ordered labels
  (`None, Bad, Okay, Good, Great`). Add, reorder, recolor, and remove metrics
  under **Metrics**.
- **Day entries.** Click any day on the chart (or press **N** for today) to log
  metric values and a note — a short headline, a long journal entry, or both.
  Notes appear on the note strip; zoomed in they show as clickable headlines,
  zoomed out they collapse into dots, then into count bubbles (click a bubble
  to zoom into it).
- **Event spans.** Drag on the span strip (below the time axis) to mark
  multi-day events — vacations, medication trials, illnesses. They render as
  colored bars plus a tinted background band. Drag a bar to move it, drag its
  edges to extend it, click it to rename/recolor/delete.
- **Zoom & scroll.** Mouse wheel zooms around the cursor (decades down to
  individual days); drag to pan; trackpad pinch and two-finger scroll work too.
  The axis adapts: year labels → month labels → day numbers with week/month
  tick emphasis.
- **Hover readout.** A crosshair follows the pointer and a tooltip shows every
  metric's value, the note, and any events for that day.
- **Table view** of all entries (click a row to edit that day).
- Light/dark theme (auto-detects, toggle with ◐).

## Saving and loading

- **Autosave:** every change is saved to the browser's local storage
  immediately; closing the tab loses nothing.
- **Save / Open (Ctrl+S / Ctrl+O):** stores the timeline as pretty-printed,
  human-readable JSON (`*.timeliner.json`). Dates are plain `YYYY-MM-DD` keys
  and label values are stored as their text, so the file is easy to read, diff,
  and edit by hand.
- **Export HTML:** writes a *single self-contained HTML file* with the app and
  your data embedded — an interactive snapshot you can open on any device,
  email to someone, or keep as a backup. It's fully functional (zoom, edit,
  save), not just a picture.

### File format sketch

```json
{
  "app": "timeliner",
  "title": "Wellbeing",
  "metrics": [
    { "id": "anx",   "name": "Anxiety", "type": "scale",  "min": 0, "max": 5, "step": 1, "color": "slot:1" },
    { "id": "sleep", "name": "Sleep",   "type": "levels", "levels": ["None", "Bad", "Okay", "Good", "Great"], "color": "slot:4" }
  ],
  "entries": {
    "2026-07-30": {
      "v": { "anx": 2, "sleep": "Good" },
      "note": { "title": "Good week", "body": "Longer journal text…" }
    }
  },
  "spans": [
    { "start": "2026-07-01", "end": "2026-07-09", "label": "Vacation", "color": "slot:3" }
  ]
}
```

`"color"` is either `slot:1`–`slot:8` (the built-in palette, which adapts to
light/dark mode) or any `#rrggbb` hex.

## Google Drive backup (optional)

Settings (⚙) has one-click **Back up now** / **Restore latest** to a
`Timeliner` folder in your Google Drive. Two requirements:

1. **Your own OAuth Client ID** (free, ~5 minutes): at
   [console.cloud.google.com](https://console.cloud.google.com) create a
   project → *APIs & Services → Enable APIs* → enable **Google Drive API** →
   *Credentials → Create credentials → OAuth client ID → Web application* →
   add your origin (e.g. `http://localhost:3000`) to *Authorized JavaScript
   origins*. Paste the client ID into Settings.
2. **Serve the app over http(s)** — Google sign-in doesn't work from `file://`.
   From this folder run either:
   ```
   npx serve
   ```
   or
   ```
   python -m http.server 3000
   ```
   and open `http://localhost:3000/timeliner.html`.

The app only requests the `drive.file` scope, so it can only see files it
created itself.

**Zero-setup alternative:** just Save / Export into your local Google Drive
(or Dropbox/OneDrive) sync folder — same durability, no OAuth.

## Keyboard shortcuts

| Key | Action |
|---|---|
| `←` / `→` | pan (Shift = faster) |
| `+` / `-` | zoom |
| `T` / `Home` | center on today |
| `F` | fit all data |
| `N` | log today |
| `Ctrl+S` / `Ctrl+O` | save / open file |
