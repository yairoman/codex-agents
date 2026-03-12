# AGENTS.md

## Project Overview

This repository contains the codebase for this project.

Agents operate using a structured multi-agent workflow coordinated by the Orchestrator.

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

---

# Mandatory Workflow

Every task should follow this phased workflow unless the Orchestrator explicitly decides otherwise.

1. **Explorer** analyzes the repository
2. **Spec Writer** converts the request into a precise specification
3. **Architect** defines the technical approach
4. **Orchestrator** approves scope boundaries, ownership map, and lane plan
5. **Implementation agents** execute in parallel only when their scopes are independent
6. **Orchestrator** consolidates results and resolves conflicts
7. **Test Agent** and **DevOps Agent** may validate in parallel on the consolidated result when applicable
8. **Reviewer** performs the final review on the consolidated result
9. **Orchestrator** publishes the final summary and persists final memory

## Parallel Execution Policy

- Parallel execution is allowed only when file ownership, interfaces, and subsystem boundaries are explicit
- Backend, Frontend, and Data SQL may run in parallel only if the Orchestrator assigns disjoint ownership
- If two lanes touch the same file, shared contract, or integration surface, that part must be serialized
- If independence is unclear, do not parallelize
- No agent should review or validate partial results as if they were final
- The Orchestrator owns merge order, conflict resolution, and fallback to sequential execution

---

# Agent Responsibilities

## Orchestrator

Coordinates phases, assigns ownership, controls parallelism, consolidates results, and persists final memory.

## Explorer

Analyzes the repository, identifies relevant files and patterns, and highlights safe parallelization opportunities.

## Spec Writer

Creates implementation-ready specifications, including lane boundaries and cross-lane dependencies.

## Architect

Defines the safest and simplest technical design, including ownership plan, merge order, and serialization points.

## Backend / Frontend / Data SQL

Implements code changes according to the architecture and assigned ownership only.

## Test Agent

Validates lane-level behavior and consolidated integration behavior.

## Reviewer

Performs final technical review only after consolidation.

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

---

# Coding Principles

Agents should prioritize:

- clarity
- simplicity
- maintainability
- minimal risk
- adherence to repository conventions

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

### When final memory should be stored

The Orchestrator should persist final memory when:

- a new architecture decision is confirmed
- a repository convention is confirmed
- a bug investigation identifies a root cause
- a feature implementation reveals important constraints
- a session completes with important outcomes

### What NOT to store

Agents must NOT store:

- temporary debugging notes
- raw logs
- large code snippets
- exploratory thoughts
- trivial implementation details
- partial lane notes that will be absorbed into the final summary

### Memory categories

Memory candidates and final summaries should use one of the following categories:

- architecture_decision
- repo_convention
- bug_investigation
- feature_context
- session_summary
