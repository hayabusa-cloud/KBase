## AGENTS.md -- KBase

Read this file and `ROOT/` at session start and after compaction.

### Bootstrap

`AGENTS.md` → `ROOT/AGENTS.md` → `ROOT/communication.md` → `ROOT/instructions.md`

### Structure

| Dir | Role | Nesting |
|-----|------|---------|
| `ROOT/` | Meta-principles, bootstrap, commands | — |
| `refs/` | Verified external knowledge | `<domain>/<topic>` |
| `notes/` | Project insights and decisions | `<project>/<topic>` |
| `skills/` | Automation workflows | `<tool>` or `<project>/<workflow>` |
| `rules/` | Constraints and conventions | `<role>`, `<lang>`, `<project>` |
| `env/` | Environment configurations | `<project>/<workflow>` |
| `perms/` | Secrets and credentials (gitignored) | `<role>` or `<project>` |
| `hooks/` | Event-driven callbacks | `<project>` |
| `plans/` | Auto-generated task plans | `<project>/<yyyymm>/<issue>` |

Every level has `AGENTS.md` (guide) and `INDEX.md` (contents).
