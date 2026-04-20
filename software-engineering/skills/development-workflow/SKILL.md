---
name: development-workflow
description: Use when implementing any feature, component, or bugfix — covers planning, per-unit review, and git commit discipline. Apply to all coding tasks alongside domain-specific skills.
---

# Development Workflow

Shared workflow for all coding tasks: plan upfront, implement one unit at a time, review with user, commit.

## Trigger conditions

Apply alongside domain-specific skills (tdd, web-app-design, api-design) for all implementation work.

## The workflow

```
PLAN     → Write plan before any code
IMPLEMENT → Build one unit (test case, component, endpoint)
REVIEW   → Stop. Show diff to user. Await approval.
COMMIT   → git commit accepted changes; mark unit done in plan
REPEAT   → Next unit
```

## Planning

Before any code, write a plan to `.claude/plans/<type>-<feature-name>.md`. Use a prefix matching the work type: `tdd-`, `web-`, `api-`, etc.

```markdown
# Plan: <feature name>

## Tasks
- [ ] <unit of work>
- [ ] <unit of work>
- [ ] ...
```

Break work into independent deliverable units. After each committed unit, mark it `[x]` and save the file.

## Review and commit

After completing each unit of work:

1. **Stop.** Do not proceed to the next unit.
2. **Show the user** a summary of what changed (files modified, what behaviour or component was added).
3. **Wait for explicit approval** before committing. If the user requests changes, make them and re-show.
4. **On approval**, run:
   ```
   git add <changed files>
   git commit -m "<conventional commit message describing what was added>"
   ```
5. **Mark the unit `[x]`** in the plan file and save it.
6. Proceed to the next unit.

## Completion

When all tasks in the plan are marked `[x]`:

1. Delete the plan file.
2. Inform the user all work is complete.
