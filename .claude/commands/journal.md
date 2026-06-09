Create or edit a journal entry in `~/Work/brain/journal/`.

Input: $ARGUMENTS

## Behavior

**If $ARGUMENTS is empty:**
Resolve today's date in YYYY-MM-DD format. Check whether `journal/YYYY-MM-DD.md` exists.
- If it doesn't exist: create it with a single header (`# YYYY-MM-DD`) followed by a blank line.
- If it exists: read and display the current content so the user can see what's already there.

Then ask: "What would you like to add?" — wait for his next message, then append it as a new paragraph. Preserve existing content exactly.

**If $ARGUMENTS looks like a date** (e.g. "yesterday", "June 1", "2026-05-30"):
Resolve it to YYYY-MM-DD. Open or create that file the same way, then invite writing.

**If $ARGUMENTS is a note** (anything clearly not a date):
Append it as a new paragraph to today's file, creating the file with a header if needed. Confirm. Done.

## File format

Filename: `journal/YYYY-MM-DD.md`

New file header:
```
# YYYY-MM-DD

```

Free-form prose after that. No required sections.

**Conventions — write these exactly as given, never reformat:**
- Lines starting with `→ ` or `-> ` are flagged reflections — the daily brief surfaces them
- Lines starting with `!! ` are urgent flags — something important being avoided; the brief names them directly and without softening

## Writing session rules

Take what the user writes and put it in the file as-is. Do not rewrite, polish, bullet-ify, or summarize their words. If they write rough notes, keep rough notes. If they write in prose, keep prose. The journal is theirs.

If they dictate something long in a conversational way, transcribe it faithfully — do not compress or editorialize.

## After writing

One line: confirm the filename and that it was saved. Do not echo the content back.
