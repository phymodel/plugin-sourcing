# Plugin Sourcing Rule: Modify Source Only, Never the Cache

## Cache Paths (Read-Only + Executable)

- `~/.chatstudio/cache/harnesses/fbc-data-generation/`
- `~/.phymodel/cache/harness/llm-trainer/`
- Any other mirrors under `~/.chatstudio/cache/` or `~/.phymodel/cache/`

## Prohibition

**Do NOT** perform `write_file` / `edit_file` / any disk modification on the following cached items:

- `HARNESS.md`
- `README.md`
- `RULER.md`
- `SKILL.md`
- `references/`
- `scripts/`
- Any configuration files

## Required Workflow

All modifications **must** be made in the GitHub source repositories:

- `phymodel__fbc-data-generation/` or
- `phymodel__llm-trainer/` (workspace root)

After making changes:

1. `commit` + `push` to the source repository.
2. Trigger a sync to refresh the local cache.

## Exception

Pipeline runtime artifacts are **not** subject to this rule, including but not limited to:

- `dataset/fbc_json*`
- `llm/out/`
- `.pipeline_status.json`

These are intended to be written and updated locally during execution.

## Conflict Resolution

**Source of truth = GitHub source.** If the cache diverges from the source:

1. Re-sync from source first.
2. **Never** patch the cache in place.
RULE_EOF; rc=$?; printf "\n__PHYM_CWD__%s\n" "$PWD"; exit $rc
