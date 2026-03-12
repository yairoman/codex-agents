# Engram Compact Prompt

Before the conversation context is compacted, persist important knowledge using Engram.

## Compaction policy

- Only the **Orchestrator** should persist the final consolidated summary for the task
- Worker agents should not store partial lane notes during compaction
- If you are a worker agent, prepare a memory candidate for the Orchestrator instead of writing final memory
- For gated SDD changes, compact from the approved `archive.md`, not from draft artifacts

## Important knowledge includes

- architecture decisions
- repository conventions
- completed feature summaries
- bug root causes
- operational risks
- deployment considerations
- archived learnings tied to a `change-id`

Only store concise, consolidated summaries that will be useful in future sessions.
