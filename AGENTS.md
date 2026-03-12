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

---

# Mandatory Workflow

Every task should follow this sequence unless the Orchestrator explicitly decides otherwise.

1. Explorer analyzes the repository
2. Spec Writer converts the request into a precise specification
3. Architect defines the technical approach
4. Implementation agents perform code changes
5. Test Agent validates behavior
6. Reviewer performs final review
7. DevOps Agent evaluates deployment implications if needed

---

# Agent Responsibilities

## Orchestrator

Coordinates all agents and consolidates final results.

## Explorer

Analyzes the repository and identifies relevant files and patterns.

## Spec Writer

Creates implementation-ready specifications.

## Architect

Defines the safest and simplest technical design.

## Backend / Frontend / Data SQL

Implements code changes according to the architecture.

## Test Agent

Validates acceptance criteria and regression risks.

## Reviewer

Performs final technical review.

## DevOps Agent

Evaluates deployment, infrastructure, and configuration impact.

---

# Definition of Done

A task is considered complete when:

- Acceptance criteria are satisfied
- Tests pass or validation is documented
- Reviewer finds no blockers
- Operational impact is documented if applicable

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

Agents should search Engram before performing work related to:

- architecture
- repository conventions
- known bugs
- module behavior
- prior technical decisions

### When to store memory

Agents should store memory when:

- a new architecture decision is made
- a repository convention is discovered
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

### Memory categories

Stored memory should use one of the following types:

- architecture_decision
- repo_convention
- bug_investigation
- feature_context
- session_summary
