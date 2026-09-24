# Book state: how any agent can pick up where another stopped

Agents lose their memory. The user may switch tools (Claude Code, Codex,
Cursor, Gemini CLI, ...) between chapters, or even halfway through one. So
**everything needed to continue lives in the book's repository**, as plain
files, never in one agent's private memory, scratchpad, or chat history.

## The `.book/` folder

```
<book>/
├── AGENTS.md              # rules (read by most agents automatically)
└── .book/
    ├── state.md           # where the book stands: READ FIRST, UPDATE OFTEN
    ├── concepts.md        # what's been shown in full, and where
    ├── captures/          # real output, saved as it was captured
    │   └── ch05/
    │       ├── 01-curl-manifest-401.txt
    │       └── 07-typecheck-after-ctor.txt
    └── scratch/           # the scratch copy of the user's code (gitignored)
```

Add `.book/scratch/` to the book's `.gitignore`. Everything else in `.book/`
is committed with the book, so it moves between machines too.

## state.md

Use [../assets/state-template.md](../assets/state-template.md). It holds:

- **Book facts**: title, the user's code folder, the check command, how to
  build the book, the decisions made (with the reasons).
- **Progress**: one line per chapter, with its status:
  `planned` → `drafting` → `written (awaiting user)` → `done`.
- **Current work**: for the chapter in progress, which phase it's in, what
  step it's on, what's already captured, and exactly what's next. Written
  so that a stranger could continue.
- **The user's code vs the book's**: spelling differences, missing pieces,
  anything that would confuse a "check my code" pass.
- **Open questions** waiting for the user, and their answers once given.
- **Log**: dated one-liners, newest last. Include which agent or tool did
  the work.

## concepts.md

The first time/later time rule (code-in-chapters.md) needs to know what
has already been shown in full. Keep a table:

```markdown
| Concept | First shown in full | Later "Your turn" uses |
|---|---|---|
| `vi.fn<T>` mock | Ch4 Step 7 | Ch5 tokenGiving |
| error class of our own | Ch3 | Ch4 RegistryError, Ch5 AuthError |
```

Add rows when a chapter is written, not afterwards. When the ledger is
unsure, grep the earlier chapters before deciding.

## captures/

Every piece of output the chapter quotes is saved here as a file as soon as
it's captured: the command, the state it was run in, and the raw output.

```
$ npm run typecheck
# state: Ch5 Step 5, constructor changed, tests not yet updated
<raw output>
```

This means a new agent never has to re-run things to find out what the
chapter should say. It can also check the chapter against real output. If
a capture has to be redone, replace the file and note it in the log.

## Checkpoints: when to write state

Write `state.md` (and commit nothing; the user commits) at each of these:

1. **Start of a session**, after orienting: the log line "picked up by
   <agent>".
2. **After each phase of the chapter loop**: interfaces checked, code built
   in scratch, captures done, chapter drafted, book built.
3. **Before any long or risky step** (a large rewrite, running against a
   live service).
4. **Before stopping for any reason**: the user's turn, running out of
   context, an error you can't resolve. Say plainly what's next.
5. **When the user gives a rule or makes a decision**: record it here and
   in `AGENTS.md` (if it's a rule).

A good "Next" line is specific: "Write Step 6 prose from
captures/ch05/09-*.txt; the scratch code is final; Steps 1–5 are
drafted in src/05-*.md", not "continue chapter 5".

## Resuming, as a new agent

1. Read `AGENTS.md`, then `.book/state.md`, then `.book/concepts.md`.
2. Trust the files, not your assumptions. If `state.md` and the repo
   disagree (e.g. the chapter file has more steps than state says), believe
   the repo, fix `state.md`, and say so in the log.
3. If `.book/scratch/` is missing (new machine, or gitignored), rebuild
   it: copy the user's code, then apply the book's code up to the current
   step (verification.md).
4. Tell the user in one or two lines where things stand and what you'll
   do next, then carry on.

## Agent memory, if the agent has one

It's fine to also keep notes in an agent's own memory, but it's never the
source of truth. Anything that matters to the book must also be in
`.book/`. If the two disagree, `.book/` wins.
