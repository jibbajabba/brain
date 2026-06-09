Generate the daily focus brief and write it to `~/Work/brain/Today.html`.

Do this silently — no step-by-step narration. Read, synthesize, write, notify. Report only what was written and where.

## Get today's date first

Run `date '+%A, %B %-d, %Y'` in the shell before doing anything else. Use that output as the date in the HTML header. Never infer the date from session context or memory — always use the shell clock.

## What to read

1. `me/profile.md` — current situation, priorities, active projects, growth edges
2. `me/listening-tour.md` — if it exists, scan for pipeline status: how many emails are out, who has responded, any key insights logged
3. Every file in `people/` (skip `_template.md`) — scan each for open threads and commitments
4. `companies/[client]/[client].md` — current client context and deadlines
5. The 3 most recent files in `companies/[client]/meetings/` — freshest working context
6. Any files in `raw/granola/` dated within the last 7 days
7. Any files in `journal/` dated within the last 7 days — read all that exist

## What to synthesize

From those files, extract:

- **Top 3 priorities for today** — always lead with the job search action (be specific: not "job search" but "send outreach emails" or "follow up with recruiter"). Then client work. Then one other. Never more than 3.
- **The thing he's avoiding** — pull from profile growth edges, open threads, and any `!! ` lines from recent journal entries. Name it plainly. If a specific job search action has been committed to but not done, name it directly every day until it's complete.
- **Open threads** — for each person note that has an unchecked `[ ]` item under "Open threads / commitments", list the name and the item.
- **Hard deadlines** — anything in the profile or meeting notes with a specific date. Always show hard client deadlines until they pass.
- **Journal signals** — from the last 7 days of `journal/` entries: (a) extract any `!! ` lines and fold them into "The thing he's avoiding" — treat them as first-person self-flags, not third-party observations; (b) extract any `→ ` or `-> ` lines as flagged reflections for the FROM YOUR JOURNAL section; (c) if the same theme recurs across multiple entries without resolution, name the pattern plainly; (d) if there are no journal files in the last 3 days, note the gap — absence from the journal is usually avoidance data.
- **One reflection prompt** — one short question drawn from the growth edges in his profile, to give him something to sit with. Rotate them: scope-checking, speaking up, public voice. Don't repeat the same one two days running.

## HTML to write

