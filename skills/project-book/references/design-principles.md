# Clean code, DRY, and SOLID

This comes in two parts. The book's code must **follow** these principles,
and the book must **teach** them. Following them comes first: the reader
copies our code, so it has to be code worth copying.

## How to teach them

- Name a principle **at the exact line where it pays off**, in plain words,
  and say what it bought us right there. Never write a lecture section or a
  list of principles up front.
- Describe what it does first, then give the name: "the code that talks
  to the registry doesn't make its own HTTP client, it's handed one ...
  That's the dependency inversion principle, the 'D' in SOLID."
- Show the cost of *not* following it when that's cheap to show: two
  copies of a helper that would drift apart, or a test that needs the
  internet.
- When a principle comes back, remind the reader in one line and use it.
  The explain → "Your turn" → reveal rule applies (code-in-chapters.md).
- Be honest about its limits. Open/closed protects a method's logic, not
  earlier tests that assert an exact count (CSI Ch10). Say so when it
  happens.
- Don't force one in. If no line in this chapter shows it, skip it.

## Clean code: rules for every code block

- **Names say what a thing is or does**: `parseChallenge`, `authorizedGet`,
  `RawManifest`. No `data`, `tmp`, `handle`, `mgr`. Test names read as
  sentences: `"reuses the token on the next request"`.
- **Small functions, one job.** If you need "and" to describe a function,
  split it (`describeApiCheck` vs `describeManifest`).
- **One level of detail per function.** `fetchManifest` says "GET with
  permission"; the token details live in `authorizedGet`.
- **Return early** on errors and special cases, and keep the main path
  unindented.
- **No magic values.** Name them: `DOCKER_HUB_API_HOST`,
  `MANIFEST_TYPES`, `DEFAULT_TAG`.
- **Errors are values with meaning.** Use the project's own error types
  with a clear message (`RegistryError(url, status, problem)`). Catch only
  what you understand; re-throw the rest, and never hide an error.
- **Don't change what you were given.** Copy it instead
  (`{ ...headers, authorization }`).
- **Make illegal states impossible** where the language allows it: tagged
  one-of types (`{ kind: "open" } | { kind: "needs-token"; ... }`),
  `readonly`, and a `switch` with no `default` so a new case won't compile
  until it's handled.
- **Comments explain why and syntax, not what the name already says.**
  (The book's teaching comments are the one exception: they also explain
  new syntax to the reader. See code-in-chapters.md.)
- **Order a file top-down**: small pieces first, then what's built from
  them. Tell the reader the order.
- **Tests are clean code too**: arrange / act / assert separated by blank
  lines, shared helpers for repeated setup, and checking only what the test
  is about (`objectContaining`).

## DRY: don't repeat yourself

What it means: every piece of *knowledge* lives in one place, so a change
happens in one place.

- Where it shows up in a book: the second time something is needed. The
  first copy is fine. The second copy is the moment to extract it and name
  DRY (`replyingWith` → `fake-http.ts`, three lines → `challengeOf`).
- Before extracting, check it's the *same knowledge* and not two things that
  merely look alike. Two things that look alike but change for different
  reasons should stay separate. Say so when that's the case.
- The extraction is a small step of its own: the behavior is unchanged, and
  the tests are still green. Show that.

## SOLID, in plain words

| Letter | Plain words | What it looks like in the books |
|---|---|---|
| **S**: single responsibility | One piece, one reason to change. | `parseReference` knows names, not networks. The content store only stores. |
| **O**: open/closed | Add new behavior by adding code, not by rewriting tested code. | `Authenticator` interface: password login = a new class; `RegistryClient` untouched. |
| **L**: Liskov substitution | Anything that claims to be an X must behave like an X, so callers can't tell which one they got. | A fake `HttpClient` returns a real `Response`; the fake behaves like `fetch` (e.g. a body is read only once). |
| **I**: interface segregation | Ask only for what you use. Small interfaces. | `HttpClient` has one method, `get`, not five. |
| **D**: dependency inversion | Depend on a description (an interface) and be handed the real thing from outside. | `RegistryClient(http, registry, auth)`; only `main` picks the real pieces. |

Rules:

- The composition root (`main`, later one wiring file) is the **only**
  place that creates real network, disk, or clock pieces. Say so each time
  a new one is wired in.
- Grow interfaces only when a real caller needs it. When you grow one, prefer
  changes that don't break anything (an optional parameter), and show that
  the old tests still pass.
- Don't add an interface "for later" with one implementation and no test
  that uses a fake. It has to earn its place in this chapter.

## Checklist before handing over a chapter

- [ ] Every new function has one job and a name that says it.
- [ ] No knowledge is written twice (a second copy was extracted, or
      there's an explained reason to keep it).
- [ ] Network, disk, and clock are reached only through something handed
      in from outside.
- [ ] Any principle this chapter's code shows is named at its line, in
      plain words, once.
- [ ] Nothing earlier was rewritten when adding code would have done.
