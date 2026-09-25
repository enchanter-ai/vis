---
"@enchanter-ai/vis-meta": minor
---

Cut `enchanter-{core,skills,web,orchestration}--v0.7.0` package tags.

WIX-INSTALL-002: the previously cut `v0.6.0` per-package tags (four separate annotated tag
objects — `enchanter-core--v0.6.0` is `4b76f9a74454a47be6bea7beb8c426c29c106f9c`,
`enchanter-skills--v0.6.0` is `0691eef236e163b3ed79e0664cef0f58db47320c`,
`enchanter-web--v0.6.0` is `dc4b1b7f6782dc03b1d81d0d1d0e1a52dbebab19`,
`enchanter-orchestration--v0.6.0` is `5d16a45d92b6da92076be41a4866dfaa1837a317` — all four peel to
the same commit, `d0d0f9c4d1e82076bce12e74e81db33d0afedc5d`, 2026-05-11) no longer identify the
conduct content downstream consumers (Wixie's `CLAUDE.md`) actually import. As of this commit,
`core` has gained `capability-fidelity.md`, `metacognition.md`, `precedent-freshness.md`,
`prior-art-discovery.md`, `reversibility-foresight.md`, `substrate-consumption.md`,
`sunk-cost-iteration.md`, `verdict-calibration.md`; `web` has gained `citation-verification.md`,
`mcp-research-discipline.md`, `research-pipeline.md`, `source-discipline.md`; `skills` and
`orchestration` also carry small content changes since their `v0.6.0` tags. Full count since
`d0d0f9c4`: 23 core files, 9 skills files, 3 orchestration files touched (additions and edits).

**This is NOT purely additive — two behavioral/contract changes are included, both already on
this branch before this changeset, neither reverted or flagged as breaking until now:**

- `packages/core/conduct/hooks.md` — Pattern 1 (PreToolUse destructive-op deny) changes its
  documented deny exit code from **1 to 2**. Any hook a downstream consumer wrote against the
  old documented contract (`exit 1` denies) now has its "deny" treated by Claude Code as a
  non-blocking error instead of a block — a silent behavior change for anyone who followed the
  old doc literally and never updated their hook script.
- `packages/core/scripts/conduct-abi-check.sh` — the default canonical path changes from
  `../agent-foundations/conduct` to `../vis/conduct`; a repo with no local `shared/conduct/` now
  reports `SKIPPED` on stderr with exit 0 (previously: silent `exit 0` with a stdout message,
  indistinguishable from a real pass) and a new `--strict` / `CONDUCT_ABI_STRICT=1` mode reports
  the same condition as exit 2. A caller that only checked `$? -eq 0` and never read stderr
  behaves the same; a caller that greps stdout for a specific old message string does not.

**Open decisions for the vis owner, not resolved by this changeset:**

1. **0.7.0 vs 1.0.0.** Given the two behavioral changes above, is `minor` still the right bump,
   or does the `hooks.md` deny-code change (a real contract break for anyone depending on the old
   documented exit code) warrant `major` per semver, i.e. `1.0.0`? This changeset ships as
   `minor` because the package is still pre-1.0 (anything-goes minor bumps per semver's own 0.x
   carve-out) and because that mirrors the `v0.6.0` precedent's cadence, not because the change is
   actually non-breaking.
2. **Annotated vs lightweight tags.** The `v0.6.0` precedent used four annotated tags (tagger
   `klaiderman`). Continue that, or switch to lightweight? This changeset does not cut tags
   itself either way (see below).
3. **Meta-package bump rule.** Should cutting per-package git tags bump
   `@enchanter-ai/vis-meta` at all? The `v0.6.0` precedent did neither (no changeset, no
   package.json bump — meta stayed `0.1.0`). This changeset takes the position that the
   changesets tool's own documented flow (`.changeset/README.md`: "The author of a normal PR
   writes the changeset; the maintainer just merges the Versions PR") is the only mechanism that
   should ever move `package.json` — so this changeset exists, but `package.json` itself is
   **not** hand-edited here; only `changeset version` (run on the auto-opened Versions PR) should
   bump it, to `0.2.0`. Whether that coupling (a per-package tag cut implies a meta bump at all)
   is the right rule long-term is still open.

This changeset records intent only. Cutting the actual `enchanter-<pkg>--v0.7.0` git tags against
this repository (and, later, pushing them) is a release action for the vis owner, outside this
changeset's scope — see the `CHANGELOG.md` entry below (this repo has one root-level changelog,
not a per-package `packages/*/CHANGELOG.md` — no such path exists) and `docs/CROSS_REPO_VERSIONING.md`
for how downstream plugin repos consume this.
