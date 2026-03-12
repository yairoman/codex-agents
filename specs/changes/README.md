# SDD Change Artifacts

This directory stores versioned, change-level artifacts for tasks that pass the SDD gate.

## Folder structure

```text
specs/changes/<change-id>/
  proposal.md
  spec.md
  design.md
  tasks.md
  verify.md
  archive.md
```

## When to create a change folder

Create `specs/changes/<change-id>/` when the Orchestrator determines that the task:

- touches more than one subsystem
- requires parallel execution
- changes contracts, interfaces, or database behavior
- has rollout or regression risk
- requires handoff between agents

## Change ID guidance

Use lowercase kebab-case and keep it stable for the full task.

Examples:

- `adopt-sdd-hybrid`
- `add-order-draft-flow`
- `fix-invoice-publish-race`

## Artifact ownership

- Explorer feeds `proposal.md`
- Spec Writer owns `spec.md`
- Architect owns `design.md`
- Orchestrator owns `tasks.md` and `archive.md`
- Test owns `verify.md`
- Reviewer validates artifact consistency before closeout

## Bypass policy

If the Orchestrator bypasses SDD, it must record the reason in the main response.
No change folder is required for trivial doc-only or low-risk single-scope changes.
