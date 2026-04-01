---
name: simplify
description: Review changed code and clean it up. Use when asked to simplify, clean up, refactor, polish, de-duplicate, or review recent code changes for reuse, code quality, maintainability, or efficiency. Inspect the current diff, run reuse/quality/efficiency review passes, and fix worthwhile issues directly.
---

# Simplify

Review the current code changes for reuse, quality, and efficiency. Fix worthwhile issues directly.

## Scope the review

First determine what changed:

1. If there are staged changes, review the full working change with `git diff HEAD`.
2. Otherwise review unstaged changes with `git diff`.
3. If there are no git changes, review the most recently modified files that:
   - the user mentioned, or
   - you edited earlier in this conversation.

Use the resulting diff or file set as the review scope.

If the user gave an extra focus area, apply it in every pass without narrowing the base review scope unless they explicitly asked for a narrower review.

## Run three review passes

Prefer using subagents if the environment supports them. Launch all three in parallel and give each one the full diff or full file context.

If no subagent tool is available, perform the same three passes yourself, one after another.

### Pass 1: Reuse review

For each change:

1. Search for existing helpers, utilities, shared modules, and nearby abstractions that could replace newly written code.
2. Flag new functions that duplicate existing behavior.
3. Flag inline logic that should use an existing utility.

Common candidates:
- string manipulation
- path handling
- environment checks
- type guards
- parsing / formatting helpers
- repeated mapping / filtering logic
- shared UI primitives or wrappers

Prefer reusing an existing abstraction over adding a second version of the same idea.

### Pass 2: Code quality review

Look for:

1. Redundant state
   - duplicated state
   - cached values that should be derived
   - effects/observers that should be direct calls

2. Parameter sprawl
   - new parameters added where restructuring or a shared options object would be cleaner
   - functions becoming harder to understand because of branching arguments

3. Copy-paste with slight variation
   - near-duplicate blocks that should become one abstraction

4. Leaky abstractions
   - exposing internal details across module boundaries
   - bypassing an existing abstraction instead of extending it cleanly

5. Stringly-typed code
   - raw strings where constants, enums, unions, or existing types should be used

6. Unnecessary UI nesting
   - wrapper elements/components that add no layout or semantic value
   - nesting that could be replaced by existing props or composition

7. Unnecessary comments
   - remove comments that only narrate what the code already says
   - remove comments about the task, caller, or obvious implementation steps
   - keep only non-obvious why/constraint/invariant comments

### Pass 3: Efficiency review

Look for:

1. Unnecessary work
   - repeated computation
   - duplicate reads
   - repeated API/network/file operations
   - N+1 patterns

2. Missed concurrency
   - independent work done sequentially that could run in parallel

3. Hot-path bloat
   - new blocking work in startup, render, request, or polling paths

4. Recurring no-op updates
   - state/store updates fired unconditionally inside loops, intervals, subscriptions, or handlers
   - wrappers that ignore “no change” signals such as same-reference returns

5. Unnecessary existence checks
   - pre-checking resource existence before acting on it when direct operation plus error handling is cleaner

6. Memory and lifecycle issues
   - missing cleanup
   - listener leaks
   - unbounded caches/collections

7. Overly broad operations
   - reading/loading whole datasets or files when only part is needed

## Fix issues

After all three passes finish:

1. Aggregate the findings.
2. Fix each worthwhile issue directly.
3. If a finding is a false positive or not worth addressing, skip it without debating it.
4. Preserve behavior unless the cleanup itself is the intended behavior change.
5. Prefer the smallest clear fix that improves the codebase.

## Final check

Before finishing:

1. Re-read the updated code.
2. Confirm the change is simpler, more reusable, and no less correct.
3. Make sure you did not introduce abstraction churn for tiny gains.
4. Ensure naming still matches the surrounding codebase style.

## Output

Briefly summarize:
- what you fixed, or
- that the reviewed code was already clean

If you skipped any findings, mention them briefly without long justification.