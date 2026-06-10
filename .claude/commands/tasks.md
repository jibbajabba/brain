View and update open tasks across the vault, then write the full list to `~/Work/brain/Tasks.html`.

Input: $ARGUMENTS

- If empty: scan all vault files, display the task list in chat, and write the HTML file.
- If it starts with `done:` — find the closest matching unchecked task and mark it `[x]` in the source file, then re-scan and regenerate the HTML. If multiple tasks match, ask the user to clarify before making any change.
- If it starts with `wontdo:` — find the closest matching unchecked task and mark it `[-]` in the source file (decided not to do / cancelled), then re-scan and regenerate the HTML. If multiple tasks match, ask the user to clarify before making any change.
- If it starts with `add:` — parse the format `add: [destination] | [task text]` where destination is a person's name or the client slug. Add a new `- [ ] task text` line to the appropriate Open threads section in `people/[firstname-lastname].md` or `companies/[client]/[client].md`, then re-scan and regenerate.

## Step 0 — Get today's date

Run `date '+%A, %B %-d, %Y'` in the shell before anything else. Use that output for the HTML header, the `Updated:` line, and any "last N days" cutoffs. Never infer the date from session context or memory — always use the shell clock.

## Step 1 — Scan for open tasks

Read these files and extract every unchecked `- [ ]` item (skip `[x]` done and `[-]` wontdo items):

1. `companies/[client]/[client].md` — Open threads section
2. Every file in `people/` (skip `_template.md`) — Open threads / commitments section
3. Every meeting file in `companies/[client]/meetings/` dated within the last 45 days — Action items section, but only items where **the user** is the listed owner

For each item, record:
- The task text
- The source file and section
- The owner / context label (name of person file, client name, or "Me")

## Step 2 — Categorize

Sort every task into one of three buckets:

**MY ACTIONS** — the user is the owner. This includes:
- Any action item from meeting notes where the user is the listed owner
- Any task in a person note that says the user owes something to that person
- Any `- [ ]` item in `me/profile.md`

**WAITING ON** — Someone else owes the user something. These are items in person notes that describe something another person committed to, is responsible for, or owes the user. Key signals: "to send", "to share", "to provide", "to schedule", "to follow up" with a third party as subject.

**CLIENT THREADS** — Open items in the client company file that are not clearly owned by a single person, or represent company-level work in progress.

When in doubt between MY ACTIONS and WAITING ON, put the item under whichever person's file it came from and label it with their name.

## Step 3 — Display in chat

Print the grouped list clearly. Use this format:

```
MY ACTIONS (N)
☐ [task] — [source: person or meeting date]
...

WAITING ON (N)
☐ [task] — [who]
...

CALX THREADS (N)
☐ [task]
...
```

## Step 4 — Write ~/Work/brain/Tasks.html

Write the following to `~/Work/brain/Tasks.html`. Use the `<head>` block below **verbatim** — do not alter the CSS, title, or favicon. Only the `<body>` content changes each run.

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Tasks</title>
<link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 32 32'><text y='28' font-size='28'>✅</text></svg>">
<style>
  :root {
    --bg: #f9f9f7;
    --fg: #1a1a1a;
    --muted: #666666;
    --label: #888888;
    --rule: #d0d0cc;
    --check-done: #aaa;
    --section-head: #1a1a1a;
  }
  @media (prefers-color-scheme: dark) {
    :root {
      --bg: #0f0f0f;
      --fg: #e8e8e8;
      --muted: #888888;
      --label: #555555;
      --rule: #2a2a2a;
      --check-done: #555;
      --section-head: #e8e8e8;
    }
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    background: var(--bg);
    color: var(--fg);
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif;
    font-size: 16px;
    line-height: 1.6;
    padding: 48px 24px;
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
  }
  .page-nav a,
  .page-nav .current {
    color: var(--label);
    text-decoration: none;
    padding-bottom: 2px;
  }
  .page-nav a:hover { color: var(--fg); }
  .page-nav .current { border-bottom: 2px solid var(--label); }
  .page-nav a,
  .page-nav .current { margin-left: 16px; }

  hr { border: none; border-top: 1px solid var(--rule); margin: 28px 0; }

  .section-head { display: flex; align-items: baseline; gap: 10px; margin-bottom: 16px; }
  .section-head h2 {
    font-size: 14px;
    font-weight: 700;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    color: var(--section-head);
  }
  .section-head .count {
    font-size: 14px;
    color: var(--muted);
    font-variant-numeric: tabular-nums;
  }

  .task-list { list-style: none; }
  .task-list li {
    display: flex;
    align-items: baseline;
    gap: 10px;
    padding: 5px 0;
    border-bottom: 1px solid var(--rule);
  }
  .task-list li:last-child { border-bottom: none; }

  .cb { flex-shrink: 0; font-size: 16px; color: var(--muted); padding-top: 1px; }
  .task-text { flex: 1; font-size: 16px; }
  .source {
    flex-shrink: 0;
    font-size: 14px;
    font-family: "SF Mono", "Fira Code", "Fira Mono", monospace;
    color: var(--muted);
    white-space: nowrap;
    padding-left: 12px;
  }

  .group-label {
    font-size: 14px;
    font-weight: 600;
    letter-spacing: 0.04em;
    text-transform: uppercase;
    padding: 14px 0 6px 0;
    color: var(--muted);
    border-bottom: 1px solid var(--rule);
    margin-bottom: 0;
  }

  .footer {
    margin-top: 32px;
    font-size: 14px;
    color: var(--muted);
    letter-spacing: 0.04em;
  }
</style>
</head>
~~~

Add per-person color variables to `:root` for each person in the WAITING ON section (e.g. `--robin: #5a3e8a`) and apply them via `.group-label.robin { color: var(--robin); }` etc. Include dark-mode variants in the `@media` block.

Body structure — fill in with real task data:

```
[Day, Month DD, YYYY]     [NAME] — TASKS
                                    Today
─────────────────────────────────────────────────

MY ACTIONS  (N)
☐ task text                     source label
☐ task text                     source label

─────────────────────────────────────────────────

WAITING ON  (N)
☐ task text                     who
...

─────────────────────────────────────────────────

CLIENT THREADS  (N)
☐ task text
...

─────────────────────────────────────────────────

Updated: [YYYY-MM-DD]
```

The header uses `.header` with `.date` (left, today's date) and `<nav class="page-nav">` (right). `Daily Brief` is `<a href="Today.html">`, `Tasks` is `<span class="current">`. Use the classes defined in the CSS above — do not add inline styles.

Use real content from the scan. If a section is genuinely empty, show "None" rather than omitting the section.

## After writing

Confirm in chat:
- File written to `~/Work/brain/Tasks.html` (or that it was updated)
- Total open task count across all three sections
- Any updates that were made (if `done:` or `add:` was used)
- Remind the user they can bookmark: `file:///Users/[username]/Work/brain/Tasks.html`
