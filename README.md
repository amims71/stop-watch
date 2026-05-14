# Activity Stopwatch — Contrived Free-Operant Recorder

A mobile-first stopwatch app for quantifying how long a child engages with each of several activities while you review a video of a behavioral observation session.

**Live:** https://amims71.github.io/stop-watch/

---

## Background — Contrived Free-Operant Observation

In Applied Behavior Analysis (ABA), a **free-operant** observation is one in which the subject is free to start, stop, switch, or do nothing — there are no discrete trials or prompts. The observer simply records what happens.

A **contrived** free-operant observation differs from a **natural** one in that the environment is *deliberately arranged*: the observer presents a chosen set of materials or activities, and lets the child engage (or not) with whatever they like. This is the standard setup for:

- Free-operant preference assessments (e.g., Roane et al., 1998)
- Engagement / interest analyses
- Activity-allocation studies in a controlled but unprompted context

The measurement of interest is typically:

- **Total duration** spent on each item / activity (primary)
- **Frequency** of engagement bouts (secondary)
- **Per-bout duration** — how long each individual bout lasted

## What this tool does

This is a coding aid for the post-hoc review step: you have a video of the session, and you need to play it back and quantify engagement with each activity.

For every activity you set up:

- Tap to start a bout, tap again to stop. Multiple activities can run simultaneously (a child can engage with two things at once).
- Playback-speed aware — if you watch at 1.25× or 2×, the stopwatch scales accordingly so totals reflect *real* engagement duration.
- Output: total duration per activity, bout count, and a per-bout breakdown (start/end timestamps in video time, with the speed used).

## Interaction model

Each activity is a large tile, split into two tap zones — designed for blind-tap operation so your eyes stay on the video.

| Tap zone | What it does |
|---|---|
| **Left half (`+`)** | Toggle this activity on/off. Other running activities are unaffected. Use when the child adds a new behavior on top of one already in progress. |
| **Right half (`▶`)** | Solo — stop all other activities, run only this one. Use when the child cleanly switches from one activity to another. |
| **Idle bar (bottom)** | Stop everything. Use when the child is doing nothing of interest. |
| **Speed pill (top)** | Cycle 1× → 1.25× → 1.5× → 1.75× → 2×. Changes split ongoing bouts mid-stream so the math stays correct. |

Distinct haptic patterns confirm each action (toggle / solo / stop-all), so a single tap registers without looking.

## How it helps

Manual stopwatch + paper coding for free-operant duration data is slow and error-prone. This tool removes the friction of:

- **Multiple simultaneous timers** — most stopwatches handle one channel at a time.
- **Speed-adjusted playback** — scaling is automatic; no spreadsheet arithmetic at the end.
- **Blind-tap UI** — large split tiles + haptic feedback let you log transitions without looking away from the video.
- **Multi-session history** — every video gets its own titled record, listed on the home screen, all stored locally in `localStorage` (key: `stopwatch.sessions.v1`). Refresh, tab close, or phone lock will not wipe your data.
- **Built-in reporting** — total time, percentage, bout count, and per-bout timestamps appear in the summary view without manual aggregation.

## Data export

The summary screen has an **Export CSV** button (this session only). The home screen has an **Export all (CSV)** link (every saved session). The CSV is long-format with one row per bout — pivot in Excel / Sheets / SPSS for totals and percentages.

Columns:

```
session_title, session_started, activity, bout_number,
start_video_time, end_video_time, duration_seconds, speed
```

- `start_video_time` and `end_video_time` are speed-adjusted video time in `mm:ss`.
- `duration_seconds` is an integer seconds count.
- `speed` records the playback speed used during that bout (or `mixed` if the speed changed mid-bout).

## Reset / clear

- **Delete session** (summary screen) — removes the current session only; other history is preserved.
- **Clear all history** (home screen footer link) — wipes every saved session.

Both actions require confirmation.

## Tech

- Single static `index.html` — no build step, no dependencies.
- Deployed via GitHub Pages.
- Works offline after first load (everything is one file).
