# Rules

Constraints and conventions. Tier 2.

## Meta Rules

Always active. Auto-trigger during conversations — no user command required.

### auto-save

Auto-save findings and user decisions to `notes/` immediately. No explicit "save" command required.

| Finding | Target | When |
|---------|--------|------|
| Research results | `notes/{topic}/` | After completing research |
| User decisions | `notes/` | When user states a preference or decision |
| Insights, patterns | `notes/` | When insight crystallizes during work |
| Review outcomes | `notes/` | After completing review |
| Resolved questions | `notes/` | When ambiguity is resolved |

How: write `notes/{topic}.md` or `notes/{topic}-{yyyy-mm-dd}.md`, update `notes/INDEX.md`, follow `ROOT/style.md`, one concept per file.

### recall-first

Before any external lookup, search KBase first. External only when KBase has no entry or entry is stale.

```
rules/ → notes/ → refs/ → external
```

If found and not stale → use it. If not found or stale → external lookup, then save per auto-save.

### deduplication

Before creating any new file in `notes/`, `refs/`, or `rules/`, search existing entries for overlap. If found, extend the existing file. One concept, one file.

| Situation | Action |
|-----------|--------|
| Same concept, same directory | Extend existing file |
| Same concept, different tier | Keep higher-tier version, update if needed |
| Overlapping but distinct | Split shared content, cross-reference |

### promote-audit

Before promoting `notes/` → `refs/`, MUST audit via external verification.

1. Extract all factual assertions
2. Verify each against authoritative primary sources
3. Cross-reference against at least two independent sources
4. If sources conflict → document conflict before promoting
5. Only after all claims verified → move to `refs/`, update both INDEX.md

Do NOT promote if: any claim unverified, sources conflict without resolution, info is time-sensitive and possibly stale, primary source unavailable.

### staleness-review

All `refs/` entries must carry `Verified: YYYY-MM-DD`. Auto-triggers when reading or citing any ref.

| Age | Action |
|-----|--------|
| < 6 months | Use as-is |
| 6–12 months | Re-verify before citing |
| > 12 months | Re-verify or demote to `notes/` |

If re-verification fails → demote back to `notes/` with a staleness note.

### skill-extraction

When a user repeats the same workflow twice, extract into a skill on second occurrence. Do not ask — extract immediately.

Detection: same step sequence, same trigger intent, same input→output shape, same transformation on different targets.

1. Recognize recurring pattern
2. Create `skills/{workflow}.md` with Trigger, Workflow, Constraints
3. Add to `skills/INDEX.md` dispatch table
4. Execute current request using the new skill
5. Inform user

---

## Domain Rules

| Rule | Scope | Status |
|------|-------|--------|
<!-- Domain-specific rules as separate files below -->
