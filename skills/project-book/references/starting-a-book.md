# Starting a new book

## Pick the project

- Propose 2–4 projects **in plain words**: what the finished tool does, and
  what the reader will be able to show at the end. No jargon in the
  pitches (the user once asked "what is DAG?").
- The project should be something real, useful on its own, and ideally
  connected to the user's other books (CSI driver → runc → oci-pull all
  touch containers).
- The last chapter should prove it works against the real thing (e.g.
  hand the folder oci-pull made to real `runc`).

## Decisions: ask one at a time

Ask each as a separate question, give a recommendation, and wait for the
answer. Record every answer in `.book/state.md` (and rules in `AGENTS.md`).

1. Language version / runtime (e.g. Node LTS, Go 1.x).
2. Test framework (Vitest, testify, ...).
3. Allowed dependencies: keep to a minimum. Build by hand whatever
   teaches something; use a library only where writing it yourself teaches
   nothing (compression formats, schema checking).
4. Scope: where the tool stops (e.g. "pull only, no push").
5. Testing approach: strict TDD, one test at a time, red then green (runc
   book), or "a few tests per chapter where they earn it" (TS book).
6. Audience: what the reader already knows. Skip the basics they have.
7. How design principles appear. Default: named at the line where they
   apply, in plain words, never a lecture.
8. When the walking skeleton walks: the chapter where the tool first works
   end to end, roughly. Recommend about a third of the way in, with each
   shortcut listed and the later chapter that replaces it. (Default yes:
   a bottom-up book without one loses the "why"; see prose-style.md.)

## Layout

```
<book>/
├── AGENTS.md          # the book's rules, from assets/AGENTS-template.md
├── book.toml          # mdBook config; src = "src"
├── src/
│   ├── SUMMARY.md     # mdBook table of contents
│   ├── 00-outline.md  # how the book works, what the finished tool does, TOC
│   ├── 01-....md
│   └── 02.5-<lang>-primer.md   # optional short primer, the one theory chapter
├── .book/
│   ├── state.md       # progress + next action (assets/state-template.md)
│   ├── concepts.md    # what's been shown in full, and where
│   ├── captures/      # real output, one file per capture
│   └── scratch/       # scratch copy of the user's code (gitignored)
└── <tool>/            # the USER's own code: they write it, we only read it
```

Add `.book/scratch/` to `.gitignore`. If the user wants the skill to
travel with the book, copy it to `.agents/skills/project-book/` and
mention that path in `AGENTS.md`.

Chapter files are `NN-slug.md`. A half-step `02.5` is used for a short
primer chapter.

## The outline (00-outline.md)

- "How this book works": the project, who it's for, and what gets
  tested and how. Say up front that later chapters show edits, not
  full reprints, and name the check command.
- "What the finished tool does": a terminal block of the end result.
- "The stages": the finished tool's job split into about six stages, each
  with the chapter(s) that build it. Every chapter's "Where we are" map
  uses this exact list.
- The walking-skeleton chapter, and the shortcuts it takes.
- Table of contents: one line per chapter, saying what working piece it
  adds. Put the design principle in italics where it will come up.
- Dependencies, and why each one (and why everything else is built by
  hand).

## State

Create `.book/state.md` from assets/state-template.md with the book facts,
the decisions above (each with its reason), and a Progress table listing
every chapter in the outline as `planned`. Create an empty
`.book/concepts.md` ledger. See book-state.md.

If the agent has its own memory, a one-line pointer to the book is fine.
The state itself stays in `.book/`.
