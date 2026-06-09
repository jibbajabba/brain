# Claude Code Second Brain

This is a personal knowledge vault. It's a set of plain markdown files that Claude Code maintains for you, built on Andrej Karpathy's LLM-wiki idea. It does two jobs: it preps you for and follows up on calls with colleagues, clients, and anyone else you meet with, and it keeps a current picture of you so Claude's coaching and writing sound like you.

The rule behind it: the files are the source of truth. Claude reads raw inputs, writes clean structured notes, and keeps them current over time. No app lock-in, no database.

## How it's organized

- `me/profile.md` — who you are, what you're optimizing for, and patterns worth remembering. This is what makes the coaching personal.
- `people/` — one note per person: how they communicate, what they care about, what you owe them. `_template.md` is the starting shape.
- `companies/` — one folder per company. `[client]/` holds the company note, `meetings/` for processed notes, and `transcripts/` for raw Zoom files. `personal/` holds peer and personal calls — conversations with no company affiliation.
- `journal/` — daily journal entries, one file per day (`YYYY-MM-DD.md`). Free-form. The daily brief reads these and surfaces flagged reflections and patterns.
- `raw/` — an inbox for anything unprocessed. `raw/granola/` is where Granola calls land.
- `.claude/commands/` — the slash commands below.
- `CLAUDE.md` — the rules Claude follows, including your voice and style. Edit this to change how the system behaves.

## The commands

Run these from this folder (`~/Work/brain`) inside Claude Code.

- `/focus` — generates `Today.html` (the daily brief) and `Tasks.html`.
  - Reads the profile, all people notes, client context, recent Granola files, and any journal entries from the last 7 days
  - Writes the brief: top 3 priorities, the thing being avoided, open threads, hard deadlines, journal reflections, and a reflection prompt
  - Then regenerates `Tasks.html` automatically so both files are current after every run
  - Also runs automatically every weekday at 8am — see below

- `/prep [person or meeting]` — prepares you for an upcoming call.
  - Reads the person's note, the company note, recent meetings, and live Granola context if connected
  - Produces a brief in chat: who they are, how to handle them, open threads, what to raise, suggested agenda
  - Saves a prep file to the matching `meetings/` folder (e.g. `/prep Acme Tuesday standup`)
  - Day names resolve to the nearest upcoming date automatically

- `/granola [optional scope]` — pulls recent Granola meetings into `raw/granola/` as markdown.
  - Defaults to the last 7 days; accepts a date, count, or search term to narrow scope
  - Requires the Granola MCP connected

- `/meeting <path or description>` — processes a raw transcript or meeting note into a structured meeting note.
  - Accepts Zoom `.vtt` transcripts, Google Meet exported notes, Granola notes, or plain text
  - If you have no file, just give it a name or description and it'll ask for your notes
  - If the company folder doesn't exist yet, creates it automatically with `meetings/` and `transcripts/`
  - Personal and peer calls (no company) route to `companies/personal/` and are written without job-seeking framing
  - Updates the relevant people notes, company note, and `me/profile.md`
  - If a prep file exists for the same date, it's merged in automatically and the standalone prep file is deleted
  - After processing, asks whether to delete the source transcript

- `/person [name]` — creates or updates a person note in `people/`.
  - Asks for role, relationship, communication style, and open commitments in a single exchange

- `/tasks` — scans every open `[ ]` item across the vault and writes `Tasks.html`.
  - Three sections: things you need to do, things you're waiting on (grouped by person), and client-level open threads
  - `/tasks done: [partial text]` — checks off a matching task in its source file, then regenerates
  - `/tasks wontdo: [partial text]` — marks a task `[-]` (decided not to do) in its source file, then regenerates
  - `/tasks add: [person or client] | [task text]` — adds a task to the right place, then regenerates
  - Runs automatically at the end of every `/focus` run — you rarely need to invoke it directly
  - Run manually after any `/meeting` to keep the task list current
  - Bookmark: `file:///Users/[username]/Work/brain/Tasks.html`

- `/journal [optional]` — creates or edits a daily journal entry in `journal/YYYY-MM-DD.md`.
  - No argument: opens today's file for a writing session
  - Quick note as argument: appends it directly without ceremony
  - Date as argument: opens that day's entry
  - Or skip the command entirely — create the file manually in any editor, it works the same way

## Journaling

Journal entries live in `journal/YYYY-MM-DD.md` — one file per day, free-form prose. Create them manually in any editor, or use `/journal` to open a writing session in Claude Code.

Two signal conventions the daily brief reads:

**`→ ` or `-> ` (flagged reflection)** — start a line with either to mark something worth carrying forward. These appear verbatim in the FROM YOUR JOURNAL section of `Today.html`. Use them for insights, patterns you noticed, or anything you want the coach layer to keep surfacing.

```
→ I keep avoiding [thing] when [other thing] feels hard. That's not a coincidence.
-> Same pattern again today.
```

**`!! ` (urgent flag)** — start a line with `!! ` to name something important you're avoiding or worried about. These get folded directly into "The Thing You're Avoiding" in the brief and treated as first-person self-reports, not softened or reframed.

