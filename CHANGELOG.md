# Changelog

## 0.7.0 - enchanter-{core,skills,web,orchestration,cost,memory,safety} (unreleased; tags not yet cut)

### Release metadata

- **Released components (computed dependency closure).** Seven packages move from `0.6.0` to
  `0.7.0`: `enchanter-core`, `enchanter-skills`, `enchanter-web`, `enchanter-orchestration`
  (the requested release) plus `enchanter-cost`, `enchanter-memory`, `enchanter-safety`, whose
  declared `enchanter-core ~0.6.0` range would reject `enchanter-core 0.7.0`. Claude Code
  installs one copy of each plugin, so without these three they would fail to load next to core
  `0.7.0` (and `orchestration` through its `cost` dependency). Each released
  `packages/<pkg>/.claude-plugin/plugin.json` and its `.claude-plugin/marketplace.json` entry
  declare `0.7.0`, so each `enchanter-<pkg>--v0.7.0` tag points at content whose manifests report
  `0.7.0`.
- **Dependency ranges.** `skills`, `web`, `orchestration`, `cost`, `memory` and `safety` depend
  on `enchanter-core ~0.7.0`; `orchestration` depends on `enchanter-cost ~0.7.0`.
- **Not released.** `enchanter-hooks` stays `0.6.0`: its dependency is a bare `enchanter-core`
  (any version), which already accepts core `0.7.0`. The marketplace catalog `metadata.version`
  also stays `0.6.0` (set once at the family split, never used as a per-release field).
- **Tags (owner decision).** Ordinary annotated, unsigned tags, all on one commit, message
  `enchanter-<pkg> 0.7.0`, following the `v0.6.0` precedent: `enchanter-core--v0.7.0`,
  `enchanter-skills--v0.7.0`, `enchanter-web--v0.7.0`, `enchanter-orchestration--v0.7.0`,
  `enchanter-cost--v0.7.0`, `enchanter-memory--v0.7.0`, `enchanter-safety--v0.7.0`.
  Version `0.7.0` (pre-1.0 minor), not `1.0.0`.
- **No `@enchanter-ai/vis-meta` bump (owner decision).** The root `package.json` records no
  component versions, and the `v0.6.0` cut did not bump it either, so it stays `0.1.0`. The
  pending `.changeset/enchanter-core-web-v0-7-0.md` (`"@enchanter-ai/vis-meta": minor`) was
  removed so that `changesets.yml` does not queue a vis-meta `0.2.0` "Version Packages" PR; its
  release notes are folded into this entry.
- **Why the release.** WIX-INSTALL-002: the `enchanter-<pkg>--v0.6.0` tags (seven separate
  annotated tag objects that all peel to `d0d0f9c4d1e82076bce12e74e81db33d0afedc5d`, 2026-05-11)
  no longer identify the conduct content downstream consumers (Wixie's `CLAUDE.md`) import.
  Since `d0d0f9c4`: `core` gained `capability-fidelity.md`, `metacognition.md`,
  `precedent-freshness.md`, `prior-art-discovery.md`, `reversibility-foresight.md`,
  `substrate-consumption.md`, `sunk-cost-iteration.md`, `verdict-calibration.md`; `web` gained
  `citation-verification.md`, `mcp-research-discipline.md`, `research-pipeline.md`,
  `source-discipline.md`; the other released packages carry smaller edits (see
  `git diff --stat enchanter-<pkg>--v0.6.0 -- packages/<pkg>` for the exact file lists).

### Behavioral changes (not purely additive)

- `packages/core/conduct/hooks.md`: Pattern 1 (PreToolUse destructive-op deny) documents deny
  exit code **2** instead of **1**. A hook written against the old doc (`exit 1` to deny) is now
  a non-blocking error in Claude Code, not a block.
- `packages/core/scripts/conduct-abi-check.sh`: the default canonical path changes from
  `../agent-foundations/conduct` to `../vis/conduct`; a repo with no local `shared/conduct/` now
  reports `SKIPPED` on stderr with exit 0 (previously a silent exit 0 with a stdout message), and
  a new `--strict` / `CONDUCT_ABI_STRICT=1` mode reports that condition as exit 2.

### Documentation

- `packages/orchestration/docs/vis-lock-spec.md` describes `.vis-lock` schema v2 (mode, tag,
  lock_version, strict `--verify` parsing, file-relative `@`-import resolution).
- Downstream `.vis-versions` pins of `~0.7.0` fail loud (`vis tag enchanter-<pkg>--v0.7.0 not
  found locally`) until the tags exist, by design.

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


