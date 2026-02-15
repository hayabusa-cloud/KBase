# Compaction

Protocol when context window approaches limits.

## Trigger

Act at ~80% context consumed or before a large operation.

## Procedure

1. Save unsaved findings → appropriate KBase directory
2. Update affected INDEX.md files
3. Save current task state → `plans/`
4. Context can now be safely compacted

## Recovery

Re-read: `AGENTS.md` → `ROOT/AGENTS.md` → `ROOT/instructions.md`.
Resume from `plans/` if task was in progress.
