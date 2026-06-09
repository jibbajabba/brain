Pull recent Granola meeting notes into the vault inbox as markdown. Optional argument (a date, count, or search term): $ARGUMENTS

This uses the Granola MCP server. If the Granola tools aren't available, tell the user to connect them first (see the Granola section in CLAUDE.md) and stop.

1. List recent Granola meetings. If $ARGUMENTS gives a date, count, or search term, use it to scope the list. Otherwise default to meetings from the last 7 days.
2. For each meeting not already present in `raw/granola/`, fetch its AI notes or summary and, when available, the transcript.
3. Write each to `raw/granola/YYYY-MM-DD-title.md` (slugify the title for the filename). Include YAML frontmatter: `granola_id`, `title`, `date`, `attendees`, and `source: granola`. Put the summary under a `## Notes` heading and the transcript under `## Transcript`.
4. Use `granola_id` in the frontmatter to avoid duplicates. Skip anything already saved.
5. Do not process these into structured notes here. Report the list of files you wrote, then remind the user he can run `/meeting raw/granola/<file>` on any of them, or say "process all new Granola notes" to do them as a batch.

Never invent meeting content. Only write what the Granola tools actually return.
