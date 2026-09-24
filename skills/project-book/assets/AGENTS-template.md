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
- Check real interfaces before drafting (context7 first, then upstream).
- The user's code lives in `<path>`: read only.
