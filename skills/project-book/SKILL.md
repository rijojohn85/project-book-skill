---
name: project-book
description: Write or continue a project-based technical book (mdBook) that teaches a language or system by building one real tool, chapter by chapter, with verified code and real captured output. Use when the user says "next chapter", "let's go with chapter N", "resume the book", "start a new book", "revise chapter N", or works inside a folder with book.toml plus src/NN-*.md chapters. Covers planning a new book, drafting a chapter, verifying its code in a scratch copy, and the per-chapter hand-off loop.
metadata:
  author: rijojohn85
  version: "1.1"
---

# Project book

The user learns by building one real tool, one chapter at a time. You write a
chapter, stop, the user types the code into their **own** project, then asks
for the next chapter. The book must be correct, because the user runs every line.

## Before anything: find where the book stands

1. Read the book's `AGENTS.md` (in the book root, next to `book.toml`). **It
   overrides this skill** on every rule it states (test style, audience, syntax
   rules). If there's no `AGENTS.md`, you're starting a book → see step table.
2. If the agent keeps memory (e.g. Claude Code auto-memory), read this
   book's entry for progress, the user's code location, and naming
   differences.
3. `git log --oneline` in the book repo, plus the list of files in `src/`. The
   last commit shows which chapter the user finished.
4. Read the end of the previous chapter: its "Next chapter" paragraph is a
   promise the new chapter must keep.
5. Read the user's real code (read only). The book continues from the book's
   code, but know where theirs is different, e.g. their own spelling of a name.

## What to load, and when

Load a reference file only when its condition is true.

| When | Read |
|---|---|
| Starting a brand-new book (no outline yet) | [references/starting-a-book.md](references/starting-a-book.md) |
| Drafting or revising any chapter's prose | [references/prose-style.md](references/prose-style.md) |
| Any chapter that contains code or tests | [references/code-in-chapters.md](references/code-in-chapters.md) |
| Designing a chapter's code, or naming a design principle in the prose | [references/design-principles.md](references/design-principles.md) |
| Before writing prose that quotes output, errors, test counts, or API behavior | [references/verification.md](references/verification.md) |
| Book is TypeScript / Node | [references/lang-typescript.md](references/lang-typescript.md) |
| Book is Go | [references/lang-go.md](references/lang-go.md) |
| Need a chapter skeleton or a new book's AGENTS.md | [assets/chapter-template.md](assets/chapter-template.md), [assets/AGENTS-template.md](assets/AGENTS-template.md) |

## Rules that always apply

These are short. The reference files explain the details.

- **Plain words, always.** Describe what a thing does before naming it. Name
  it once. No unexplained jargon, not in chat either.
- **One chapter at a time.** Write it, stop, and let the user try it. Never
  write two chapters in a row without being asked.
- **Never write in the user's own project folder** unless they say to. Verify
  the chapter's code in a scratch copy of it (see verification.md).
- **Real output only.** Every terminal block, error, and test count in the
  chapter was produced by actually running it. Never type it by hand.
- **Check real interfaces before drafting**: docs via context7 first, then
  upstream source or the live service (curl it).
- **First time a concept appears, show its code in full. After that: explain
  → "Your turn" → reveal.** Never skip the reveal.
- **Comment every code block** in plain words, so the reader never has to
  flip back to the prose to follow the code.
- **Book code is clean code worth copying**: DRY and SOLID are followed,
  and each is named in plain words at the line where it pays off, never
  lectured. Run design-principles.md's checklist before handing over.
- **Be honest about gaps.** Say what the chapter doesn't handle, and why.
- **Every chapter ends with** the check command passing, one git commit, a
  list of "what you should now be able to answer", and a one-paragraph
  preview of the next chapter.
- **Ask one question at a time** when a decision is the user's to make.

## Chapter loop

1. Orient (section above). Re-read the outline entry for this chapter.
2. Check the real interfaces: live endpoints, library docs, compiler flags.
3. Design the chapter's code (design-principles.md), then build it in the
   scratch copy. Get the check command green.
   Capture every output the chapter will show, **including the in-between
   states** (errors mid-refactor, failing tests shown on purpose).
4. Write the chapter file (`src/NN-slug.md`). Use the chapter template.
5. Re-check: counts, file:line numbers, and outputs match the exact state the
   reader will be in at that step. Rebuild that state if in doubt.
6. Build the book (`mdbook build`) to catch broken markdown.
7. Update the book's memory entry: which chapter is written, what's
   awaiting the user, any new decisions.
8. Reply briefly: what the chapter builds, and anything the user must know
   (e.g. their spelling is different from the book's). Then stop.

## When the user comes back

- "resume" / "it works": check their code compiles and tests pass (read
  only). If something's broken, point to the exact line and the fix in
  words. Don't edit their files.
- A new rule from the user ("always X", "remember this"): add it to the
  book's `AGENTS.md` **and** a feedback memory. If it's true for every book,
  also add it to this skill's references.
