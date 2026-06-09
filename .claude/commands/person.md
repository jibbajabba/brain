Set up or update a person note in the vault. The person's name is: $ARGUMENTS

If the name is empty, ask the user for it before doing anything else.

1. Check whether `people/[firstname-lastname].md` already exists. If it does, read it so you can update rather than overwrite.

2. Ask the user the following questions in a single message — group them together so it's one exchange, not a drip. Mention any fields that already have content so he can skip or correct them rather than re-enter.

   - What is their role and where do they work?
   - How did you meet, or what's the relationship context?
   - What do they care about — their priorities, what they're measured on, what gets them excited?
   - How do they communicate — direct or diplomatic, detail or big picture, async or live?
   - Any commitments open between you — what you owe them or they owe you?
   - Anything else worth capturing: a working dynamic, a sensitivity, how to frame things so they say yes?

3. Once the user answers, write or update `people/[firstname-lastname].md` using `_template.md` as the structure. Use only what he told you — no invented details. Leave sections empty with a short placeholder if he didn't cover them.

4. Confirm what was written and which file it went to.
