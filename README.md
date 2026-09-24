# project-book: an Agent Skill for writing project-based technical books

An [Agent Skill](https://agentskills.io) that helps a coding agent (Claude Code
or any agent that supports the format) write a **project-based technical
book**: one that teaches a language or system by building one real tool,
chapter by chapter.

It encodes a workflow proven on three books (a Kubernetes CSI driver in Go, a
mini container runtime in Go, and a container image puller in TypeScript):

- **One chapter at a time.** The agent writes a chapter and stops. You type the
  code into your own project, then ask for the next one.
- **Every line is real.** The agent builds the chapter's code in a scratch
  copy of your project, gets the tests green, and uses only output it
  captured itself: test counts, compiler errors, stack traces, live `curl`
  replies. Nothing is typed by hand.
- **Your code stays yours.** The agent reads your project, but never writes to
  it.
- **Plain words.** Each thing is described by what it does before it's named.
  No unexplained jargon.
- **Learn by doing.** The first time a concept appears, the code is shown in
  full. After that, the agent explains what's needed, gives a
  **"Your turn"** task, then reveals the answer.
- **Clean code, DRY, SOLID.** The book's code follows them, and each one is
  named in plain words at the line where it pays off, never lectured.
- **Honest gaps.** Each chapter says what it doesn't handle, and why.

## How it loads (progressive disclosure)

Only the name and description are loaded at startup. `SKILL.md` (under 100
lines) loads when the skill is triggered. Each reference file loads only when
its condition is true:

```
skills/project-book/
├── SKILL.md                       workflow, always-on rules, the "what to load when" table
├── references/
│   ├── starting-a-book.md         new book only: choosing the project, decisions, outline, layout
│   ├── prose-style.md             writing chapter prose; terminal-block rules
│   ├── code-in-chapters.md        chapters with code: first-time vs "Your turn", comments, edits
│   ├── design-principles.md       designing code: clean code, DRY, SOLID (+ checklist)
│   ├── verification.md            scratch copy, capturing real output, rebuilding in-between states
│   ├── lang-typescript.md         TypeScript/Node books only
│   └── lang-go.md                 Go books only
└── assets/
    ├── chapter-template.md        chapter skeleton
    └── AGENTS-template.md         per-book rules file (overrides the skill)
```

## Install

### Claude Code: for all your projects

```bash
git clone https://github.com/rijojohn85/project-book-skill.git
mkdir -p ~/.claude/skills
cp -r project-book-skill/skills/project-book ~/.claude/skills/
```

Or **symlink** it, so that a `git pull` updates the skill:

```bash
git clone https://github.com/rijojohn85/project-book-skill.git ~/src/project-book-skill
mkdir -p ~/.claude/skills
ln -s ~/src/project-book-skill/skills/project-book ~/.claude/skills/project-book
```

### Claude Code: for one book project only

From the root of your book's repository:

```bash
mkdir -p .claude/skills
cp -r /path/to/project-book-skill/skills/project-book .claude/skills/
```

Commit it, and anyone working on the book gets the skill.

### Other agents

Copy `skills/project-book/` into the folder where your agent looks for skills.
The format follows the [Agent Skills specification](https://agentskills.io/specification).

### Check it's installed

Start a **new** Claude Code session (skills are found at startup) and type
`/project-book`. It should autocomplete. You can also validate the folder:

```bash
claude plugin validate ./skills
```

## Use

Trigger it by what you say, or call it directly with `/project-book`:

- "Let's start a new book about building X in Rust." The agent proposes
  projects in plain words, then asks for decisions **one at a time**
  (runtime, test framework, dependencies, scope, testing style, audience)
  and writes the outline, `book.toml`, and the book's `AGENTS.md`.
- "Next chapter" / "let's go with chapter 5." The agent reads the
  book's `AGENTS.md` and the end of the previous chapter, checks the real
  docs and services, verifies the code in a scratch copy, writes
  `src/NN-slug.md`, builds the book, and stops.
- "Resume" / "it works." The agent checks that your code compiles and its
  tests pass, and points to anything that's off. It doesn't edit your files.

### Book layout it expects

```
my-book/
├── AGENTS.md        # this book's rules; they override the skill
├── book.toml        # mdBook
├── src/
│   ├── SUMMARY.md
│   ├── 00-outline.md
│   └── 01-....md
└── <tool>/          # YOUR code: the agent only reads it
```

Put anything specific to your book in the book's `AGENTS.md`, such as the
audience, test style, or syntax rules. It always wins over the skill's
defaults.

## Requirements

- [mdBook](https://rust-lang.github.io/mdBook/) to build the book.
- Whatever toolchain the book teaches (Node, Go, ...).
- Optional: a documentation lookup tool such as
  [Context7](https://context7.com), which the skill uses first when
  checking library docs.
