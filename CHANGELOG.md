# Changelog

## Unreleased — 2026-09-25

### Release metadata

- **`enchanter-{core,skills,web,orchestration}--v0.7.0` package tags** (WIX-INSTALL-002).
  Bumped from `v0.6.0` (all four packages were pinned to the same tag commit,
  `4b76f9a74454a47be6bea7beb8c426c29c106f9c`, dated 2026-05-11). This release
  contains no content changes of its own beyond this metadata; it exists to
  give the conduct content already on this branch (added since `v0.6.0`) a
  version identity that downstream `.vis-lock` pins can reference. See
  `.changeset/enchanter-core-web-v0-7-0.md` for the exact file-level diff
  driving the bump, and `docs/CROSS_REPO_VERSIONING.md` for how per-package
  git tags relate to the `@enchanter-ai/vis-meta` changesets version (bumped
  `0.1.0` → `0.2.0` alongside this entry — see that doc's "TL;DR" for why the
  two numbers are independent).
- Cutting and pushing the actual `enchanter-<pkg>--v0.7.0` git tags is a
  release action for the vis owner; this repository's remediation clone
  records the changeset and this entry only. Downstream `.vis-versions` pins
  that reference `~0.7.0` before the tags exist will fail loud
  (`tag missing in vis: enchanter-<pkg>--v0.7.0`) rather than silently
  resolving to something else — this is by design (WIX-INSTALL-002's
  fail-loud contract), not a defect in this changelog entry.

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


