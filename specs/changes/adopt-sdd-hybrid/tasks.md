# Tasks

- **Change ID**: adopt-sdd-hybrid
- **Status**: done
- **Owner**: Orchestrator

## Task List

- TASK-1:
  - Owner: Orchestrator
  - Requirement: REQ-1, REQ-2
  - Lane: process-policy
  - Touch scope: `AGENTS.md`, `.codex/agents/`, `.codex/engram-*`
  - Depends on: none
  - Status: done

- TASK-2:
  - Owner: Orchestrator
  - Requirement: REQ-1
  - Lane: artifacts
  - Touch scope: `specs/changes/`
  - Depends on: TASK-1
  - Status: done

- TASK-3:
  - Owner: Orchestrator
  - Requirement: REQ-4
  - Lane: skills
  - Touch scope: `.codex/skills/`
  - Depends on: TASK-1, TASK-2
  - Status: done

- TASK-4:
  - Owner: Orchestrator
  - Requirement: REQ-6
  - Lane: docs
  - Touch scope: `README.md`
  - Depends on: TASK-1, TASK-2, TASK-3
  - Status: done

## Parallelism Notes

The work was largely documentation/configuration oriented and could be serialized safely in one lane. Future gated product changes may use multiple lanes after `design.md` approval.

## Blockers

None.

## Consolidation Plan

Review policy, prompts, skills, artifact templates, and README together, then close `verify.md` and `archive.md`.