Write the following to `~/Work/brain/Today.html`. Use the `<head>` block below **verbatim** — do not alter the CSS, title, or favicon. Only the `<body>` content changes each run.

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Daily Brief</title>
  <link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 32 32'><text y='28' font-size='28'>☀️</text></svg>">
  <style>
    :root {
      --bg: #ffffff;
      --fg: #111111;
      --muted: #666666;
      --divider: #dddddd;
      --label: #888888;
    }

    @media (prefers-color-scheme: dark) {
      :root {
        --bg: #0f0f0f;
        --fg: #e8e8e8;
        --muted: #888888;
        --divider: #2a2a2a;
        --label: #666666;
      }
    }

    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      background: var(--bg);
      color: var(--fg);
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif;
      font-size: 17px;
      line-height: 1.55;
      padding: 48px 24px 80px;
    }

    .wrap { max-width: 860px; margin: 0 auto; }

    .header {
      display: flex;
      justify-content: space-between;
      align-items: baseline;
      margin-bottom: 40px;
      flex-wrap: wrap;
      gap: 8px;
    }

    .date {
      font-size: 14px;
      color: var(--muted);
      letter-spacing: 0.04em;
      text-transform: uppercase;
    }

    .page-nav {
      font-size: 14px;
      letter-spacing: 0.06em;
      text-transform: uppercase;
      display: flex;
      align-items: center;
      color: var(--label);
    }

    .page-nav .current {
      color: var(--label);
      border-bottom: 2px solid var(--label);
      padding-bottom: 2px;
    }

    .page-nav a {
      color: var(--label);
      text-decoration: none;
      margin-left: 16px;
    }

    .page-nav a:hover { color: var(--fg); }

    hr {
      border: none;
      border-top: 1px solid var(--divider);
      margin: 32px 0;
    }

    .label {
      font-size: 14px;
      font-weight: 700;
      letter-spacing: 0.08em;
      text-transform: uppercase;
      color: var(--label);
      margin-bottom: 16px;
    }

    .priorities { list-style: none; counter-reset: pri; }

    .priorities li {
      counter-increment: pri;
      display: flex;
      gap: 16px;
      margin-bottom: 14px;
      font-size: 19px;
      line-height: 1.4;
    }

    .priorities li::before {
      content: counter(pri);
      font-size: 14px;
      font-weight: 700;
      color: var(--muted);
      min-width: 16px;
      padding-top: 5px;
      flex-shrink: 0;
    }

    .avoid { font-size: 17px; line-height: 1.6; }

    .threads { list-style: none; display: grid; gap: 12px; }

    .threads li {
      display: flex;
      gap: 12px;
      font-size: 16px;
      line-height: 1.5;
    }

    .threads .person { font-weight: 600; min-width: 140px; flex-shrink: 0; }
    .threads .detail { color: var(--fg); }

    .deadlines { list-style: none; display: grid; gap: 12px; }

    .deadlines li {
      display: flex;
      gap: 16px;
      font-size: 16px;
      line-height: 1.5;
    }

    .deadlines .when {
      font-weight: 600;
      min-width: 72px;
      flex-shrink: 0;
      font-variant-numeric: tabular-nums;
    }

    .count { color: var(--muted); font-size: 14px; }

    .journal-entries { list-style: none; display: grid; gap: 10px; }

    .journal-entries li {
      font-size: 16px;
      line-height: 1.55;
      padding-left: 20px;
      position: relative;
    }

    .journal-entries li::before {
      content: "→";
      position: absolute;
      left: 0;
      color: var(--muted);
    }

    .journal-note { font-size: 16px; color: var(--muted); font-style: italic; }

    .reflection { font-size: 19px; line-height: 1.55; font-style: italic; }

    .footer { margin-top: 56px; font-size: 14px; color: var(--label); }
  </style>
</head>
~~~

Body structure — fill in with synthesized content:

```
[Day, Month DD, YYYY]          DAILY BRIEF
                                        Tasks
─────────────────────────────────────────────

TOP 3 TODAY
1. [priority]
2. [priority]
3. [priority]

─────────────────────────────────────────────

THE THING YOU'RE AVOIDING
[Call it out plainly. One or two sentences. No softening.]

─────────────────────────────────────────────

OPEN THREADS
[Person name] — [what's owed, one line]
...

─────────────────────────────────────────────

HARD DEADLINES
[Date] — [what]
...

─────────────────────────────────────────────

FROM YOUR JOURNAL
[→ flagged reflections from the last 7 days, one per line]
[If no entries in the last 3+ days: "No entries since [day]. That gap is data."]
[If entries exist but no → flags: one plain sentence naming the most recurring theme]

─────────────────────────────────────────────

SIT WITH THIS
[One reflection question]
```

The header uses `.header` with `.date` (left) and `<nav class="page-nav">` (right). `Daily Brief` is `<span class="current">`, `Tasks` is `<a href="Tasks.html">`. Use the classes defined in the CSS above — do not add inline styles.

**FROM YOUR JOURNAL:** Show `→ ` or `-> ` flagged lines from the last 7 days as a list, verbatim — do not rewrite them. If there are no journal files in the last 3 days, show: `No entries since [day]. That gap is data.` If there are entries but no `→ ` or `-> ` flags, write one plain sentence naming the most recurring theme across the entries.

Use the actual synthesized content — not placeholders. Every section must have real content drawn from the vault. If a section is genuinely empty (no open threads, no hard deadlines), say "None this week" rather than omitting it.

## After writing the file

Send a push notification with this format (keep it under 120 characters):
`Daily brief ready — [top priority for today, shortened to fit]`

Then run the `/tasks` skill to regenerate `~/Work/brain/Tasks.html` so both files are current.

Then confirm in chat: tell the user the file was written to `~/Work/brain/Today.html` and remind them to set their browser homepage to:
`file:///Users/[username]/Work/brain/Today.html`
