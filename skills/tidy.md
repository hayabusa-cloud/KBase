# tidy

## Trigger

`tidy <scope>`, `tidy all`

## Workflow

1. Read target INDEX.md files
2. Verify entries match actual files — remove dead links, add unindexed files
3. Update status tags (Active → Archived if superseded)
4. Consolidate duplicates
5. Apply lifecycle transitions (see `ROOT/AGENTS.md` Lifecycle)
6. Update all affected INDEX.md files

## Constraints

- Do not delete files — archive or move
- Do not change file content — only reorganize and re-index
- Verify before promoting notes to refs
