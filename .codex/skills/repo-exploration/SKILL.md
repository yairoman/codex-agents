---
name: repo-exploration
description: >
  Analyze the repository before changing code, identify relevant files, and surface implementation constraints.
  Trigger: When a task starts, when scope is unclear, or when an SDD proposal needs repository evidence.
license: Apache-2.0
metadata:
  author: gentleman-programming
  version: "1.0"
allowed-tools: Read, Grep, Glob, Bash
---

# Repository Exploration

## When to Use

Use this skill when:
- a task begins and the codebase must be grounded first
- the Orchestrator needs proposal-ready evidence
- a change may touch multiple files or subsystems
- you need to identify shared surfaces before parallel execution

## Critical Patterns

- Do not modify files during exploration
- Prefer concrete paths and patterns over guesses
- Distinguish confirmed facts from assumptions
- Highlight safe opportunities for parallel work
- Flag shared-file and shared-interface risks early
- Gather findings in a format reusable for `proposal.md`

## Decision Tree

| Situation | Action |
|---|---|
| Trivial one-file change | Keep exploration short but still identify the owning file and test surface |
| Multi-layer or risky change | Expand exploration to contracts, data flow, rollout, and tests |
| Possible parallel work | Identify ownership boundaries and shared surfaces explicitly |
| Unknown behavior | Mark it as unknown instead of guessing |

## Output

1. Relevant files
2. Existing patterns
3. Likely implementation path
4. Dependencies
5. Risks
6. Parallelization opportunities
7. Shared-file risks
8. Proposal-ready summary

## Code Examples

```text
Relevant files:
- src/api/orders.ts
- src/ui/orders/OrderForm.tsx

Risks:
- Shared validation schema used by API and UI
- Contract changes will require serialized work
```

## Commands

```bash
rg -n "OrderForm|orders" .
find . -path '*test*' -o -path '*spec*'
```

## Resources

- **Change artifact guide**: `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/specs/changes/README.md`
- **Proposal template**: `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/specs/changes/_template/proposal.md`
