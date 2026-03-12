# Proposal

- **Change ID**: adopt-sdd-hybrid
- **Status**: approved
- **Owner**: Orchestrator

## Goal

Adopt a lightweight, artifact-driven SDD workflow inspired by gentle-ai without forcing heavy process on trivial changes.

## In Scope

- Add versioned change artifacts under `specs/changes/<change-id>/`
- Gate medium-risk and high-risk changes into SDD
- Update agent prompts to operate on named artifacts and task IDs
- Enrich selected skills with rich frontmatter and usage guidance
- Add minimal SDD skills for propose/design/tasks/verify/archive
- Align Engram persistence to `change-id` and archived conclusions

## Out of Scope

- Replacing Codex configuration format
- Making SDD mandatory for every trivial change
- Adding external dependencies

## Risks

- Process overhead if agents use SDD for trivial tasks
- Drift between implementation and artifacts if prompts stay vague
- README becoming stale if not updated

## Unknowns

- Exact future balance between bypassed and gated changes
- Whether additional foundation skills will be standardized later

## Shared Surfaces / Parallelism Notes

- Orchestrator, AGENTS policy, skills, README, and memory instructions must stay aligned
- This change touches both process docs and agent prompts, so artifact-driven coordination is appropriate

## Approval Notes

Use a soft gate: require SDD for multi-scope, risky, SQL, cross-contract, or parallel work; allow explicit bypass for trivial docs or local fixes.
