# Go books

- Tests use `github.com/stretchr/testify`. Use `require` for setup that
  must work (fails and stops) and `assert` for the thing being checked
  (fails and continues). No hand-written `if err != nil { t.Fatalf }`
  blocks.
- Shared test helpers keep tests DRY (retrofitted to the CSI book,
  chapters 6–10).
- If the book is strict TDD (runc): **one test at a time**. Write one
  test, watch it fail (red), make it pass (green), then the next. Show
  each red for real.
- Tests or binaries that need root: the Makefile wraps them with
  `sudo $(GO)`, where `GO := $(shell which go)`, so root finds the user's
  Go. Always show `make test` / `make check` / `make run`, never raw
  `sudo go test`, which fails with "go: command not found".
- Kubernetes/CSI chapters: check upstream source (csi.proto, sidecar
  RBAC) before drafting. When the real dependency can't run locally,
  build a fake of its API (see verification.md). The user's own `kind`
  cluster is the final proof, so ask them to paste the output.

## Building a Go program in the chapter

- Build each program from the ground up: the smallest piece that runs
  first (for example, connect and print OK), then one idea per step, run
  after each step, real output captured for each.
- Keep the whole program in view. Before the first step, show a short map
  of the finished program's parts (for example: flags, client, send,
  result), each with one plain sentence. At every later step, show the map
  again, mark the part this step adds, and say what the parts still to
  come will do. If a step changes the map, say how.
- The reader must always know where the code they are typing sits in the
  whole program, and why.
