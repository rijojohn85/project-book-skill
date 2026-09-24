# Book state: <title>

Any agent picking this book up: read `AGENTS.md`, then this file, then
`concepts.md`. Update this file at every checkpoint (see the project-book
skill, references/book-state.md). If this file and the repo disagree, the
repo wins: fix this file.

## Book facts

- Title: <title>
- Teaches: <language / system>, by building <tool>
- User's code: `<path>` (read only; the user writes it)
- Check command: `<npm run check | make check>`
- Build the book: `mdbook build`
- Decisions (asked one at a time, confirmed by the user):
  - <decision>: <choice>. Why: <reason>

## Progress

| Ch | Title | Status |
|---|---|---|
| 1 | <title> | done |
| 2 | <title> | written (awaiting user) |
| 3 | <title> | planned |

## Current work

- Chapter: <N, title>
- Phase: <orient | interfaces checked | scratch built | captured | drafting | book built | awaiting user>
- Done so far: <which steps are verified or drafted>
- Captures: `.book/captures/chNN/` (<count> files)
- Scratch: `.book/scratch/<tool>` matches book code through <step>
- Promise from the previous chapter's "Next chapter": <what it said>
- **Next:** <one specific action>

## User's code vs the book's

- <e.g. user spells `checkAPI`, book spells `checkApi`: fine, just different>

## Open questions

- <question>: <answer, once given>

## Log

- <YYYY-MM-DD> <agent/tool>: <what happened>
