---
name: parallel-orchestration
description: >
  Coordinate safe multi-agent parallel execution with ownership, consolidation, and centralized memory.
  Trigger: When Backend, Frontend, and/or Data SQL may work concurrently after design constraints are approved.
license: Apache-2.0
metadata:
  author: gentleman-programming
  version: "1.0"
allowed-tools: Read, Grep, Bash, Task
---

# Parallel Orchestration

## When to Use

Use this skill when:
- a task spans multiple subsystems
- Backend, Frontend, or Data SQL may work concurrently
- ownership and merge order need to be made explicit
- an SDD-gated change already has approved `spec.md` and `design.md`

## Critical Patterns

- Parallelize only independent lanes
- Assign a single owner per file, interface, or subsystem for each phase
- If two lanes touch the same file, contract, or integration surface, serialize that work
- If independence is unclear, fall back to sequential execution
- Do not ask Reviewer to review partial results as final
- Test and DevOps may run in parallel only after consolidation
- Require each lane to reference task IDs from `tasks.md`

## Decision Tree

| Situation | Action |
|---|---|
| Shared file or contract | Serialize that area of work |
| Clear backend/frontend split | Run lanes in parallel with explicit ownership |
| New dependency discovered mid-lane | Stop the lane and return to Orchestrator |
| No approved design | Do not parallelize yet |

## Workflow

1. Confirm `spec.md` and `design.md` are approved
2. Build lanes from the approved scope boundaries
3. Assign ownership for files, interfaces, and subsystems
4. Record each lane's touch scope and task IDs in `tasks.md`
5. Execute only disjoint lanes in parallel
6. Stop a lane immediately if it discovers a new dependency or overlap
7. Consolidate all lane outputs before validation and review
8. Run Test and DevOps on the consolidated result when applicable
9. Run Reviewer last on the consolidated result

## Commands

```bash
rg -n "REQ-|TASK-" specs/changes/<change-id>
```

## Resources

- **Tasks template**: `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/specs/changes/_template/tasks.md`
- **Design template**: `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/specs/changes/_template/design.md`
