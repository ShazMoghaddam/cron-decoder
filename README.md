# Cron Expressions, Decoded

An interactive cron decoder. Paste a schedule and it explains every field in plain English and computes the next times it will run, live in the browser. One self-contained HTML page, no build step, no dependencies.

**Live:** https://shazmoghaddam.github.io/cron-decoder/

## What it does

- Explains a cron expression field by field, in plain English, as you type
- Computes the next run times in your local timezone, with relative labels ("in 2h 25m")
- Auto-detects standard 5-field cron and the 6-field form with a leading seconds field
- Handles `*`, ranges, lists, steps, month and day names (JAN, MON), the `@daily`/`@hourly`/etc. shortcuts, and `@reboot`
- Gets the tricky cases right: `day-of-month` and `day-of-week` use the OR rule when both are set, `0` and `7` both mean Sunday, and impossible dates (like Feb 30) report no runs instead of hanging
- Clear inline errors that name the field and the problem
- Clickable presets, a shareable URL (the expression lives in the link), and copy buttons

## How it works

The whole thing is vanilla JavaScript in a single file. The next-run engine steps forward field by field from the current time (month, then day, then hour, minute, second), which converges in a few thousand steps even for a once-a-year schedule, rather than ticking minute by minute. The parser expands each field to a set of matching values, so the plain-English breakdown and the run times come from the same source of truth.

The logic is covered by a set of unit tests (next-run calculations, the day-of-month/day-of-week union rule, seconds handling, named months and days, and the validation paths).

## Scope

This decoder covers the cron grammar shared by Unix/Vixie cron and most schedulers, plus an optional seconds field. It does not implement the Quartz-only operators `L`, `W` and `#`; if you paste one it says so clearly rather than guessing. A `?` in a day field is accepted and treated as `*`, so most Quartz expressions still work.

## Put it live on GitHub Pages

1. Create a new public repo named `cron-decoder`.
2. Add `index.html` and `preview.png` to the root of the repo.
3. Settings, then Pages. Source: "Deploy from a branch", branch `main`, folder `/ (root)`, Save.
4. Give it a minute, then open https://shazmoghaddam.github.io/cron-decoder/

If you name the repo something else, update the three absolute URLs near the top of `index.html` (`canonical`, `og:url`, `og:image`).

## Files

- `index.html` — the page (and the whole app)
- `preview.png` — the 1200x630 card shown when the link is shared

Built by Shaz Moghaddam.
