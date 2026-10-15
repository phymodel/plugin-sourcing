---
name: Plugin sourcing
description: Modify a package in its upstream source repo only — never a mirror/clone copy that will be overwritten or that nobody else can see.
always: true
---

# Plugin Sourcing Rule: Modify Source Only, Never a Copy

## How packages are loaded (current mechanism)

ChatStudio packages (Harness / Skill / Rule / Subagent) are registered in
`~/.chatstudio/harnesses.json` / `skills.json` / `rules.json` / `subagents.json`. Each entry is a
**local `path`**, read from disk as-is — **there is no download, cache, TTL or patch step**. The
registered directory *is* the live package; normally it is a git working copy of that package's
own repo.

- Find the upstream repo with `git -C <registered path> remote -v`. Do not assume a fixed org or
  host — the same package may exist on GitHub and on Gitee, and the clone base differs per user.
- `~/.chatstudio/cache/**`, `~/.chatstudio/github/repos/**` and the repo clone bases
  (`~/Desktop/Repo/{GitHub,Gitee}/**`) may contain copies produced by Studio or other tools.
  Those copies are never the source of truth.

## Prohibition

Never leave a package change only inside a copy. For the package's own files — `RULE.md`,
`HARNESS.md`, `SKILL.md`, `RULER.md`, `README.md`, `references/`, `scripts/`, `docs/`, `config/`,
and other configuration — the change must land in the upstream repository.

## Required workflow

1. Edit the package in its source repo (a normal checkout you can commit from).
2. `commit` + `push` to its upstream. An uncommitted local edit is exactly the failure this rule
   guards against: invisible to every other machine, and overwritten by the next clone/sync.
3. Reload the package in Studio (the Rules / Harness / Skill panel re-reads the local `path`).
   There is no separate sync step.
4. If a copy disagrees with the source, re-clone / re-sync from the source first — never patch the
   copy in place.

## Decision test

Before saving, ask: **"is the target one of the package's own files (rule / doc / script / config)?"**
Yes → upstream repo, steps 1–2. No → it is a runtime artifact, see the exception below.

## Exception: runtime artifacts are writable locally

Files a pipeline produces while running are out of scope, for example: `dataset/**` outputs
(`fbc_json*`, `fbc_tmp`), `llm/dataset/**`, `llm/out/`, `.pipeline_status.json`, `.build/**`
(fetch / clean / emit intermediates, dedup sqlite), `.state/**` (browser harness shots, logs,
downloads, profile), training checkpoints and logs.

Their *declared shape* (schemas, config keys, file names) is still package content and follows the
rule above; only the produced files are exempt.
