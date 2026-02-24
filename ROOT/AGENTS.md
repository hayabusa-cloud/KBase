## AGENTS.md -- ROOT

Highest authority. Read first.

### Bootstrap

`AGENTS.md` → `ROOT/AGENTS.md` → `ROOT/communication.md` → `ROOT/instructions.md` → `rules/INDEX.md` → `skills/INDEX.md` → `notes/INDEX.md`

After compaction: re-read first four minimum. See `compaction.md`.

### Reference

| File | When |
|------|------|
| `style.md` | Writing KBase content |
| `template.md` | Creating new directories |
| `compaction.md` | Context approaching limits |
| `communication.md` | Structured interaction with user |

### Principles

1. **Active persistence** — Auto-save findings immediately without user request
2. **KBase-first** — Search order: rules → notes → refs → skills → external
3. **Authoritative sources** — Verify against primary sources; unverified knowledge is dangerous
4. **Doc-first** — Update KBase before acting; conflict → update lower-tier source
5. **Skill dispatch** — Check `skills/INDEX.md` before ad-hoc work
6. **Plan-mode bias** — Prefer entering plan mode before major edits

### Source of Truth

| Tier | Source |
|------|--------|
| 1 | `ROOT/` |
| 2 | `rules/` |
| 3 | `refs/` |
| 4 | `notes/` |
| 5 | External |

Higher tier overrides lower. Conflict → update lower-tier source.

### Auto-Save

| Finding | Target |
|---------|--------|
| Constraints, conventions | `rules/` |
| Insights, decisions, patterns | `notes/` |
| Verified external knowledge | `refs/` |
| Automation workflows | `skills/` |
| Task plans | `plans/` |

### Lifecycle

| Transition | When |
|------------|------|
| notes → refs | Verified against primary source |
| notes → rules | Pattern recurs, becomes constraint |
| plans → notes/rules | Plan completed, outcomes extracted |

### Status

Used in INDEX.md across all directories.

| Tag | Meaning |
|-----|---------|
| Active | Current |
| Reference | Stable |
| Resolved | Completed |
| Archived | Superseded |

### Cross-Referencing

Relative paths: `[topic](../refs/domain/topic.md)`. Backtick-quote directories: `rules/`, `notes/`.
