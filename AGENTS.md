# AGENTS.md — plugin-addr

Standalone plugin repo for the `addr` capability (`verb:addr`). The plugin is a
Go module at `candy/plugin-addr/` (module path
`github.com/opencharly/plugin-addr/candy/plugin-addr`); the root `charly.yml`
only declares `discover: candy` so the repo is a project and its candy is
scanned.

Canonical files:

- `candy/plugin-addr/charly.yml` — the `plugin-addr:` candy entity (`plugin:`
  block, `plan:` check).
- `candy/plugin-addr/plugin.go` — the `verb` implementation (`RunVerb` via
  `sdk/kit.CheckContext`), `NewCheckVerb()` + `NewMeta()`.
- `candy/plugin-addr/schema/addr.cue` — the self-contained `#AddrInput`.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-internals:plugin` — the plugin authoring reference: the `plugin:`
  block, the unified Provider model, the per-plugin CUE-schema contract,
  placement. Load before touching the provider or schema.
- `/charly-check:check` — the declarative check-verb surface the `addr:` verb is
  authored through.
- `/charly-internals:git-workflow` — before any git/PR action.

## Build / validate / test

- `go build ./...` in `candy/plugin-addr/` — compile the plugin module.
- `go test ./...` in `candy/plugin-addr/` — the plugin's Go tests.
- `charly box validate` at the repo root — the structural check (the candy +
  `plugin:` block, CUE schema).
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate.
- There is no live bed: the verb is host-coupled and compiled-in, so its evidence
  is its `plan:` `check:` step plus the Go tests.

## Modify this repo

- Edit the `plugin-addr:` candy entity, the Go source, and `schema/addr.cue`
  **together** — the schema is the single source for the `params/` struct, so a
  field change not mirrored in the schema desyncs the generated types.
- This plugin is **compiled-in only** (a `kit.CheckVerbProvider` on the live
  check engine); do not describe it as out-of-process.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
