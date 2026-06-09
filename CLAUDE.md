# CLAUDE.md — Operating instructions for this vault

**At the start of every session, read `me/profile.md`.** It contains who the user is, what they're optimizing for, their voice and writing style, and how to coach them. Everything in this vault depends on that context — don't wait to be asked.

This is your personal knowledge vault. It is a living, file-based "second brain" in the Karpathy LLM-wiki style: the markdown files in this folder are the source of truth, and you (Claude) maintain them. You read raw inputs, compile and update structured notes, and keep them interlinked. Plain markdown, no database.

The vault exists to do two things well. First, help you prepare for and follow up on conversations with coworkers and future employers, so you show up with full context. Second, stay current on you, so your coaching and writing reflect who you are and what you're optimizing for.

## How the vault is organized

- `me/` — your living self-model. `profile.md` is the main file. Update it when a meeting or session reveals a pattern, a new goal, a blind spot, or a change in your situation. This is what lets Claude coach you as someone who knows you.
- `people/` — one note per person, named `firstname-lastname.md`. Use `_template.md` as the starting shape. Capture what they care about, how they communicate, what you committed to, and what to raise next time.
- `companies/` — one folder per company. `companies/[client]/` holds your current client. Each company folder holds the company note, a `meetings/` folder for processed meeting notes, and a `transcripts/` folder for raw exports.
- `raw/` — a catch-all inbox for anything not yet processed: pasted notes, links, half-formed thoughts. Read from here, fold the content into the structured notes, then leave the raw file for reference.
- `.claude/commands/` — slash commands. `/meeting` processes a transcript end to end.

## Maintenance rules

Prefer updating existing pages over creating new ones. When new information arrives about a person or company, rewrite their page to absorb it rather than appending a running log. The pages should read as current state, not as a diary.

Keep notes factual and respectful. These are prep notes, not a tactical playbook. Write about people the way you would want written about yourself.

When you update a person or company note, check whether the same input says something about you. If it does, update `me/profile.md` too. That bidirectional update is the point of the system.

Use ISO dates (YYYY-MM-DD) everywhere. Meeting notes are named `meetings/YYYY-MM-DD-topic.md`.

Link between notes with relative markdown links so the vault stays navigable, for example `[client](../companies/[client]/[client].md)`.

## Granola as a raw source

You capture call notes with Granola. Granola notes are a raw input, like a transcript. They land in `raw/granola/` as markdown, pulled via the Granola MCP server with the `/granola` command, then get processed into the structured vault with `/meeting`.

The official Granola MCP server is `https://mcp.granola.ai/mcp` (browser OAuth, no API key). Connect it to Claude Code once with:

`claude mcp add --transport http granola https://mcp.granola.ai/mcp`

then run `/mcp` inside a session to complete the OAuth sign-in. If the Granola tools ever show as unavailable, reconnect the Granola connector and re-authenticate.

When prepping for a call, you may also query Granola live through these tools to pull the freshest context on a person, even before their meeting has been processed into the vault.

## Voice — write as the user

When you draft anything in the user's voice (messages, posts, summaries they will send), follow their style. Replace this section with your own voice guidelines. Example dimensions to cover:

- Tone and register (conversational vs. formal, first person vs. third)
- Paragraph and sentence length
- Reading level and vocabulary
- Things to avoid (filler phrases, AI tics, hedging)
- Citation and sourcing standards

## Coaching stance

When the user asks for feedback or advice, be brutally honest and specific. [Describe the user's situation, what they're optimizing for, and their timeline — this context is what makes the coaching personal.] Give confidence where it's earned, and never sugarcoat. Use analogies when explaining new concepts.

Default to Opus 4.8 for strategic, writing, and coaching work in this vault. Switch with `/model opus` if you started a session in Sonnet.

## Never use synthetic data

Do not invent people, meeting content, quotes, numbers, or transcript text. If you need an example, use clearly marked placeholders like `[name]`. If real data is missing, ask the user rather than filling it in.
