---
name: parallel-orchestration
description: Coordinate safe multi-agent parallel execution with ownership, consolidation, and centralized memory.
---

# Parallel Orchestration

## Goal

Run multiple agents in parallel only when their work is truly independent and can be consolidated safely.

## Use this skill when

- a task spans multiple subsystems
- Backend, Frontend, or Data SQL may work concurrently
- you need explicit ownership and merge order
- you want centralized memory handling with Engram

## Preconditions

- Explorer has identified relevant files and shared-surface risks
- Spec Writer has defined scope boundaries and cross-lane dependencies
- Architect has defined ownership plan, dependency graph, and serialization points

If any precondition is missing, do not parallelize yet.

## Rules

- Parallelize only independent lanes
- Assign a single owner per file, interface, or subsystem for each phase
- If two lanes touch the same file, contract, or integration surface, serialize that work
- If independence is unclear, fall back to sequential execution
- Do not ask Reviewer to review partial results as final
- Test and DevOps may run in parallel only after consolidation

## Workflow

1. Build lanes from the approved scope boundaries
2. Assign ownership for files, interfaces, and subsystems
3. Record each lane's touch scope before implementation
4. Execute only disjoint lanes in parallel
5. Stop a lane immediately if it discovers a new dependency or overlap
6. Consolidate all lane outputs before validation and review
7. Run Test and DevOps on the consolidated result when applicable
8. Run Reviewer last on the consolidated result

## Conflict handling

- If a lane reports overlap, abort parallel execution for the overlapping area
- Re-plan the overlapping work as sequential or as a narrower lane
- Do not merge conflicting outputs without Orchestrator review

## Memory policy

- All agents may read Engram for context
- Only the Orchestrator writes final memory
- Worker agents return memory candidates with type, title, summary, location, and risk/learning
- Deduplicate candidates before persisting final observations

## Output

1. Lane plan
2. Ownership map
3. Shared-surface risks
4. Consolidation plan
5. Escalation conditions
6. Memory candidate handling
