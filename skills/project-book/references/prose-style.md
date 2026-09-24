# Prose style

## Plain language

- Describe what a thing *does* before giving its name, then name it once.
  "A pretend function that also records how it was called. That's a
  *mock*."
- Never use a term that hasn't been explained. If a term shows up with no
  explanation next to it, that's a bug in the book.
- One new term per idea. Don't pile several up in one paragraph.
- Short sentences. Concrete examples. Analogies only when they really help.
- Start from what the reader already knows (Docker, Go, Python), then peel
  back one layer at a time.
- **Repeat explanations.** When a concept comes back in a later chapter,
  give a one-line plain reminder where it's used ("`??` means 'or, if
  that's missing'"). Don't just point back to the earlier chapter. People
  need to hear things more than once.
- Compare to the reader's known languages where it helps ("Go writes
  `...Response`, Python writes `*args`").

## Chapter shape

1. Opening: what the last chapter left us with, and what this one adds.
   Say plainly what's new in this chapter.
2. Show the real thing by hand first (curl, shell commands) so the reader
   knows exactly what the code must do.
3. A short "The plan": the pieces, one job each.
4. Steps (`## Step N: ...`), each ending in a runnable, checkable state.
5. "Try it": real runs, success and failure cases, each one explained.
6. "What this chapter doesn't do": honest gaps, and where (or whether)
   they get fixed.
7. Commit block.
8. "What you should now be able to answer": questions, not statements.
9. "Next chapter": one paragraph. It's a promise, and the next chapter
   must keep it.

## Design principles

Name SOLID/DRY/etc. at the exact line where they pay off, in plain words:
"This is the open/closed principle, the 'O' in SOLID ... adding password
login later means writing a new class; this one doesn't change." Never a
lecture section.

## Honesty

- Say which parts are simplified, and what a real tool would do instead.
- When the real service behaves surprisingly (Docker Hub answers `401`,
  not `404`, for a missing repo), show it and explain why.
- If output depends on time (digests change, tags get rebuilt), say so.
- If a claim needs the user's own environment (a real cluster, root), leave
  a clearly marked placeholder, and fill it in once the user pastes real
  output. Never make up output to fill the gap.

## Terminal blocks

The reader copies commands from the book. Output must never look like a
command.

- Commands start with `$ ` (normal user) or `# ` (root). Nothing else
  starts with those.
- Output has **no** prefix.
- Prefer two blocks, the command then the output. A sentence between them
  ("you should see:") helps when it isn't obvious.
- No bare `#` comment lines inside terminal blocks. Put explanations in
  the prose.
- Headers or long output may be trimmed. Say so in the prose ("headers
  trimmed", "cut short").

(Why: a reader once pasted an output line into bash, because the command
and output were formatted the same.)
