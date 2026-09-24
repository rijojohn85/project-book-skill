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
