---
name: sdd-propose
description: >
  Create a change proposal artifact with intent, scope, non-scope, risks, and unknowns.
  Trigger: When a change passes the SDD gate or when the Orchestrator needs a proposal before spec/design work begins.
license: Apache-2.0
metadata:
  author: gentleman-programming
  version: "1.0"
allowed-tools: Read, Write, Edit, Grep, Bash
---

# SDD Propose

## When to Use

Use this skill when:
- the Orchestrator decides a task requires SDD
- a change needs a formal `proposal.md`
- the repo needs to record intent, scope, and risks before implementation

## Critical Patterns

- Create the `change-id` before authoring the proposal
- Capture business intent and technical impact separately
- Record non-scope explicitly
- Include risks, unknowns, and shared surfaces
- Keep the proposal short enough to approve quickly

## Decision Tree

| Situation | Action |
|---|---|
| Small but risky change | Write a short proposal with clear risk callouts |
| Multi-layer change | Include lane candidates and shared-surface risks |
| Many unknowns | Call them out before spec/design starts |

## Output

1. Change ID
2. Goal
3. In scope
4. Out of scope
5. Risks
6. Unknowns
7. Approval notes

## Code Examples

```text
Change ID: adopt-sdd-hybrid
In scope:
- Add versioned change artifacts
- Enrich agent prompts
Out of scope:
- Replacing Codex config format
```

## Commands

```bash
mkdir -p specs/changes/<change-id>
cp specs/changes/_template/proposal.md specs/changes/<change-id>/proposal.md
```

## Resources

- **Artifact guide**: `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/specs/changes/README.md`
- **Proposal template**: `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/specs/changes/_template/proposal.md`
