Process a meeting into the vault. The input is: $ARGUMENTS

Do the following with Opus-level care.

**Determining the source:**
- If the input looks like a file path (starts with `/`, `./`, `~/`, or ends with `.md`, `.vtt`, `.txt`), read that file. For `.vtt` files, ignore timestamps and cue numbers and read only the speaker dialogue.
- If the input is a description (e.g. "Met with Naresh today") or is empty, do NOT invent content. Instead, tell the user you're ready to take his notes and ask him to share what he remembers: who attended, what was discussed, any decisions, and any action items. Wait for his reply, then proceed using what he provides as the source. If anything important is still unclear after his notes, add it to Open questions.

1. From the source (file or the user's inline notes), extract the meeting content.

2. **Determine the nature and company.**

   **Personal / peer calls** — if the meeting has no company affiliation (a peer, a friend, an inbound connection, a personal conversation), route it to `companies/personal/`. Create that folder structure if it doesn't exist (`companies/personal/meetings/`, `companies/personal/transcripts/`, `companies/personal/personal.md` stub). Do not apply job-seeking or client-relationship framing to these notes. Capture them as human conversations: what was shared, what resonated, what the person cares about. See the Personal call framework below.

   **Work / client calls** — determine the company from the transcript or filename. If the `companies/[company]/` folder doesn't exist, create it along with `companies/[company]/meetings/` and `companies/[company]/transcripts/` subfolders and a stub company note at `companies/[company]/[company].md`.

   Write a clean meeting note to the appropriate `meetings/` folder. Name it `YYYY-MM-DD-topic.md` using the date and topic from the transcript or filename. If neither gives a date (e.g. the source is the user's inline notes, or says "today"/"yesterday"), run `date '+%Y-%m-%d (%A)'` in the shell and resolve from that — never infer the date from session context or memory.

   Before writing, check whether a prep file exists for the same date in that folder — look for any file matching `YYYY-MM-DD-*-prep.md` where the date matches. If one exists, read it and include its content as the first section of the meeting note under the heading `## Pre-meeting prep`, then continue with the normal structure below it. This merges the forward-looking context with the actual meeting record in one file.

   **Work / client meeting note structure:**
   - Attendees
   - A one-paragraph summary of what the meeting was about
   - Pre-meeting prep *(if a prep file exists for this date)*
   - Decisions made
   - Action items as a checklist, with owners
   - Open questions
   - Follow-ups and prep for next time

   **Personal / peer meeting note structure:**
   - Attendees
   - A one-paragraph summary written in a human, not transactional, register — what the conversation was about, what connected you, what emerged
   - What I took away — insights, ideas, things that shifted or crystallized
   - Action items (keep-in-touch notes, anything committed to)
   - Open questions

3. **Update the relevant company or personal note** with any new people, open threads, or context.

4. **For each attendee who isn't the user, create or update their note in `people/`.**

   For **work contacts**, use `_template.md` as the base. Capture how they communicate, what they care about, and open threads.

   For **personal / peer contacts**, use a lighter structure — no "Working with them well" section, no instrumental framing. Sections:
   - Who they are (brief — role, location, how you're connected)
   - What they care about (genuine, not strategic)
   - How they communicate (only if distinctive or useful to remember)
   - Connection (what brought you together, what the relationship is)
   - Open threads / commitments
   - History

   In both cases: capture only what the transcript actually shows. Do not infer beyond the evidence.

5. If the transcript reveals something about the user — a pattern, a strength, a recurring trap, a new goal, something that shifted — update `me/profile.md`.

6. Never invent content. If something important is unclear, list it under Open questions and ask the user.

When you're done, report back with a short summary of what you wrote and what you updated. Match the tone of the summary to the meeting — keep it human for personal calls, not clinical.

Then ask: "Delete the source transcript?" If the user says yes, delete the source file. If the input was inline notes (not a file), skip this step.

---

## Personal call framework

Personal calls are collegial, not transactional. The goal of the notes is to remember the person and the conversation, not to optimize future interactions. Write as if you're telling a friend what the conversation was like.

Things worth capturing:
- What connected you in the first place
- What they care about (not instrumentally — genuinely)
- What surprised you or stayed with you
- Anything they shared about their own life or work that matters
- Any real follow-through you owe them (not "next steps" — just what you said you'd do)

Things to leave out:
- Job-seeking or interview framing unless the conversation was explicitly about that
- Tactical notes on how to "land" with them
- Anything that would read strangely if the person saw the note
