# Code in chapters

## First time vs later times

Keep track of which concepts and patterns the book has already shown
(grep earlier chapters when unsure).

- **First appearance** of a concept (a syntax feature, a library call, a
  pattern like "fake the network"): show the code in full, then explain it.
- **Every later appearance**: explain in plain words what's needed →
  "Your turn" block → reveal the code. Never skip the reveal.

Format:

~~~markdown
> **Your turn.** In `src/auth.ts`, export an interface `Challenge` with
> two `readonly` string fields, `realm` and `service`.

Here it is:

```ts
...
```
~~~

A "Your turn" task must say enough to be done without guessing: the file,
the name, the signature, what counts as right, and any new API needed
(e.g. "`new Response` takes headers as a plain object: ...").

If a step mixes new and repeated ideas, show the new parts, and make the
repeated parts a "Your turn".

## Comments in code blocks

Every code block carries plain-word comments on non-obvious lines,
explaining the syntax and the intent. The code must be readable without
the prose. This applies to "Your turn" reveals too. Re-explain repeated
syntax in comments as well. Prose still explains the *why*.

## Edits, not reprints

- After a file exists, show edits: say which file and where ("add below
  `checkApi`", "replace the `401` branch", "the top of the file").
- Reprint a whole file only when it's small or mostly changed.
- After several edits to one file, give a sentence listing its order top
  to bottom, so the reader can check theirs.
- Use `// ...` to mark code that was left out.

## Tests

Follow the book's `AGENTS.md` for method (TDD or not). Always:

- Faking the network, the clock, and the disk is the main testing lesson.
  Code takes what it depends on as a parameter (dependency inversion) so a
  test can hand it a fake.
- Show a test failing on purpose when the failure teaches something
  (mock diff output, a forgotten `await`, running out of fake replies).
  Capture that failure for real, then tell the reader to undo the change.
- Use the real data from the chapter's by-hand section as test data (real
  headers, real error text).
- When a second test file needs a helper, move it to a shared file, and
  name DRY at that moment.

## Changes that break earlier code

When a change breaks callers (a new required parameter), run the checker,
show its real error list, and use it as a to-do list. When a change is
designed *not* to break anything (an optional parameter), run the checks
and show them still passing. That is the lesson.
