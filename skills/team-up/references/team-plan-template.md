# Team Plan Template

Present the team plan using the sections below. Fill in all tables based on the task decomposition.

---

## Team Overview

| Teammate | Role | Tasks | Key Files |
|----------|------|-------|-----------|
| `teammate-1` | Brief role description | #1, #2, #3 | `src/foo.ts`, `src/bar.ts` |
| `teammate-2` | Brief role description | #4, #5 | `src/baz.ts`, `tests/baz.test.ts` |

## Task Breakdown

| # | Task | Owner | Blocked By | Files Owned (Read/Write) |
|---|------|-------|------------|--------------------------|
| 1 | Task title | `teammate-1` | — | `src/foo.ts` |
| 2 | Task title | `teammate-1` | #1 | `src/bar.ts` |
| 3 | Write tests for foo and bar | `teammate-1` | #1, #2 | `tests/foo.test.ts`, `tests/bar.test.ts` |
| 4 | Task title | `teammate-2` | — | `src/baz.ts` |
| 5 | Write tests for baz | `teammate-2` | #4 | `tests/baz.test.ts` |

## Dependency Graph

```
#1 ──► #2 ──► #3
#4 ──► #5
```

Show the actual DAG. Use arrows (──►) to indicate "must complete before". Independent chains can be listed on separate lines.

## File Ownership Map

| File | Owner | Access |
|------|-------|--------|
| `src/foo.ts` | `teammate-1` | Read/Write |
| `src/bar.ts` | `teammate-1` | Read/Write |
| `src/baz.ts` | `teammate-2` | Read/Write |
| `src/foo.ts` | `teammate-2` | Read-only |

Every file that any teammate touches must appear here. A file may have only ONE Read/Write owner. Other teammates get Read-only access.

## Verification Checklist

Before presenting the plan to the user, verify:

- [ ] No file has more than one Read/Write owner
- [ ] Dependencies form a DAG (no cycles)
- [ ] Each teammate has 3-6 tasks
- [ ] At least one task per teammate is a testing/verification task
- [ ] All files from the task breakdown appear in the ownership map
- [ ] No teammate is blocked on all their tasks by another teammate (would serialize work)
