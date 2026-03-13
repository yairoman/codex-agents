# AGENTS.md

## Project Overview

This repository contains the operating system for this project’s multi-agent workflow.

Agents operate using a structured workflow coordinated by the Orchestrator.
The goal is to produce reliable, minimal, and reviewable changes while respecting repository conventions.

---

# Core Rules

- Always explore the repository before implementing changes
- Prefer existing patterns over introducing new ones
- Keep changes small and reviewable
- Avoid modifying unrelated files
- Do not introduce new dependencies without justification
- Preserve backward compatibility unless explicitly required otherwise
- Always validate behavior with tests when possible
- Prefer sequential execution unless independence is explicit
- Assign a single owner per file or subsystem in each implementation phase
- Use SDD artifacts for medium-risk and high-risk changes

---

# SDD Artifacts

For changes that pass the SDD gate, the Orchestrator must create a change folder under:

```text
specs/changes/<change-id>/
```

Each gated change should use these artifacts:

- `proposal.md` — intent, scope, non-scope, risks, unknowns
- `spec.md` — requirements, scenarios, acceptance criteria
- `design.md` — technical decisions, ownership, dependencies, serialization points
- `tasks.md` — implementation tasks, lane assignment, task IDs, status
- `verify.md` — validation against requirements and task completion
- `archive.md` — final summary, follow-up work, archived learnings

## SDD Gate Policy

The Orchestrator must require SDD artifacts when a task:

- touches more than one subsystem
- requires parallel execution
- changes contracts, interfaces, or database behavior
- carries regression, rollout, or operational risk
- requires handoff between agents

The Orchestrator may bypass full SDD when a task is:

- documentation-only
- a trivial local change in one scope
- a low-risk fix with no shared contract impact

When bypassing SDD, the Orchestrator must state the reason explicitly.

---

# Mandatory Workflow

Every task should follow this phased workflow unless the Orchestrator explicitly decides otherwise.

1. **Explorer** analyzes the repository and gathers proposal-ready context
2. **Spec Writer** converts the request into a precise specification
3. **Architect** defines the technical approach and ownership constraints
4. **Orchestrator** decides whether SDD is required, assigns a `change-id`, and approves scope boundaries
5. **Implementation agents** execute only against approved task IDs and only in parallel when scopes are independent
6. **Orchestrator** consolidates results and resolves conflicts
7. **Test Agent** and **DevOps Agent** may validate in parallel on the consolidated result when applicable
8. **Reviewer** performs the final review on the consolidated result and artifacts
9. **Orchestrator** publishes the final summary, closes `archive.md`, and persists final memory

## Parallel Execution Policy

- During gated SDD planning, **Explorer**, **Spec Writer**, and **Architect** should run sequentially by default
- For gated changes, the default order is: Explorer -> Spec Writer -> Architect -> Orchestrator approval
- Parallel execution during planning is allowed only if the Orchestrator can defend independent outputs and no shared artifact will be blocked
- Parallel execution is allowed only when file ownership, interfaces, and subsystem boundaries are explicit
- Backend, Frontend, and Data SQL may run in parallel only if the Orchestrator assigns disjoint ownership
- If two lanes touch the same file, shared contract, or integration surface, that part must be serialized
- If independence is unclear, do not parallelize
- No agent should review or validate partial results as if they were final
- The Orchestrator owns merge order, conflict resolution, and fallback to sequential execution
- Parallel execution may start only after `design.md` is approved for gated changes

---

# Agent Responsibilities

## Orchestrator

Coordinates phases, assigns ownership, controls parallelism, manages SDD artifacts, consolidates results, and persists final memory.
For gated planning, it should prefer sequential handoffs with minimal context and only parallelize after `design.md` is approved.
For gated planning handoffs, it should default to `fork_context = false` and only inherit the full thread when a concrete blocker cannot be resolved with a compact summary.

## Explorer

Analyzes the repository, identifies relevant files and patterns, highlights safe parallelization opportunities, and feeds `proposal.md`.
It should keep outputs concise and avoid extra memory/doc lookups unless the prompt explicitly asks for them.

## Spec Writer

Creates implementation-ready specifications and writes `spec.md` for gated changes.
It should consume the Explorer summary instead of re-exploring broad repository context whenever possible.

## Architect

Defines the safest and simplest technical design, including ownership plan, merge order, serialization points, and `design.md`.
It should consume the consolidated spec/design inputs and avoid reopening unrelated historical context unless needed.

## Backend / Frontend / Data SQL

Implements code changes according to the architecture, assigned ownership, and approved task IDs.

## Test Agent

Validates lane-level behavior and consolidated integration behavior, and writes `verify.md` for gated changes.

## Reviewer

Performs final technical review only after consolidation, using the diff and relevant SDD artifacts.

## DevOps Agent

Evaluates deployment, infrastructure, and configuration impact on the consolidated result.

---

# Definition of Done

A task is considered complete when:

- Acceptance criteria are satisfied
- Tests pass or validation is documented
- Reviewer finds no blockers
- Operational impact is documented if applicable
- The Orchestrator has consolidated all parallel lanes
- Final memory has been persisted without duplicates or partial lane noise
- For gated changes, `archive.md` reflects the final outcome and follow-up work

---

# Coding Principles

Agents should prioritize:

- clarity
- simplicity
- maintainability
- minimal risk
- adherence to repository conventions
- artifact-driven coordination when risk justifies it

---

# Language Behavior

Agents should respond in the same language used by the user.

If the user writes in Spanish, responses should be in Spanish.

## Persistent Memory (Engram)

Agents should use Engram to store and retrieve high-value project knowledge.

### When to search memory

Agents may search Engram before performing work related to:

- architecture
- repository conventions
- known bugs
- module behavior
- prior technical decisions
- similar change artifacts or archived learnings

For worker agents, this should be **on-demand**, not mandatory by default. If the Orchestrator prompt already includes sufficient context, prefer using that context first.

### Who stores final memory

- Only the **Orchestrator** should persist final memory for a task
- Worker agents may read memory and emit **memory candidates**, but must not store final observations directly
- The Orchestrator deduplicates memory candidates before persisting them

### Memory candidates

When a worker agent finds something worth preserving, it should return a structured memory candidate with:

- suggested type
- suggested title
- what changed
- why it matters
- where it applies
- risk or learning
- `change-id` when one exists

### When final memory should be stored

The Orchestrator should persist final memory when:

- a new architecture decision is confirmed
- a repository convention is confirmed
- a bug investigation identifies a root cause
- a feature implementation reveals important constraints
- a session completes with important outcomes
- a gated change reaches `archive.md`

### What NOT to store

Agents must NOT store:

- temporary debugging notes
- raw logs
- large code snippets
- exploratory thoughts
- trivial implementation details
- partial lane notes that will be absorbed into the final summary
- drafts of SDD artifacts that are not approved

### Memory categories

Memory candidates and final summaries should use one of the following categories:

- architecture_decision
- repo_convention
- bug_investigation
- feature_context
- session_summary
