# Book writing rules: <tool name>

## Audience
- Knows: <languages/tools the reader already has>
- New to: <what the book teaches>

## Style
- Plain words, always. Describe what a thing does before naming it; name
  it once. No unexplained jargon, ever.
- One working piece of the tool per chapter. No theory-only chapters
  (except an optional short primer).
- Clean code, DRY, SOLID: followed in all book code, and named at the exact line where they apply, in plain
  words. Never a lecture.
- Testing: <strict TDD, one test at a time | a few tests per chapter where
  they earn it>. Faking (network, clock, disk) is the main testing lesson.
- **First appearance of a concept: show the code in full.**
  **Every later appearance: explain what is needed, then a "Your turn"
  block, then reveal the code.** Never skip the reveal.
- Re-explain a concept briefly every time it comes back.
- Every code block carries plain-word inline comments.
- Real captured output only. Never hand-typed terminal output.
- Every chapter ends with: `<check command>` green, one git commit, "what
  you should now be able to answer", one-paragraph preview of the next.
- Terminal blocks: commands start with `$ ` (or `# ` as root). Output has no
  prefix. When in doubt, split into two blocks.
- Later chapters show edits, not full reprints. Say which file and where.
- <language-specific rules>

## Process
- One chapter at a time. Write it, stop, the user tries it, then the next.
- Check real interfaces before drafting (a docs lookup tool if the agent has
  one, then official docs, upstream source, the live service).

## State (any agent, any tool)
- Progress, decisions, and the next action live in `.book/state.md`. Read it
  first; update it at every checkpoint and before stopping.
- What's been shown in full vs "Your turn": `.book/concepts.md`.
- Real captured output: `.book/captures/chNN/`. Scratch copy of the user's
  code: `.book/scratch/` (gitignored).
- Workflow: the project-book skill
  (https://github.com/rijojohn85/project-book-skill). If your agent can't
  load skills, read `skills/project-book/SKILL.md` from that repo, or from
  `.agents/skills/project-book/` if it's copied into this book.
- The user's code lives in `<path>`: read only.
