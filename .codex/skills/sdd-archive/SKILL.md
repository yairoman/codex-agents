---
name: sdd-archive
description: >
  Close a gated change by archiving the final outcome, follow-ups, and stable learnings.
  Trigger: When review is complete and the Orchestrator is finalizing `archive.md` and memory.
license: Apache-2.0
metadata:
  author: gentleman-programming
  version: "1.0"
allowed-tools: Read, Write, Edit, Grep, Bash
---

# SDD Archive

## When to Use

Use this skill when:
- a gated change is complete
- the Orchestrator must finalize `archive.md`
- stable learnings need to be persisted to Engram without draft noise

## Critical Patterns

- Archive only approved outcomes
- Summarize what changed, why, and what remains
- Include follow-up work and deferred risks
- Persist memory from the archived conclusion, not from intermediate drafts

## Decision Tree

| Situation | Action |
|---|---|
| Change complete | Finalize archive and persist memory |
| Follow-up needed | Record it explicitly in archive and summary |
| Spec/design drift detected | Resolve drift before archiving |

## Output

1. Final summary
2. Requirements delivered
3. Follow-ups
4. Risks carried forward
5. Memory-ready conclusions

## Commands

```bash
cp specs/changes/_template/archive.md specs/changes/<change-id>/archive.md
```

## Resources

- **Archive template**: `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/specs/changes/_template/archive.md`
- **Verify template**: `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/specs/changes/_template/verify.md`
