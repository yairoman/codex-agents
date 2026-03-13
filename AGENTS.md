# AGENTS.md

## Overview

This repo defines the operating system for the project’s multi-agent workflow. The goal is to produce reliable, minimal, reviewable changes that follow repository conventions.

## Core Rules

- Explore before changing anything.
- Prefer existing patterns and small diffs.
- Avoid unrelated changes and new dependencies unless justified.
- Preserve backward compatibility unless explicitly allowed otherwise.
- Validate behavior when possible.
- Prefer sequential execution unless independence is explicit.
- Assign a single owner per file, subsystem, or artifact for each phase.
- Use SDD artifacts for medium/high-risk work.

## SDD

For gated work, create `specs/changes/<change-id>/` with:

- `proposal.md`
- `spec.md`
- `design.md`
- `tasks.md`
- `verify.md`
- `archive.md`

Gate SDD when the task:

- touches multiple subsystems
- requires parallel execution
- changes contracts/interfaces/database behavior
- carries regression, rollout, or operational risk
- requires handoff between agents

Bypass SDD only for documentation-only work, trivial local changes, or low-risk fixes with no shared-contract impact. If bypassing, state the reason explicitly.

## Workflow

Default sequence:

1. `Explorer`
2. `Spec Writer`
3. `Architect`
4. `Orchestrator` approves scope and `change-id`
5. implementation agents
6. `Orchestrator` consolidates
7. `Test` and `DevOps` on the consolidated result when applicable
8. `Reviewer`
9. `Orchestrator` closes summary, archive, and final memory

For gated planning, use:

`Explorer -> Spec Writer -> Architect -> Orchestrator approval`

## Parallelism

- Gated planning is sequential by default.
- Implementation may parallelize only with explicit boundaries and disjoint ownership.
- `Backend`, `Frontend`, and `Data SQL` may run in parallel only if ownership is explicit.
- If two lanes touch the same file, interface, contract, or integration surface, serialize.
- `Test` and `DevOps` may run in parallel only after consolidation.
- `Reviewer` always runs after consolidation.
- If independence is unclear, do not parallelize.
- The Orchestrator owns merge order, conflict resolution, and fallback to sequential execution.

## Agent Responsibilities

- **Orchestrator**: gate/bypass SDD, assign `change-id`, control parallelism, consolidate outputs, persist final memory. For gated planning, prefer minimal-context handoffs and default to `fork_context = false`.
- **Explorer**: repository context, patterns, risks, proposal-ready findings.
- **Spec Writer**: implementation-ready `spec.md`.
- **Architect**: ownership, sequencing, serialization, `design.md`.
- **Backend / Frontend / Data SQL**: implement only approved task IDs and touch scope.
- **Test**: validate consolidated result and `verify.md`.
- **DevOps**: evaluate deployment/config/operational impact on the consolidated result.
- **Reviewer**: final review only on the consolidated result.

## Definition of Done

A task is done when:

- acceptance criteria are satisfied
- tests pass or validation is documented
- reviewer finds no blockers
- operational impact is documented when applicable
- the Orchestrator has consolidated all lanes
- final memory is persisted without duplicate partial-lane noise
- gated changes have an accurate `archive.md`

## Language

Respond in the same language as the user. If the user writes in Spanish, respond in Spanish.

## Memory (Engram)

- Use Engram for high-value project knowledge.
- Worker agents read memory only on-demand when the prompt or missing context justifies it.
- Only the **Orchestrator** persists final memory.
- Worker agents may emit **memory candidates**, but must not persist final observations.
- Prefer consolidated conclusions over intermediate notes.

Store final memory when:

- an architecture decision is confirmed
- a repository convention is confirmed
- a bug root cause is identified
- a feature reveals important constraints
- a session ends with important outcomes
- a gated change reaches `archive.md`

Do **not** store raw logs, drafts, large code snippets, exploratory thoughts, or partial lane notes.

Use these categories for memory candidates/final summaries:

- `architecture_decision`
- `repo_convention`
- `bug_investigation`
- `feature_context`
- `session_summary`
