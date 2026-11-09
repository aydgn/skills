---
name: agentic-refactoring
description: Targeted, behavior-preserving refactoring of a named function, component, module, or documentation section. Use when asked to rewrite, modernize, or de-bloat existing code while keeping contracts intact.
---

# Agentic Refactoring

Make the smallest reviewable change that meets the user's goal. Treat contracts, tests, and comprehension as constraints.

## Workflow

1. **Scope:** Locate the target; inspect callers, consumers, types/schemas, tests/stories, and nearby conventions. Limit edits to the target and required dependencies. For ambiguity, use the narrowest reversible assumption; ask before destructive, security-sensitive, or product-visible choices.
2. **Contract:** Identify behavior and interfaces to preserve (APIs, props/events, routes, data and error shapes, side effects, accessibility) alongside the requested improvement. Define checks that demonstrate success.
3. **Baseline:** Inspect or run focused tests, typechecks, snapshots, and examples. When consequential behavior lacks coverage, add characterization tests or establish a reproducible manual check before editing.
4. **Refactor:** Prefer deletions, flatter flow, clear names, existing patterns, and local helpers over new frameworks or generic abstractions. Preserve intent comments, remove dead code, update affected callers, and justify dependencies. Keep unrelated formatting and files unchanged.
5. **Validate:** Run focused tests, then relevant typecheck, lint, or build checks for shared code or changed contracts. Review the diff for behavior drift and unrelated churn; resolve failures or identify a concrete blocker.
6. **Report:** Summarize changes, preserved contracts, validation results, and meaningful assumptions or tradeoffs. Separate verified outcomes from untested claims.

## Situational checks

- **Frontend:** Preserve controlled state, effect dependencies and cleanup, keyboard/focus/ARIA behavior, analytics, routing, loading and error states, and server/client boundaries. Prefer derived values to duplicated state.
- **Performance:** Establish correctness first; measure or explain the evidence before claiming gains. Avoid speculative memoization.
- **Documentation:** Preserve supported facts and intent while removing repetition and improving navigation.
- **Risky scope:** For cross-file, public API, security, state-management, accessibility, or performance-sensitive work, read [the deep review checklist](references/deep-review-checklist.md) before editing and again before reporting.
