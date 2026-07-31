---
name: todo
description: Scan the full relevant chat context and its directly referenced documents, attachments, links, and artifacts, then synthesize a comprehensive A-to-Z-style execution roadmap with lettered sections and numbered checkbox subtasks. Use when the user invokes /todo or $todo, asks to turn the current conversation into a to-do list, wants action items extracted from chat and supporting material, or wants an existing contextual task list regenerated or refined.
---

# Todo

Produce a context-derived execution list, not a generic checklist. Review the conversation and its supporting sources before drafting.

## Gather the context

1. Read the full relevant conversation, not only the latest message. Treat the latest instruction as authoritative when the user corrects, narrows, or supersedes earlier content.
2. Identify every document, attachment, local file, URL, or other artifact directly referenced in the relevant chat. Open and inspect each accessible source before creating the list. Use the appropriate connector or tool for the source type.
3. Do not recursively follow incidental references unless they are needed to understand the requested work.
4. Extract goals, decisions, requirements, constraints, commitments, dependencies, blockers, unresolved questions, and unfinished work.
5. Exclude rejected ideas and superseded requirements from executable work. Retain them only when a task must explicitly avoid or revisit them.
6. State any source that could not be accessed. Do not imply that unread material was reviewed.

## Build the roadmap

1. Create sequential lettered sections beginning with `A` and ending at the last section the work actually needs. Do not force all 26 letters.
2. Use each letter for one coherent phase, workstream, deliverable, or outcome. Order sections by execution sequence and dependencies, not alphabetically by title.
3. Under every lettered section, create as many numbered Markdown checkbox subtasks as necessary. Restart subtask numbering at `1` in each section.
4. Write every subtask as a concrete action with a verb and a recognizable finish condition. Split work enough to make execution clear, but do not inflate trivial actions into separate tasks.
5. Order subtasks by dependency and practical execution order. Identify a dependency or blocker inline when it changes what can be done next.
6. Mention a relevant source file, document, or requirement inline when it materially defines the task.
7. Make a final coverage pass against the chat and every inspected source. Add missing work, remove duplicates, and reconcile contradictions using the latest authoritative instruction.

## Use this format

```markdown
# TODO

### A. Define the outcome

1. [ ] Confirm the acceptance criteria in `requirements.md`.
2. [ ] Resolve the open retention-policy decision from the chat.

### B. Implement the change

1. [ ] Add validation for malformed import rows.
2. [ ] Cover accepted and rejected rows with focused tests.
```

Keep the headings descriptive rather than using letters alone. Add a short `Context gaps` section after the roadmap only when a referenced source was inaccessible or a material ambiguity prevents a reliable task. Do not invent owners, deadlines, estimates, or commitments.

Keep the result in the conversation unless the user explicitly asks to create or update a file or connected task system. For a requested write, inspect the target first, preserve its conventions, and make the smallest scoped change. Obtain any approval required for an external write.
