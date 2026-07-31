# Plugin Sourcing Rule

ChatStudio rule — **Modify Source Only, Never the Cache**.

## Purpose

Prevents AI agents from directly editing cached harness/plugin mirrors (e.g. `~/.chatstudio/cache/`, `~/.phymodel/cache/`). All changes must go through the GitHub source first, then sync.

## Rule File

See [RULE.md](./RULE.md) for the full rule content that ChatStudio consumes.

## Quick Summary

| Cached item | Action |
|---|---|
| `HARNESS.md`, `README.md`, `RULER.md`, `SKILL.md`, `references/`, `scripts/`, config files | ❌ Never modify |
| `dataset/fbc_json*`, `llm/out/`, `.pipeline_status.json` | ✅ Runtime artifacts, writable |

## Workflow

1. Edit in source repo → commit + push
2. Trigger cache sync
3. Never patch the cache in place
EOF; rc=$?; printf "\n__PHYM_CWD__%s\n" "$PWD"; exit $rc
