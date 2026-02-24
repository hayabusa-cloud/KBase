# KBase Communication Protocol

> **The Protocol** defines the strict interaction standard between Human (`@U`) and AI (`@A`).

## 1. Semantics

### Entities
- **@U (User)**: Initiates requests, defines constraints, validates outcomes.
- **@A (Agent)**: Executes tasks, reports status, delivers artifacts.

### States
| State | Semantics | Transition Rule |
|-------|-----------|-----------------|
| `ready` | Task accepted; processing proceeding normally. | Initial state or continued progress. |
| `blocked` | Task halted due to conflict, missing input, or ambiguity. | Violation of `((Must))` or missing `[[Must]]` input. |
| `done` | Task completed; all `[[Must]]` deliverables satisfied. | All `[[Must]]` present + `((Must))` satisfied. |

### Invariants
1.  **Schema Compliance**: All messages MUST follow the defined schema.
2.  **Atomic Deliverables**: `[[Must]]` items are binary (present/absent).
3.  **Fail-Fast**: If constraints conflict or inputs are missing, `@A` MUST transition to `blocked` immediately.
4.  **Binary Truth**: Queries `?[...]` MUST return `Yes` or `No`. Ambiguity requires `blocked`.

### Artifacts
Deliverables (`[[...]]`, `[...]`) are the atomic units of value.
- **Immutable**: Once delivered, an artifact is considered fixed for that turn.
- **Verifiable**: Each artifact must satisfy all applicable `((Constraints))`.

---

## 2. Syntax & Schema

### 2.1 Message Components
| Symbol       | Meaning | Behavior                                |
|--------------|---------|-----------------------------------------|
| `## Summary` | Executive Summary | **Mandatory** first section. 1-2 lines. |
| `!status:`   | State Flag | `ready` \| `blocked` \| `done`.         |
| `[[ ... ]]`  | Must Deliverable | Mandatory artifact.                     |
| `[ ... ]`    | Optional Deliverable | Nice-to-have artifact.                  |
| `(( ... ))`  | Must Constraint | Hard rule. Violation = `blocked`.       |
| `( ... )`    | Prefer Constraint | Soft rule. Best effort.                 |
| `( ... ) > ~( ... )` | Preference with Reduce | Prefer left; reduce tendency toward right. |
| `?( ... )`   | Validation/Query | Checklist or logic check.               |
| `< ... >`    | Input Context | Raw data, source code, or logs.         |
| `<< ... >>`  | Reference | Pointer to external source (URL/File).  |
| `@Q`         | Clarification | Question to resolve ambiguity.          |
| `> ...`      | Quote | Reply to specific context.              |
| `->`         | Control Flow | `?( Condition ) -> [ Action ]`.         |
| `[ X ] : ( Y )` | Review-Fix (soft) | Review X against expectation Y; fix if unsatisfied. Best effort. |
| `[[ X ]] : (( Y ))` | Review-Fix (hard) | Deep review X against Y; MUST satisfy. Violation = `blocked`. |
| `[ ... ](RO)` | Read-Only Optional | Like `[ ... ]` but DO NOT change product code. Analysis/review only. |
| `[[ ... ]](RO)` | Read-Only Must | Like `[[ ... ]]` but DO NOT change product code. Must deliver artifact read-only. |
| `[ ... ](+W)` | Write-Allow Optional | Like `[ ... ]` with explicit permission to edit product code. |
| `[[ ... ]](+W)` | Write-Allow Must | Like `[[ ... ]]` with explicit permission to edit product code. |
| `[ ... ](SK)` | Skill-First Optional | Like `[ ... ]` but explicitly search skills and rules, and use existing skills. |
| `[[ ... ]](SK)` | Skill-First Must | Like `[[ ... ]]` but explicitly search skills and rules, and use existing skills. |

### 2.1.1 Plan Mode for Major Edits

When making a very **major, large, or core** edit — regardless of `(+W)` — `@A` may ask `@U` whether to enter plan mode before proceeding.
If no explicit `(+W)` is present, `@A` SHOULD prefer entering plan mode for edits.

### 2.2 Response Schema (@A)
The Agent MUST output strictly in this order:

1.  `## Summary`
2.  `@A !status: <state>`
3.  `# Context / ## Topic / ### Task`
4.  `[[Must Deliverables]]` (Content)
5.  `[Optional Deliverables]` (Content)
6.  `((Must Constraints))` (Verification: OK/Note)
7.  `(Prefer Constraints)` (Verification: OK/Tradeoff)
8.  `?(Review)` (Checklist: `[x]` Pass, `[ ]` Todo, `[-]` N/A)
9.  `?[Closure]` (Final check: Yes/No)

---

## 3. Control Flow

### Standard Flow
`@U` Request → `@A` Response (`ready` -> `done`)

### Exception Flow (Blocked)
If `((Constraint))` violated OR `[[Deliverable]]` impossible:

```markdown
## Summary
Cannot proceed due to [Reason].

@A !status: blocked
# Context

## Conflicts
- ((Constraint A)) conflicts with ((Constraint B)): [Explanation]

## Missing
- [[Deliverable X]] requires input <Y>

@Q
1. [Question to resolve blocking]?
```

---

## 4. Templates

### Request (@U)
```markdown
@U
# {Project}
## {Topic}
### {Task}

[[ {Must Deliverable} ]]
[ {Optional Deliverable} ]

(( {Strict Constraint} ))
( {Soft Constraint} )

< {Input Context} >
<< {Reference} >>

?[ {Condition} ] -> [ {Next Step} ]
```

### Response (@A)
```markdown
## Summary
{Summary}

@A !status: {ready|blocked|done}
# {Project}
## {Topic}
### {Task}

[[ {Must Deliverable} ]]
{Content...}

(( Constraints ))
- {Rule} = OK

?( Review )
- [x] {Pass}
- [ ] {Todo}
- [-] {N/A}

?[ {Done?} ] = Yes
```
