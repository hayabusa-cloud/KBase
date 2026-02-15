# KBase

A structured, self-organizing knowledge base for AI coding agents.

[English](README.md) | [中文](README.zh-CN.md) | [Español](README.es.md) | [日本語](README.ja.md) | [Français](README.fr.md)

## Background

KBase persists agent knowledge — rules, references, decisions, skills — across sessions, so agents start informed instead of cold.

## Structure

| Directory | Role |
|-----------|------|
| `ROOT/` | Meta-principles, bootstrap, commands |
| `refs/` | Verified external knowledge |
| `notes/` | Project insights and decisions |
| `skills/` | Automation workflows |
| `rules/` | Constraints and conventions |
| `env/` | Environment configurations |
| `perms/` | Secrets and credentials (gitignored) |
| `hooks/` | Event-driven callbacks |
| `plans/` | Auto-generated task plans |

Every directory contains two files: `AGENTS.md` (guide for the agent) and `INDEX.md` (table of contents).

## Principles

1. **Active persistence** — Agents save findings immediately, without being asked.
2. **KBase-first** — Search internal knowledge before external sources. Order: rules, notes, refs, skills, then external.
3. **Authoritative sources** — All knowledge verified against primary sources. Unverified knowledge is not stored.
4. **Doc-first** — Update documentation before acting on it. On conflict, update the lower-tier source.
5. **Skill dispatch** — Check existing skills before doing ad-hoc work.

## Communication Protocol

KBase defines a structured notation for human–agent interaction. Full spec: [`ROOT/communication.md`](ROOT/communication.md).

| Notation | Semantics |
|----------|-----------|
| `[[ ... ]]` | Must deliverable — mandatory artifact |
| `[ ... ]` | Optional deliverable |
| `(( ... ))` | Must constraint — violation blocks the task |
| `( ... )` | Prefer constraint — soft rule, best effort |
| `( ... ) > ~( ... )` | Preference with reduce — prefer left; reduce tendency toward right |
| `?( ... )` | Validation query |
| `< ... >` | Input context |
| `<< ... >>` | Reference pointer |
| `->` | Control flow — `?( Condition ) -> [ Action ]` |
| `[ X ] : ( Y )` | Review-fix (soft) — review X against Y; fix if unsatisfied |
| `[[ X ]] : (( Y ))` | Review-fix (hard) — deep review X against Y; must satisfy |

States: `ready` → `done` (normal) or `ready` → `blocked` (conflict/missing input).

## Authority

| Tier | Source |
|------|--------|
| 1 | `ROOT/` |
| 2 | `rules/` |
| 3 | `refs/` |
| 4 | `notes/` |
| 5 | External |

Higher tier overrides lower. On conflict, update the lower-tier source.

## Lifecycle

| Transition | When |
|------------|------|
| notes → refs | Verified against primary source |
| notes → rules | Pattern recurs, becomes a constraint |
| plans → notes/rules | Plan completed, outcomes extracted |

## Quick Start

1. Clone: `git clone https://github.com/hayabusa-cloud/KBase.git`
2. Point your AI coding agent's config to the `KBase/` directory.
3. Work. The agent reads `AGENTS.md` on session start and self-maintains the knowledge base.

## Auto-Save

The agent saves findings to the appropriate directory automatically:

| Finding | Target |
|---------|--------|
| Constraints, conventions | `rules/` |
| Insights, decisions, patterns | `notes/` |
| Verified external knowledge | `refs/` |
| Automation workflows | `skills/` |
| Task plans | `plans/` |

To add content manually, create a markdown file in the target directory and add an entry to its `INDEX.md`.

## Bootstrap

On session start, the agent reads in this order:

`AGENTS.md` → `ROOT/AGENTS.md` → `ROOT/instructions.md` → `rules/INDEX.md` → `skills/INDEX.md` → `notes/INDEX.md`

After context compaction, the agent re-reads the first three files minimum.

## Commands

```
research <topic>     Gather and verify information; save to refs/ or notes/
dive into <topic>    Multi-pass deep research with cutting-edge sources
review <subject>     Evaluate against KBase and web research
tidy <scope>         Reorganize structure and content; tidy all for entire KBase
recall <topic>       Search KBase for existing knowledge before external lookup
save <topic>         Save a finding to the appropriate directory; update INDEX.md
promote <file>       Move a note to refs/ or rules/; update both INDEX.md files
```

## License

[MIT](LICENSE) — ©2026 [Hayabusa Cloud Co., Ltd.](https://code.hybscloud.com)
