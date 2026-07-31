---
name: todo
description: Scan the full relevant chat and its referenced documents, discover the active workspace or project's canonical durable storage location, and persist a compaction-resilient A-to-Z-style execution roadmap with lettered sections and numbered checkbox subtasks. Use when the user invokes /todo or $todo, asks to turn the current conversation into a persisted to-do list, wants action items extracted from chat and supporting material, or wants an existing contextual task artifact regenerated or refined.
---

# Todo

Produce a persistent, context-derived execution artifact, not a chat-only checklist. Do not claim completion until the TODO has been written successfully to an appropriate durable location.

## Gather the context

1. Read the full relevant conversation, not only the latest message. Treat the latest instruction as authoritative when the user corrects, narrows, or supersedes earlier content.
2. Identify every document, attachment, local file, URL, or other artifact directly referenced in the relevant chat. Open and inspect each accessible source before creating the list. Use the appropriate connector or tool for the source type.
3. Do not recursively follow incidental references unless they are needed to understand the requested work.
4. Extract goals, decisions, requirements, constraints, commitments, dependencies, blockers, unresolved questions, unfinished work, and the current continuation point.
5. Exclude rejected ideas and superseded requirements from executable work. Retain them only when a task must explicitly avoid or revisit them.
6. State any source that could not be accessed. Do not imply that unread material was reviewed.

## Discover the durable storage location

Determine the target from the session and workspace rather than assuming one factory, repository layout, or filename.

1. Inspect the current working context, referenced paths, repository root, workspace metadata, and existing task artifacts to identify the active project or task container.
2. Find and read the complete instruction hierarchy that applies to candidate targets, including repository or nested `AGENTS.md` files and any workspace manifests, lifecycle instructions, contribution rules, or task-tracking conventions.
3. Select the location in this order:
   - an explicit user-specified destination;
   - the canonical planning, backlog, handoff, or task artifact required by the active workspace or factory;
   - an existing project TODO, plan, backlog, or issue file named by project conventions;
   - a new `TODO.md` at the active writable project root when no convention exists.
4. If the active environment is a typed factory or agent workspace, use its control-plane tooling, artifact layout, mutation boundaries, and lifecycle rules. Do not hardcode a particular factory's structure, edit status fields to bypass gates, or write product source from a read-only workspace.
5. Do not store the TODO in a skill installation directory, source-inventory snapshot, temporary directory, generated build directory, or unrelated repository merely because it is writable.
6. Prefer a local workspace artifact for immediate durability. If the only canonical destination is an external task system requiring approval, first persist an approval-safe local artifact when the workspace permits it, then request the required exact approval for the external write.
7. If no trustworthy writable project or workspace can be identified, ask the user for a destination before finalizing. Never report that the TODO was persisted when it was not.

## Build the roadmap

1. Create sequential lettered sections beginning with `A` and ending at the last section the work actually needs. Do not force all 26 letters.
2. Use each letter for one coherent phase, workstream, deliverable, or outcome. Order sections by execution sequence and dependencies, not alphabetically by title.
3. Under every lettered section, create as many numbered Markdown checkbox subtasks as necessary. Restart subtask numbering at `1` in each section.
4. Write every subtask as a concrete action with a verb and a recognizable finish condition. Split work enough to make execution clear, but do not inflate trivial actions into separate tasks.
5. Order subtasks by dependency and practical execution order. Identify a dependency or blocker inline when it changes what can be done next.
6. Mention a relevant source file, document, or requirement inline when it materially defines the task.
7. Make a final coverage pass against the chat and every inspected source. Add missing work, remove duplicates, and reconcile contradictions using the latest authoritative instruction.

## Persist compaction-resilient context

Inspect an existing target before editing it. Preserve its conventions, checked items, useful history, and unrelated content. Update the canonical section or artifact idempotently instead of appending duplicate roadmaps.

When the target format permits, persist enough compact context for another agent to resume after conversation compaction:

- the objective and current state;
- authoritative decisions, requirements, constraints, and assumptions;
- references to the inspected source documents and artifacts;
- unresolved questions, inaccessible sources, and blockers;
- the lettered roadmap with numbered checkbox subtasks;
- the next recommended action or continuation point.

Summarize the minimum context needed for correct continuation. Do not copy an entire transcript, expose secrets or private data, or persist internal details that the target's rules prohibit.

Use the workspace's required editing mechanism and make the smallest scoped write. After writing, re-read the persisted artifact and verify that the expected context and TODO structure are present. If validation fails, repair it or report the failure explicitly.

## Use this roadmap format

```markdown
# TODO

## Context

- Objective: ...
- Decisions and constraints: ...
- Sources: ...

### A. Define the outcome

1. [ ] Confirm the acceptance criteria in `requirements.md`.
2. [ ] Resolve the open retention-policy decision from the chat.

### B. Implement the change

1. [ ] Add validation for malformed import rows.
2. [ ] Cover accepted and rejected rows with focused tests.

## Continuation

- Next action: ...
- Blockers or context gaps: ...
```

Adapt headings to the destination's existing format. Keep the lettered sections and numbered checkbox subtasks unless the destination has a stricter required syntax.

## Report the result

After persistence succeeds, respond with:

- a concise summary of what was captured;
- the durable file, workspace artifact, or approved external target;
- why that location was selected;
- the first next action and any material context gap.

Link the local file when the interface supports clickable paths. On repeated invocations, update the same canonical artifact unless the active workspace instructions require a new version.