```
!! Three weeks since I did [important thing]. Not okay.
```

Everything else is just prose — Claude reads it for context and recurring themes but doesn't require any structure. If the same thing comes up across multiple entries without resolution, the brief names the pattern.

If there are no entries for 3 or more days, the brief calls the gap out by name. Disappearing from the journal is usually avoidance data.

## The daily brief

`Today.html` is the operating dashboard. Open it in a browser — set it as your homepage so it loads automatically.

```
file:///Users/[username]/Work/brain/Today.html
```

The brief regenerates every weekday at 8am via a LaunchAgent. Run `/focus` manually anytime to regenerate mid-day.

To set it up on a new machine:

**1. Create the LaunchAgent**

Save this to `~/Library/LaunchAgents/com.user.brain.focus.plist`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.user.brain.focus</string>
    <key>ProgramArguments</key>
    <array>
        <string>/Users/[username]/.local/bin/claude</string>
        <string>--dangerously-skip-permissions</string>
        <string>-p</string>
        <string>/focus</string>
    </array>
    <key>WorkingDirectory</key>
    <string>/Users/[username]/Work/brain</string>
    <key>StartCalendarInterval</key>
    <array>
        <dict><key>Hour</key><integer>8</integer><key>Minute</key><integer>0</integer><key>Weekday</key><integer>1</integer></dict>
        <dict><key>Hour</key><integer>8</integer><key>Minute</key><integer>0</integer><key>Weekday</key><integer>2</integer></dict>
        <dict><key>Hour</key><integer>8</integer><key>Minute</key><integer>0</integer><key>Weekday</key><integer>3</integer></dict>
        <dict><key>Hour</key><integer>8</integer><key>Minute</key><integer>0</integer><key>Weekday</key><integer>4</integer></dict>
        <dict><key>Hour</key><integer>8</integer><key>Minute</key><integer>0</integer><key>Weekday</key><integer>5</integer></dict>
    </array>
    <key>StandardOutPath</key>
    <string>/Users/[username]/Work/brain/.claude/focus.log</string>
    <key>StandardErrorPath</key>
    <string>/Users/[username]/Work/brain/.claude/focus.error.log</string>
    <key>EnvironmentVariables</key>
    <dict>
        <key>HOME</key>
        <string>/Users/[username]</string>
        <key>PATH</key>
        <string>/Users/[username]/.local/bin:/usr/local/bin:/usr/bin:/bin</string>
    </dict>
</dict>
</plist>
```

Update the `claude` path if needed (`which claude` to find it).

**2. Load it**

```bash
launchctl load ~/Library/LaunchAgents/com.user.brain.focus.plist
```

**3. Set a wake schedule**

The Mac needs to be awake at 8am. Set a repeating weekday wake at 7:56am:

```bash
sudo pmset repeat wake MTWRF 07:56:00
```

`MTWRF` = Monday–Friday. Day letters: `M T W R F S U` (R = Thursday, U = Sunday).

**Test it**

```bash
launchctl start com.user.brain.focus
```

Output goes to `.claude/focus.log`. Check `launchctl list com.user.brain.focus` for exit status.

## The daily loop

**Morning**
- Open the browser — the brief regenerates automatically at 8am and is already there
- Review: top 3 priorities, the thing you're avoiding, open threads, hard deadlines, journal flags
- Run `/focus` manually anytime to regenerate mid-day

**Before a call**
- `/prep [person or topic]` — produces a brief in chat and saves a prep file to the meetings folder
- Examples: `/prep Jane Doe`, `/prep Acme Tuesday standup`

**After calls**
- `/granola` — pulls new Granola notes into `raw/granola/`
- `/meeting raw/granola/<file>` — processes each one into a structured note; or say "process all new Granola notes" to batch them
- `/tasks` — re-scan and regenerate `Tasks.html` with any new action items

**Journaling**
- Create `journal/YYYY-MM-DD.md` in any editor and write freely, or use `/journal`
- `→ ` or `-> ` at the start of a line flags a reflection — surfaces in the next brief
- `!! ` at the start of a line flags something urgent — appears in "The Thing You're Avoiding"

## Running it

```
cd ~/Work/brain
claude --model claude-opus-4-8
```

Opus is the recommended model for this work. If you start in Sonnet, `/model opus` switches without losing context.

## If you ever need to reconnect Granola

```
claude mcp add --transport http granola https://mcp.granola.ai/mcp
```

Then run `/mcp` in a session to sign in through the browser. If the Granola tools stop showing up, reconnect the connector and re-authenticate.

## Privacy reminders

- Granola trains on de-identified data by default. Opt out in Granola Settings.
- Keep offer negotiations and other sensitive calls out of Granola entirely. Take those by hand into the vault.
- The notes here are private. If you push this to a git remote, keep it private or uncomment the transcript-ignore line in `.gitignore`.
- Recording laws vary by location. Know your local consent requirements before recording calls.

## Where to change things

Behavior and voice live in `CLAUDE.md`. Who you are lives in `me/profile.md`. Update those and everything downstream follows.

---

Created by [Michael Angeles](https://michaelangeles.com).
