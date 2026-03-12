---
name: sdd-verify
description: >
  Validate a gated change against its requirements, tasks, and implementation outcomes.
  Trigger: When implementation has been consolidated and Test/Reviewer need to produce `verify.md`.
license: Apache-2.0
metadata:
  author: gentleman-programming
  version: "1.0"
allowed-tools: Read, Write, Edit, Grep, Bash
---

# SDD Verify

## When to Use

Use this skill when:
- implementation is complete or consolidated
- Test must validate requirement coverage
- Reviewer needs artifact-aware verification context

## Critical Patterns

- Verify against requirement IDs and task IDs, not just against the diff
- Separate passing checks, failures, and deferred validation
- Record residual risk explicitly
- Keep `verify.md` factual and reproducible

## Decision Tree

| Situation | Action |
|---|---|
| Requirement fully covered | Mark pass with evidence |
| Requirement partially covered | Mark partial/fail and note gap |
| Validation impossible now | Record deferred check and reason |

## Output

1. Requirement coverage
2. Task completion status
3. Failures and gaps
4. Residual risk
5. Follow-up checks

## Commands

```bash
cp specs/changes/_template/verify.md specs/changes/<change-id>/verify.md
```

## Resources

- **Verify template**: `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/specs/changes/_template/verify.md`
- **Spec template**: `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/specs/changes/_template/spec.md`
