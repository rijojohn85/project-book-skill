# Verification: every line the user runs must be real

## Check real interfaces first

Before drafting, confirm how the things the chapter touches really behave:

- Library APIs: a docs lookup tool first if the agent has one (e.g.
  Context7), then official docs, upstream source, or the web. Say which
  answered.
- Live services: curl them and keep the exact output (status, headers,
  body shape). Try 2–3 providers (Docker Hub, ghcr, quay, mcr) to find
  where they differ.
- Specs and protocols: read the real spec or source (e.g. csi.proto),
  not memory.

## The scratch copy

Never write in the user's own project folder. Instead:

```bash
S=<book>/.book/scratch   # gitignored, survives agent switches
cp -r <book>/<tool> $S/<tool> && rm -rf $S/<tool>/.git
```

- Make the copy match **the book's** code, not the user's. Rename where
  the user's spelling is different (e.g. `checkAPI` → `checkApi`), and
  replace their files with the book's versions of the previous chapter
  where it matters for line numbers.
- Pull the previous chapter's code blocks straight out of its markdown
  when you need its exact files.
- Put the chapter's code in the copy **with its final comments**, because
  comments change line numbers in errors and stack traces.
- Run the book's check command (`npm run check`, `make check`) until green.

## Capture everything the chapter shows

Save each capture **the moment it's taken** to
`.book/captures/chNN/NN-<what>.txt`: the command, a `# state:` line saying
what the code looked like, then the raw output. Another agent can then
continue the chapter without re-running anything.

- Final test counts, and the count after each step that shows one.
- Errors shown mid-step: **rebuild that exact in-between state** (only the
  code the reader has typed so far) and run the checker there. Unused
  names, missing imports, and shifted line numbers all show up in real
  output. Include them, and explain them.
- Failures shown on purpose: make the change, capture, then revert.
- "Try it" runs against real services, success and failure cases.
- Trim output only when you say so in the prose. Never edit what's left.

If a result depends on something only the user has (a real cluster, root,
their hardware), write a marked placeholder and ask them to paste the real
output.

## Fakes of other people's code

If a dependency can't run locally (Kubernetes, gRPC servers), build a
small fake of its public API in the scratch copy, only to compile and
test the chapter. Don't teach the fake's insides on the page unless the
reader writes it too. Audit fakes against the real API: mismatches have
caused real bugs for readers before.

## Before handing over

- `mdbook build` succeeds.
- Every "Your turn" reveal matches the verified code.
- Every "N passed", `file(line,col)`, and error message matches what the
  scratch copy printed at that point.
- The previous chapter's "Next chapter" promise is kept.
