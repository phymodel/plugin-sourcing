# Plugin Sourcing Rule

ChatStudio rule — **modify the source repository, never a copy**.

## Purpose

Stop an Agent from "fixing" a package only inside a copy that will be overwritten or that nobody
else can see. The package's upstream repo is the single source of truth.

The local directory registered in `~/.chatstudio/*.json` is the live package: the loader reads it
straight from disk (no download / cache / sync / patch step). Find its upstream repo with:

```bash
git -C <registered path> remote -v
```

## Quick summary

| Item | Action |
|---|---|
| `RULE.md`, `HARNESS.md`, `SKILL.md`, `RULER.md`, `README.md`, `references/`, `scripts/`, `docs/`, `config/` | edit in the upstream repo → commit (push per project policy) |
| `dataset/**` outputs, `llm/dataset/**`, `llm/out/`, `.build/**`, `.state/**`, `.pipeline_status.json`, checkpoints | runtime artifacts — writable locally |

## Workflow

1. Edit in the package's source repo
2. `commit` in that checkout; `push` when the project does it that way and the user agrees — and
   say so if the push fails
3. Reload the package in Studio (the panel re-reads the local `path`)

## Rule file

[RULE.md](./RULE.md) — the text ChatStudio injects; it carries the decision test and the full
exception list.
