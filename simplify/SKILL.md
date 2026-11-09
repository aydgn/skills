---
name: simplify
description: Clean up current or recent code changes through reuse, quality, and efficiency passes. Use when asked to simplify, polish, deduplicate, or improve a diff; fix worthwhile findings rather than only reporting them.
---

# Simplify

Review all changes in scope, then make the smallest behavior-preserving cleanup.

## Scope

- If staged changes exist, use `git diff HEAD` to review staged and unstaged work; otherwise use `git diff`.
- If the working tree is clean, review the most recently modified files named by the user or edited earlier in this conversation.
- Apply requested focus across the full scope unless the user explicitly narrows it.

## Three passes

Use parallel subagents with the full diff or file context when supported; otherwise perform each pass yourself. Complete all three passes before fixing findings.

1. **Reuse:** Find existing utilities, helpers, shared modules, and UI primitives that can replace duplicate functions, copied logic, or newly introduced abstractions.
2. **Quality:** Check duplicated or derived state, unnecessary effects, parameter sprawl, near-duplicate branches, leaky or stringly typed interfaces, pointless UI wrappers, and comments that narrate obvious code. Retain intent and invariant comments.
3. **Efficiency:** Check repeated computation and I/O, N+1 operations, missed concurrency, blocking work in hot paths, no-op state updates, redundant existence checks, memory or listener leaks, and overly broad reads.

## Finish

- Aggregate all findings; fix worthwhile issues directly. Skip false positives and speculative rewrites.
- Preserve contracts and behavior; favor existing patterns and minimal diffs over abstraction churn.
- Re-read the final diff and run relevant tests, typecheck, lint, or build checks. Briefly report changes (or why none were needed) and any validation gaps.
