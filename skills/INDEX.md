# Skills

## Meta-Skills

Operate on the skill system itself. 

### write-skill

Triggers: `write skill`, `create skill`, `new skill`, or auto-invoked by skill-extraction rule.

1. Identify the repeating workflow in one sentence
2. Name — verb-noun or verb: `tidy`, `write-skill`, `audit-skills`
3. Extract 2–4 trigger keyword phrases
4. Check trigger uniqueness against this dispatch table
5. Write steps — imperative verb first, concrete actions
6. Write constraints — binary testable, not subjective
7. Validate anatomy — exactly: Trigger, Workflow, Constraints
8. Add entry to dispatch table below
9. Read back dispatch table, confirm no trigger overlaps

Constraints: triggers must not overlap, steps must be imperative, constraints must be pass/fail, one skill per file.

### audit-skills

Triggers: `audit skills`, `review skills`, `skill check`, `skill health`

1. Read this dispatch table, collect all registered skills
2. Check every file link in dispatch table resolves to an existing file
3. Check every `.md` in `skills/` (except AGENTS.md, INDEX.md) has a dispatch entry
4. Detect trigger keyword overlaps across skills
5. Validate each skill file has Trigger, Workflow, Constraints sections
6. Verify steps start with imperative verbs
7. Verify constraints are binary testable
8. Report using `?(Review)` checklist format

Constraints: read-only — report only, do not modify. Report all issues, not just first.

---

## Dispatch

| Trigger | Skill | Priority |
|---------|-------|----------|
| tidy, reorganize, clean up | [tidy](tidy.md) | MEDIUM |
