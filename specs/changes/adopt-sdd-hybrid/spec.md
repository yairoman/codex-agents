# Spec

- **Change ID**: adopt-sdd-hybrid
- **Status**: approved
- **Owner**: Spec Writer

## Problem Statement

The repository has a multi-agent workflow but lacks change-level SDD artifacts, soft gating rules, and skill structure comparable to gentle-ai’s richer workflow patterns.

## Scope

Introduce a lightweight SDD layer that keeps the current agents but adds change artifacts, prompt behavior tied to artifacts, and richer skills.

## Out of Scope

- Mandatory SDD for trivial tasks
- Runtime/tooling changes outside prompts, docs, and repo artifacts

## Requirements

- REQ-1: The repo must define a versioned artifact structure for gated changes.
- REQ-2: The Orchestrator must decide gated vs bypassed execution and assign a `change-id` for gated work.
- REQ-3: Agent prompts must reference artifacts and task IDs where relevant.
- REQ-4: The repo must provide minimal SDD skills for propose/design/tasks/verify/archive.
- REQ-5: Memory policy must align final persistence to archived conclusions and `change-id`.
- REQ-6: README should explain the SDD workflow and new skills.

## Scenarios / Acceptance Criteria

- SCN-1: A gated change can create `specs/changes/<change-id>/` with all six artifacts.
- SCN-2: A trivial docs task can be bypassed with an explicit reason and no artifact folder.
- SCN-3: Parallel work may start only after `design.md` and `tasks.md` exist.
- SCN-4: Test and Reviewer can verify work against requirement IDs and task IDs.

## Edge Cases

- A task starts as trivial but grows into a gated change.
- A parallel lane finds a shared-surface conflict after implementation starts.
- A change has no code diff but still has operational or architectural risk.

## Error Handling Expectations

- If required artifacts are missing for a gated change, the Orchestrator must block implementation.
- If a lane discovers spec/design drift, the lane must escalate instead of improvising.

## Security / Permission Considerations

- No secrets should be stored in artifacts.
- Artifacts should record security considerations when changes impact auth, permissions, or deployment.

## Scope Boundaries

- Process docs and prompts are in scope.
- Codex runtime config is unchanged.

## Lane Inputs / Outputs

- Explorer → proposal context
- Spec Writer → `spec.md`
- Architect → `design.md`
- Orchestrator → `tasks.md` and `archive.md`
- Test → `verify.md`

## Cross-Lane Dependencies

- `design.md` depends on `spec.md`
- `tasks.md` depends on `design.md`
- Parallel implementation depends on approved `tasks.md`
- `verify.md` depends on consolidated implementation

## Open Questions / Assumptions

- Assume markdown artifacts are sufficient for v1.
- Assume selected skills are enough; full 9-skill SDD parity is deferred.
