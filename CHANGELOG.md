# Changelog

## Unreleased — 2026-09-25

### Release metadata

- **`enchanter-{core,skills,web,orchestration}--v0.7.0` package tags** (WIX-INSTALL-002).
  Bumped from `v0.6.0` — four separate annotated tags that all peel to the same commit,
  `d0d0f9c4d1e82076bce12e74e81db33d0afedc5d` (dated 2026-05-11); `4b76f9a7...` is only the
  `core` tag's own object id, not a shared commit. This release adds no new content of its own
  beyond this metadata — the conduct content itself was already on this branch — but it is
  **not purely additive**: `hooks.md`'s Pattern 1 deny exit code changes from 1 to 2, and
  `conduct-abi-check.sh`'s default canonical path and no-dir behavior change. See
  `.changeset/enchanter-core-web-v0-7-0.md` for the full file count, the two behavioral changes
  flagged in detail, and the open decisions left for the vis owner (0.7.0 vs 1.0.0, annotated vs
  lightweight tags, whether/how this should bump `@enchanter-ai/vis-meta` at all).
- **`package.json` is intentionally NOT hand-bumped in this entry.** Per `.changeset/README.md`'s
  own documented flow, only `changeset version` (run against the auto-opened Versions PR) moves
  `package.json` and generates its changelog entry; a normal commit/PR contributes a changeset,
  not a version bump. Hand-bumping here in addition to the changeset would double-bump once CI's
  `changesets.yml` workflow runs `changeset version` on `main`.
- Cutting and pushing the actual `enchanter-<pkg>--v0.7.0` git tags is a release action for the
  vis owner; this repository's remediation clone records the changeset and this entry only, plus
  a `.vis-lock` regenerated against a disposable clone with those tags cut locally (see the
  companion Wixie-side change). Downstream `.vis-versions` pins that reference `~0.7.0` before
  the real tags exist will fail loud (`vis tag enchanter-<pkg>--v0.7.0 not found locally`) rather
  than silently resolving to something else — by design, not a defect in this changelog entry.

## Unreleased — 2026-05-12

### Fixed

- **vis-missing failure mode now fails loud.** Per s2.0 recommendation (lifecycle-research arc). Cloning a sibling plugin into a fresh directory without `vis` alongside used to leave `@-imports` silently missing (Claude Code's `@`-loader fails soft). Now:
  - `scripts/bootstrap.sh` auto-clones `vis` from `ENCHANTER_VIS_REPO` (default: github.com/enchanter-ai/vis) if not alongside.
  - `scripts/hooks/sessionstart-vis-drift.sh` fires on every Claude Code session start; exits non-zero with `"vis sibling missing — run ./scripts/bootstrap.sh"` if absent.
  - `.github/workflows/vis-verify.yml` runs `./scripts/bootstrap.sh --verify` on every push to main; CI fails on any drift or missing-vis state.
- **Drift detection.** `.vis-lock` pins `vis_commit` + per-package `tag_commit` + per-conduct-file SHA1. `bootstrap.sh --verify` fails loud with a verbatim one-line remediation on any divergence (corruption, vis HEAD move, missing file).
- **Verified before/after.** Sandbox-tested with a fresh clone of `crow` in isolation; before: silent fail; after: loud fail with named remediation. Sandbox report at the recommendation workspace.

### Added (Layer 1–3)

- `packages/orchestration/templates/bootstrap.sh` + `.ps1` (Bash + PowerShell) — resolve / verify modes.
- `packages/orchestration/templates/sessionstart-vis-drift.sh` — SessionStart hook template.
- `packages/orchestration/templates/vis-verify.yml` — GitHub Actions workflow template.
- `packages/orchestration/docs/vis-lock-spec.md` — lock-file schema specification.

### Documented (Layer 4 — not shipped)

- `docs/future/org-mirror.md` — Cargo source-replacement pattern-precedent + 3 trigger conditions. Built only when triggers fire.


