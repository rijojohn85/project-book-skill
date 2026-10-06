---
name: project-book
description: Write or continue a project-based technical book (mdBook) that teaches a language or system by building one real tool, chapter by chapter, with verified code and real captured output. Use when the user says "next chapter", "let's go with chapter N", "resume the book", "start a new book", "revise chapter N", or works inside a folder with book.toml plus src/NN-*.md chapters or a .book/state.md file. Covers planning a new book, drafting a chapter, verifying its code in a scratch copy, and resuming work another agent started.
metadata:
  author: rijojohn85
  version: "2.0"
---

# Project book

The user learns by building one real tool, one chapter at a time. You write a
chapter, stop, the user types the code into their **own** project, then asks
for the next chapter. The book must be correct, because the user runs every line.

The skill works with any agent. It uses only reading and writing files,
running shell commands, and (if available) a way to look up docs and fetch
web pages. **All progress lives in the book's repository**, in `.book/`, so
the user can switch agents between chapters, or halfway through one.

## Before anything: find where the book stands

1. Read the book's `AGENTS.md` (in the book root, next to `book.toml`). **It
   overrides this skill** on every rule it states. If there's no
   `AGENTS.md` and no `src/`, you're starting a book → see the table below.
2. Read `.book/state.md`, then `.book/concepts.md`. They say what's done,
   what's in progress, and the exact next action. **If they're missing** on
   an existing book, create them now from the repo (chapters, git log, the
   user's code) before doing anything else. See references/book-state.md.
3. Check state against the repo: `git log --oneline`, the files in `src/`,
   the current chapter file. If they disagree, the repo wins. Fix
   `state.md` and log it.
4. Read the end of the previous chapter: its "Next chapter" paragraph is a
   promise the new chapter must keep.
5. Read the user's real code (read only). Note where it's different from the
   book's (for example, their own spelling of a name) in `state.md`.
6. Add a log line to `state.md` ("picked up by <agent>"), and tell the user in
   one or two lines where things stand and what's next.

## What to load, and when

Load a reference file only when its condition is true.

| When | Read |
|---|---|
| Resuming, handing over, or `.book/` is missing or unclear | [references/book-state.md](references/book-state.md) |
| Starting a brand-new book (no outline yet) | [references/starting-a-book.md](references/starting-a-book.md) |
| Drafting or revising any chapter's prose | [references/prose-style.md](references/prose-style.md) |
| Any chapter that contains code or tests | [references/code-in-chapters.md](references/code-in-chapters.md) |
| Designing a chapter's code, or naming a design principle in the prose | [references/design-principles.md](references/design-principles.md) |
| Before writing prose that quotes output, errors, test counts, or API behavior | [references/verification.md](references/verification.md) |
| Book is TypeScript / Node | [references/lang-typescript.md](references/lang-typescript.md) |
| Book is Go | [references/lang-go.md](references/lang-go.md) |
| Need a template | [assets/chapter-template.md](assets/chapter-template.md), [assets/AGENTS-template.md](assets/AGENTS-template.md), [assets/state-template.md](assets/state-template.md) |

## Rules that always apply

These are short. The reference files explain the details.

- **State lives in the repo, not in you.** Update `.book/state.md` at every
  checkpoint: after each phase, before anything risky, and before you stop
  for any reason. Save real output to `.book/captures/` as soon as you
  capture it.
- **Plain words, always.** Describe what a thing does before naming it. Name
  it once. No unexplained jargon, not in chat either.
- **One chapter at a time.** Write it, stop, and let the user try it. Never
  write two chapters in a row without being asked.
- **Never write in the user's own project folder** unless they say to. Verify
  in the scratch copy `.book/scratch/` (see verification.md).
- **Real output only.** Every terminal block, error, and test count in the
  chapter was produced by actually running it. Never type it by hand.
- **Check real interfaces before drafting**: use a docs lookup tool if the
  agent has one, otherwise the official docs, upstream source, or the live
  service (curl it).
- **First time a concept appears, show its code in full. After that: explain
  → "Your turn" → reveal.** Never skip the reveal. `.book/concepts.md`
  records which is which.
- **Comment every code block** in plain words, so the reader never has to
  flip back to the prose to follow the code.
- **Book code is clean code worth copying**: DRY and SOLID are followed,
  and each is named in plain words at the line where it pays off, never
  lectured. Run design-principles.md's checklist before handing over.
- **Be honest about gaps.** Say what the chapter doesn't handle, and why.
- **Keep the "why" visible.** Every chapter shows a "Where we are" map,
  opens on a real gap, says why it matters, and ends on the next gap. Plan
  a rough end-to-end version early (a walking skeleton). See prose-style.md.
- **Write it whole, then split it.** Reuse existing functions; write new
  logic inline first. Once it works, split out helpers only when the
  split teaches something (SOLID, testability, a second use), with tests
  passing after each step. See prose-style.md.
- **Every chapter ends with** the check command passing, one git commit, a
  list of "what you should now be able to answer", and a one-paragraph
  preview of the next chapter.
- **Ask one question at a time** when a decision is the user's to make.
  Record the answer in `state.md`.

## Chapter loop

Each step ends with a checkpoint: update `.book/state.md` (phase, what's done,
the exact next action).

1. Orient (section above). Re-read the outline entry for this chapter. Set
   the chapter to `drafting`.
2. Check the real interfaces: live endpoints, library docs, compiler flags.
   Save the curl output to `.book/captures/chNN/`.
3. Design the chapter's code (design-principles.md), then build it in
   `.book/scratch/`. Get the check command green. Capture every output the
   chapter will show, **including the in-between states** (errors
   mid-refactor, failing tests shown on purpose), one file per capture.
4. Write the chapter file (`src/NN-slug.md`) from the chapter template and
   the captures. For a long chapter, checkpoint after each step you draft.
5. Re-check: counts, file:line numbers, and outputs match the captures and
   the exact state the reader will be in at that step.
6. Build the book (`mdbook build`) to catch broken markdown.
7. Update `concepts.md` with this chapter's first-time concepts. Set the
   chapter to `written (awaiting user)`.
8. Reply briefly: what the chapter builds, and anything the user must know
   (e.g. their spelling is different from the book's). Then stop.

## When the user comes back

- "resume" / "it works": check their code compiles and tests pass (read
  only). If something's broken, point to the exact line and the fix in
  words. Don't edit their files. Once it works, mark the chapter `done`.
- A new rule from the user ("always X", "remember this"): add it to the
  book's `AGENTS.md` so every agent sees it. If it's true for every book,
  also suggest adding it to this skill.
