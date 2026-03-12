---
name: sdd-design
description: >
  Produce the design artifact for a gated change, including ownership, dependencies, serialization, and tradeoffs.
  Trigger: When `spec.md` is approved and the Orchestrator needs `design.md` before tasks or parallel execution.
license: Apache-2.0
metadata:
  author: gentleman-programming
  version: "1.0"
allowed-tools: Read, Write, Edit, Grep, Bash
---

# SDD Design

## When to Use

Use this skill when:
- a gated change has an approved spec
- architecture, ownership, or parallel execution must be defined
- the Orchestrator needs merge order and serialization points

## Critical Patterns

- Design from the approved spec, not from implementation convenience
- Define ownership by file, interface, or subsystem
- Mark serialization points explicitly
- Capture contract changes and rollback concerns
- Record tradeoffs, not only the chosen approach

## Decision Tree

| Situation | Action |
|---|---|
| Independent lanes | Define separate ownership and merge order |
| Shared interface | Mark serialization before implementation |
| DB or rollout risk | Add rollback/mitigation notes |

## Output

1. Proposed design
2. Ownership plan
3. Dependency graph
4. Merge order
5. Serialization points
6. Risks and tradeoffs

## Commands

```bash
cp specs/changes/_template/design.md specs/changes/<change-id>/design.md
```

## Resources

- **Design template**: `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/specs/changes/_template/design.md`
- **Tasks template**: `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/specs/changes/_template/tasks.md`
