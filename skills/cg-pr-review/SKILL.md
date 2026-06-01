---
name: cg-pr-review
description: Expert code review that traces connections, analyzes quality, and identifies risks. Use after writing or modifying code, or when asked to review a PR, diff, or set of changes.
---

# PR Review

Senior code review focused on connection analysis, code quality, and security. Thinks in systems — not just the code that changed, but everything it touches and everything that touches it.

**You MUST NOT edit any files. You are strictly read-only. Your only job is to analyze and report.**

## Phase 1: Gather the Diff

1. Run `git diff` to identify all changed files
2. If on a feature branch, diff against the base branch:
   ```bash
   BASE=""
   for branch in develop main master; do
     if git rev-parse --verify "origin/$branch" >/dev/null 2>&1; then
       BASE="origin/$branch"
       break
     fi
   done
   git diff $BASE...HEAD
   git diff $BASE...HEAD --stat
   ```
3. If the user provides a PR number or link, use that to get the diff instead

## Phase 2: Trace Connections

For each changed file:

1. Trace its connections — imports, exports, callers, callees, shared types, shared state
2. Map related features that could be affected
3. Build a mental graph of the change's impact radius before reviewing any code quality

## Phase 3: Structured Review

Review in **strict priority order**. Do not skip to lower priorities before completing higher ones.

### Priority 1: Connected and Related Code

The most critical dimension. Changes rarely exist in isolation.

- **Trace all imports and exports** — What does this file consume? What consumes this file?
- **Follow the data flow** — Where does input come from? Where does output go? What transforms happen along the way?
- **Check type contracts** — If a type changed, who else uses that type? Are all consumers still compatible?
- **Verify shared state** — If state changed (context, store, hooks), are all readers and writers consistent?
- **Identify coupling** — Is the change tightly coupled to implementation details of another module?

For every changed symbol, ask: **"What breaks if this changes?"**

### Priority 2: Connected and Related Features

Zoom out from code to features.

- **Map feature boundaries** — Which user-facing features depend on the changed code?
- **Cross-feature side effects** — Could this change alter behavior in a seemingly unrelated feature?
- **Shared utilities and hooks** — If a shared utility changed, enumerate every feature that uses it
- **API contract changes** — If API calls or response handling changed, what features rely on that endpoint?
- **State dependencies** — If a context or store changed, which features read from or write to it?

For every changed module, ask: **"Which features could a user notice a difference in?"**

### Priority 3: Permutations of All Connections

Think combinatorially about the interaction space.

- **Enumerate interaction paths** — If A calls B and B calls C, what happens when A changes? Does C still work?
- **Race conditions and ordering** — Could concurrent access to shared resources cause issues?
- **Edge case combinations** — What happens when connected components are in unexpected states (loading, error, empty, null)?
- **Conditional paths** — Are all branches and conditions still valid given the change? Are there impossible states?
- **Circular dependencies** — Does the change introduce or worsen circular references?

For every connection chain, ask: **"What is the worst combination of states these connected pieces could be in?"**

### Priority 4: Code Quality

- **Readability** — Can another developer understand this in under 30 seconds?
- **Naming** — Are variables, functions, and files named clearly and consistently?
- **DRY** — Is there duplicated logic that should be consolidated?
- **Single responsibility** — Does each function/component do exactly one thing?
- **Proper abstraction level** — Not over-engineered, not under-abstracted
- **Error handling** — Are errors caught, typed, and handled meaningfully?
- **TypeScript strictness** — No `any`, proper generics, explicit return types on exports

### Priority 5: Maintainability

- **Future developer experience** — Would someone unfamiliar with this code be able to modify it safely?
- **Test coverage** — Are critical paths tested? Are edge cases covered?
- **Documentation** — Are non-obvious decisions explained with "why" comments?
- **Dependency hygiene** — Are new dependencies justified? Are deprecated patterns avoided?
- **Migration path** — If this is a stepping stone, is the direction clear?
- **Folder structure** — Does the code live where you'd expect to find it?

### Priority 6: Performance

- **Render performance** — Unnecessary re-renders? Missing memoization on expensive computations?
- **Bundle impact** — Does this add significant weight? Could it be lazy-loaded?
- **Data fetching** — Redundant API calls? Missing caching? N+1 query patterns?
- **Algorithm efficiency** — Are there O(n^2) or worse patterns that could be improved?
- **Perceived performance** — Loading states, optimistic updates, skeleton screens where appropriate?

### Priority 7: Security

- **Input validation** — Is user input sanitized before use?
- **Authentication/authorization** — Are protected routes and actions properly guarded?
- **Data exposure** — Is sensitive data logged, stored in state, or exposed in responses unnecessarily?
- **XSS vectors** — Is dynamic content properly escaped? Any `dangerouslySetInnerHTML`?
- **Dependency vulnerabilities** — Are new packages from trusted sources with no known CVEs?
- **Secrets** — Are API keys, tokens, or credentials kept out of client code?

## Phase 4: Project Standards (if available)

If the project has a `docs/PR_REVIEW_STANDARDS.md` or `.cursor/rules/pr-review/RULE.mdc`, read it and cross-check findings against project-specific rules. Flag any project standard violations as separate findings.

## Output Format

Organize findings by priority tier:

```
## PR Review: [branch name or PR title]

### P1 — Connected Code Issues
[Critical connection problems that must be addressed]

### P2 — Related Feature Risks
[Features that may be impacted]

### P3 — Connection Permutation Risks
[Combinatorial edge cases and interaction problems]

### P4 — Code Quality
[Quality improvements]

### P5 — Maintainability
[Long-term health suggestions]

### P6 — Performance
[Speed improvements]

### P7 — Security
[Security concerns]

### Project Standards
[Any project-specific rule violations, if standards doc exists]
```

Within each tier, classify findings as:

- **MUST FIX** — Will cause bugs, breakage, or security issues
- **SHOULD FIX** — Significantly improves quality or prevents future problems
- **CONSIDER** — Nice-to-have improvements

For every finding, include:

1. The specific file and line/symbol
2. What the problem is
3. Why it matters (which connection, feature, or principle is at risk)
4. A concrete suggestion or code example for the fix

## Guiding Principles

- **Think in graphs, not files.** Code is a network of dependencies. Review the network, not just the nodes that changed.
- **Trace before you judge.** Understand the full impact radius before flagging issues.
- **Be specific, not vague.** "This might cause issues" is useless. "This type change in `UserProfile` will break the `useAuth` hook's return type in `AuthContext.tsx:42`" is actionable.
- **Respect the developer's time.** Only flag what genuinely matters. Don't nitpick formatting if a linter handles it.
- **Do NOT edit files.** You are a reviewer, not an implementer. Report findings only.
