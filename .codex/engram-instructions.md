# Engram Memory Instructions

Use Engram to persist important project knowledge across sessions.

## Read policy

- Any agent may search or read Engram when prior context is useful
- Search memory before work involving architecture, conventions, module behavior, known bugs, prior decisions, or archived learnings

## Write policy

- Only the **Orchestrator** should persist final memory for a task
- Worker agents must not write final observations directly
- Worker agents should return a **memory candidate** to the Orchestrator instead
- For gated SDD changes, final memory should be aligned to the `change-id`

## Memory candidate contract

When a worker agent identifies something worth saving, it should return:

- suggested type
- suggested title
- what changed
- why it matters
- where it applies
- risk or learning
- `change-id` when one exists

## When the Orchestrator should store final memory

Store memory when:

- a new architecture decision is confirmed
- repository conventions are confirmed
- bug investigations identify root causes
- a session summary may help future work
- a technical risk or limitation is confirmed
- a gated change reaches approved `archive.md`

## Deduplication rules

Before persisting memory, the Orchestrator should deduplicate by:

- suggested type
- normalized title
- primary path or subsystem
- `change-id` when present

If multiple memory candidates describe the same outcome, store only the consolidated version.

## Do NOT store

- temporary debugging notes
- raw logs
- code snippets
- minor implementation details
- exploration noise
- partial lane notes that will be absorbed into the final summary
- drafts of `proposal.md`, `spec.md`, `design.md`, or `tasks.md`

## Required fields for final memory

When saving memory include:

- project name
- scope (backend, frontend, infra, etc)
- summary
- relevant files or components
- known risks
- `change-id` when the change used SDD artifacts
- artifact references when they help future sessions
