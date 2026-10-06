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
2. **Keep the "why" visible** (see below): the "Where we are" map, the
   gap shown for real, a why paragraph, and "By the end of this chapter
   you can:".
3. Show the real thing by hand first (curl, shell commands) so the reader
   knows exactly what the code must do.
4. A short "The plan": the pieces, one job each.
5. Steps (`## Step N: ...`), each ending in a runnable, checkable state.
6. "Try it": real runs, success and failure cases, each one explained.
7. "What this chapter doesn't do": honest gaps, and where (or whether)
   they get fixed.
8. Commit block.
9. "What you should now be able to answer": numbered questions, each
   with its answer hidden in a `<details><summary>Answer</summary>` block
   (blank lines around the answer so markdown renders inside it). Answers
   are plain words, 2-5 sentences, and say only what the chapter taught.
   Writing them also checks the chapter: a question the text can't answer
   means the text is missing something.
10. "Next chapter": one paragraph. It's a promise, and the next chapter
   must keep it. End on the next gap, shown for real where possible.

## Keeping the "why" visible

A project book is built bottom-up: each chapter makes one part, and the
parts only do something together. Readers lose track of why a part
matters ("with this build from bottom up approach I kinda lose why we're
doing it"). Every chapter must show where it fits:

- **"Where we are" map**, right after the opening: the stages of the
  finished tool's job (6 or so, fixed in the outline), each marked ✓
  done, ▶ this chapter, or · later (with the chapter that does it). The
  same list in every chapter, so the reader sees it fill up.
- **Open on the gap, shown for real**: run the tool as the last chapter
  left it and show, with real output, what's missing or broken. (The CSI
  book did this naturally: kubelet crash-looped until the next chapter
  fixed it.)
- **Why paragraph**: how this chapter gets us closer to the end goal, in
  two or three sentences. Then "By the end of this chapter you can:" with
  2-4 bullets of things the reader can *do*.
- **End on the next gap**: the last "Try it" or the "Next chapter"
  paragraph shows what still doesn't work.
- **Walking skeleton**: make the tool work end to end, roughly, as early
  as possible (about a third of the way in), with honest shortcuts.
  Later chapters each replace one shortcut, and say which. Plan this in
  the outline (starting-a-book.md).

Why these work: people learn details better after seeing an overview to
hang them on (Ausubel's "advance organizer" research); Crafting
Interpreters opens with a map chapter; *Distributed Services with Go*
opens each chapter with "how does this help us achieve the goal?"; GOOS
starts every project with a walking skeleton.

## Write it whole, then split it

"Make it work, then make it right." Fowler's *Refactoring* opens this
way: one long `statement()` function, then helpers pulled out one at a
time with the tests run after each. It keeps the "why" visible for
design: the reader sees the long, hard-to-test function (the gap)
before the split (the fix).

- If a function we need already exists, use it. Otherwise write the new
  logic inline in the function that needs it.
- Once it works, split out a helper only when the split teaches
  something: a SOLID idea (name it at that line), making code testable,
  or code needed a second time. DRY waits for the second use.
- Test the outer function's behavior before splitting. Those tests must
  pass unchanged after the split; that's the proof nothing broke. Add
  small helper tests afterwards. (Testing helpers first means every
  split breaks tests.)
- One helper per step; show the real check output after each.
- Cap the inline function at about 40 lines; longer is hard to explain
  in prose. Build it across a few sections, then split at the end.
- Not every chapter: the reader types the code twice.

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
