---
"@enchanter-ai/vis-meta": minor
---

Cut `enchanter-{core,skills,web,orchestration}--v0.7.0` package tags.

WIX-INSTALL-002: the previously cut `v0.6.0` per-package tags (all at
`4b76f9a74454a47be6bea7beb8c426c29c106f9c`, 2026-05-11) no longer identify the
conduct content downstream consumers (Wixie's `CLAUDE.md`) actually import. As
of this commit, `core` has gained `capability-fidelity.md`, `metacognition.md`,
`precedent-freshness.md`, `prior-art-discovery.md`, `reversibility-foresight.md`,
`substrate-consumption.md`, `sunk-cost-iteration.md`, `verdict-calibration.md`
(plus small edits to `doubt-engine.md`, `failure-modes.md`, `hooks.md`); `web`
has gained `citation-verification.md`, `mcp-research-discipline.md`,
`research-pipeline.md`, `source-discipline.md`; `skills` and `orchestration`
also carry small content changes since their `v0.6.0` tags. None of this is a
breaking change to existing consumers of the pinned content (every file
present at `v0.6.0` is unchanged or additive) — a minor bump.

This changeset records the intent; cutting the actual
`enchanter-<pkg>--v0.7.0` git tags against this repository (and, later,
pushing them) is a release action for the vis owner outside this changeset's
scope. See `packages/*/CHANGELOG` entry below and
`docs/CROSS_REPO_VERSIONING.md` for how downstream plugin repos consume this.
