---
name: sdd-tasks
description: >
  Break a gated change into implementation tasks with task IDs, lane assignment, ownership, and status tracking.
  Trigger: When `design.md` is approved and implementation needs a task graph.
license: Apache-2.0
metadata:
  author: gentleman-programming
  version: "1.0"
allowed-tools: Read, Write, Edit, Grep, Bash, Task
---

# SDD Tasks

## When to Use

Use this skill when:
- a gated change is ready for implementation planning
- lanes need explicit ownership and task IDs
- the Orchestrator must decide what can run in parallel

## Critical Patterns

- Give every task a stable ID like `TASK-1`
- Map each task to a requirement or scenario
- Assign one owner per task and one lane per implementation phase
- Mark blockers, dependencies, and completion status explicitly
- Keep tasks small enough for a single agent handoff

## Decision Tree

| Situation | Action |
|---|---|
| Shared dependency | Create an explicit prerequisite task |
| Parallel lanes | Separate tasks by owner and touch scope |
| Unknown implementation detail | Add a discovery/spike task before coding |

## Output

1. Task list
2. Ownership
3. Lane assignment
4. Dependencies
5. Status tracking

## Commands

```bash
cp specs/changes/_template/tasks.md specs/changes/<change-id>/tasks.md
```

## Resources

- **Tasks template**: `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/specs/changes/_template/tasks.md`
- **Parallel orchestration skill**: `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/skills/parallel-orchestration/SKILL.md`
