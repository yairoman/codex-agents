# Verify

- **Change ID**: adopt-sdd-hybrid
- **Status**: approved
- **Owner**: Test Agent

## Requirement Coverage

- REQ-1: pass
- REQ-2: pass
- REQ-3: pass
- REQ-4: pass
- REQ-5: pass
- REQ-6: pass

## Task Completion

- TASK-1: done
- TASK-2: done
- TASK-3: done
- TASK-4: done

## Passing Checks

- Verified that `specs/changes/` now documents artifact structure and templates.
- Verified that agent prompts reference `change-id`, artifacts, and task IDs for gated changes.
- Verified that SDD skills exist and selected skills now use richer frontmatter and resource sections.
- Verified that memory instructions now align persistence to approved archive conclusions.

## Failures / Gaps

- No automated test suite exists for prompt/skill markdown; validation is document-level.

## Reproduction Steps

- Inspect `AGENTS.md`
- Inspect `.codex/agents/`
- Inspect `.codex/skills/`
- Inspect `specs/changes/`
- Inspect `README.md`

## Residual Risk

- Teams must keep artifacts up to date during future gated changes.

## Deferred Checks

- Exercise the new flow on a real multi-layer change to validate ergonomics.
