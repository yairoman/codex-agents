# Design

- **Change ID**: adopt-sdd-hybrid
- **Status**: approved
- **Owner**: Architect

## Proposed Design

Layer a lightweight SDD process on top of the existing multi-agent flow using versioned markdown artifacts and richer skills. Keep the current agents but require them to operate on named artifacts when the soft gate is triggered.

## Impacted Components

- `AGENTS.md`
- `.codex/agents/*`
- `.codex/skills/*`
- `.codex/engram-instructions.md`
- `README.md`
- `specs/changes/*`

## Data / Request Flow

User request → Orchestrator decides gated vs bypassed → gated changes create `change-id` → Explorer/Spec/Architect produce proposal/spec/design → Orchestrator writes tasks → implementers execute task IDs → Test/Reviewer validate against artifacts → Orchestrator archives and persists memory.

## Dependency Graph

- Policy update enables prompt and artifact changes
- Artifact templates support skill resources
- README documents the new process after implementation

## Ownership Plan

- Orchestrator: AGENTS policy, archive, tasks, memory
- Spec/Design roles: prompt updates and artifact contracts
- Skills: rich frontmatter and SDD workflow skills
- Docs: README and artifact guide

## Merge Order

1. Policy and prompt changes
2. Artifact structure and templates
3. Skills enrichment and new SDD skills
4. README update

## Serialization Points

- AGENTS.md and Orchestrator prompt must be updated before documenting README
- Memory policy must be updated before final archive guidance

## Interface / Contract Changes

- Prompts now require `change-id`, artifact references, and task IDs for gated changes
- No runtime API changes outside documentation and instructions

## Tradeoffs

- Gains traceability and stronger handoff at the cost of some process overhead
- Soft gating reduces overhead versus mandatory SDD for all work

## Migration / Rollout Concerns

- Existing trivial tasks still work via bypass
- Future gated tasks should use the new artifact folder structure

## Risks

- Artifact drift if Orchestrator does not enforce updates
- Some skills remain lighter than full gentle-ai parity

## Recommendation

Adopt the hybrid model now and expand later only if teams need deeper SDD granularity.
