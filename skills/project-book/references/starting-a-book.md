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
answer. Record every answer in the book's memory entry and `AGENTS.md`.

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
└── <tool>/            # the USER's own code: they write it, we only read it
```

Chapter files are `NN-slug.md`. A half-step `02.5` is used for a short
primer chapter.

## The outline (00-outline.md)

- "How this book works": the project, who it's for, and what gets
  tested and how. Say up front that later chapters show edits, not
  full reprints, and name the check command.
- "What the finished tool does": a terminal block of the end result.
- Table of contents: one line per chapter, saying what working piece it
  adds. Put the design principle in italics where it will come up.
- Dependencies, and why each one (and why everything else is built by
  hand).

## Memory

Create `book-<slug>.md` (type project) holding: location, the user's code
folder, the decisions above with **Why/How to apply**, and a progress line.
Link the feedback memories from it.
