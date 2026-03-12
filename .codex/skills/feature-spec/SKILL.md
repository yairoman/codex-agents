---
name: feature-spec
description: >
  Convert a feature request into an implementation-ready specification with explicit boundaries and scenarios.
  Trigger: When a request is ambiguous, when SDD is gated on, or when implementation needs acceptance criteria.
license: Apache-2.0
metadata:
  author: gentleman-programming
  version: "1.0"
allowed-tools: Read, Grep, Bash
---

# Feature Specification

## When to Use

Use this skill when:
- translating a user request into requirements
- preparing `spec.md` for a gated change
- defining acceptance criteria for Test and Reviewer
- clarifying lane boundaries before parallel work

## Critical Patterns

- Prefer testable requirements over descriptive prose
- Separate scope from non-scope
- Capture edge cases and error expectations
- Make lane boundaries and cross-lane dependencies explicit when relevant
- Give every important scenario a stable identifier
- Do not let implementation start from a vague spec in gated changes

## Decision Tree

| Situation | Action |
|---|---|
| Trivial, local task | Write a compact spec with only core acceptance criteria |
| Multi-layer change | Include requirement IDs, scenarios, and lane boundaries |
| Contract or DB change | Add error cases, compatibility notes, and explicit non-scope |
| Parallel lanes | Define inputs, outputs, and shared contracts per lane |

## Output

1. Problem statement
2. Scope
3. Out of scope
4. Acceptance criteria
5. Edge cases
6. Scope boundaries
7. Lane inputs and outputs
8. Cross-lane dependencies
9. Requirement IDs or scenario IDs
10. Assumptions

## Code Examples

```text
REQ-1: Users can save draft invoices without publishing them.
REQ-2: Publishing validates customer and tax fields.

Lane boundaries:
- Backend: draft persistence and publish API
- Frontend: draft UX and validation states
```

## Commands

```bash
rg -n "invoice|draft|publish" .
```

## Resources

- **Change artifact guide**: `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/specs/changes/README.md`
- **Spec template**: `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/specs/changes/_template/spec.md`
