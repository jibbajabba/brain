Prepare the user for an upcoming conversation. The subject is: $ARGUMENTS

The subject may be a person's name, a company, or both (for example "[client] standup" or "Jane Doe"). If it's empty, ask who or what the meeting is with.

## Get today's date first

Run `date '+%Y-%m-%d (%A)'` in the shell before doing anything else. Use that output for all date resolution below. Never infer today's date or weekday from session context or memory — always use the shell clock.

1. If a person is named, read their note in `people/`. If the note doesn't exist or is thin, say so and ask the user for context rather than inventing any.
2. If a company is named or implied (for example the person works at [client]), read that company note (for example `companies/[client]/[client].md`) and the two or three most recent notes in its `meetings/` folder.
3. Read `me/profile.md` for the user's goals and what he's optimizing for, so the prep serves his actual interests.
4. If the Granola MCP tools are available, you may query them live for the person's most recent calls, to catch context that hasn't been processed into the vault yet. Use only what the tools return.

Then produce a short prep brief in the chat. Keep it scannable:

- **Who and why** — one line on the person and the relationship, and what this conversation is likely about.
- **How to handle them** — how they communicate and what lands with them, drawn only from their note.
- **What they care about** — their priorities, so the user can frame things their way.
- **Open threads** — what the user owes them and what they owe the user. Flag anything outstanding that the user committed to, so he doesn't walk in having dropped a ball.
- **What to raise** — two or three specific things to bring up or ask, tied to the open threads and to the user's goals. Draft these in the user's voice.
- **Watch out for** — any risk, tension, or sensitivity worth keeping in mind.
- **Suggested agenda** *(omit for informal or sensitive 1:1s)* — 3–5 talking points in the order the user should raise them, loosely time-boxed if useful. Lead with the item that most serves his goals or closes the most important open thread. Draft each item as a one-line prompt in his voice.

Base everything on what the notes actually contain. Never invent people, history, or commitments. If a section has no information, say so plainly and suggest what the user should fill in.

After producing the brief, save it as a file:

- Infer the meeting date from the subject: if a day name is given (e.g. "Tuesday"), resolve it to the nearest upcoming date as YYYY-MM-DD, counting forward from the shell `date` output above (if the named day is today, use today). If no day is given, use today's date from that same output.
- Determine the correct `meetings/` folder from the company context (e.g. `companies/[client]/meetings/`). For a person-only prep with no company, save to `raw/prep/`.
- Filename: `YYYY-MM-DD-[slugified-subject]-prep.md` (e.g. `2026-06-03-acme-tuesday-standup-prep.md`).
- File structure: YAML frontmatter with `type: prep`, `date`, and `subject`, followed by the full brief content exactly as shown in chat.
- Tell the user where the file was saved.
