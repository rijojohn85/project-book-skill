# TypeScript / Node books

- **No semicolons at line ends.** A statement starting with `[`, `(` or a
  backtick needs a leading `;`. Check book code and the user's code for
  this. To remove semicolons from the user's files (only when asked), strip
  the trailing `;` only. Don't run Prettier, because it rewraps lines.
- **An explicit return type on every function**, including small test
  helpers. Long inline return shapes get a named interface (e.g.
  `FakeHttp`).
- Strict compiler settings (from `tsconfig.json`): `strict`,
  `noUncheckedIndexedAccess`, `exactOptionalPropertyTypes`,
  `noUnusedLocals/Parameters`, `verbatimModuleSyntax`,
  `erasableSyntaxOnly` (so no enums and no constructor parameter
  properties; fields are declared and assigned by hand).
- Imports use `.ts` extensions, and `import type` / inline `type` for
  names that are only types.
- Run with `node src/cli.ts` (Node strips types). Check with
  `npm run check` (= `tsc --noEmit` + `vitest run`).
- Tests: Vitest. `vi.fn<T>()` with the type argument, always. Async
  failures use `await expect(p).rejects.toThrow(...)`.
- Server data arrives as `unknown`, never `any`. It gets checked by hand
  until zod is introduced, then by a zod schema.
- Compare to Go and Python at the point of use: Go's `[]byte` and
  Python's `bytes` for `Uint8Array`, `*args` for rest parameters.
