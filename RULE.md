---
name: Plugin sourcing
description: Change a package in its upstream repo, never a copy. Commit there; push per project policy; report a failed push.
always: true
---

# Change the source, never a copy

A package registered in `~/.chatstudio/*.json` is a local `path`, read from disk as-is — no
download, cache or sync step. `git -C <registered path> remote -v` gives its upstream; any other
copy (cache dirs, clone bases, mirrors) is never the source of truth.

- **Package files** — `RULE.md` / `HARNESS.md` / `SKILL.md` / `RULER.md` / `README.md`, and
  `references/` / `scripts/` / `docs/` / `config/`: edit in the upstream checkout, then commit.
- **Push** only if that is the project's practice and the user agrees — a public repo or a
  half-finished change is a reason to hold. If the push fails (credentials, permissions,
  non-fast-forward), tell the user: a local-only edit is invisible to every other machine and is
  overwritten by the next clone.
- **Runtime artifacts** (produced while running: pipeline outputs, `dataset/**`, `out/`,
  checkpoints, logs, `.build/**`, `.state/**`) are writable locally. Their *declared shape*
  (schema, config keys, file names) is still package content and follows the rule above.

Done when `git -C <path> status --porcelain` is empty and `HEAD` matches `origin/<branch>`.
