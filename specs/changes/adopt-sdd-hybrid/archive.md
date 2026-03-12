# Archive

- **Change ID**: adopt-sdd-hybrid
- **Status**: final
- **Owner**: Orchestrator

## Final Summary

The repository now supports a lightweight, artifact-driven SDD workflow inspired by gentle-ai. Gated changes create versioned artifacts, prompts operate on `change-id` and task IDs, selected skills use richer guidance, and Engram persists only final archived conclusions.

## Requirements Delivered

- Added change-level artifact structure and templates
- Added soft SDD gating and `change-id` ownership to the Orchestrator
- Updated agent prompts to work from artifacts and task IDs
- Added minimal SDD skills and enriched selected existing skills
- Updated memory policy to follow archived conclusions and `change-id`
- Updated README to document the new model

## Implementation Notes

The approach remains hybrid: trivial tasks may bypass SDD with an explicit reason; risky or multi-scope changes should use the full artifact flow.

## Validation Summary

Document-level validation confirmed the presence and alignment of policy, prompts, skills, templates, and README.

## Follow-Up Work

- Try the flow on a real backend + frontend change
- Consider enriching additional non-SDD skills to the same rich format later

## Risks Carried Forward

- Artifact drift is still possible if Orchestrator discipline is weak
- Teams may overuse SDD on trivial tasks unless bypass remains explicit

## Memory-Ready Learnings

- Hybrid SDD works best when gated by risk and scope, not universally
- `change-id` keeps memory, artifacts, and review aligned
- Rich skills are most valuable when they point to local templates and artifact contracts
