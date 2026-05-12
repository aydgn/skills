# Deep Review Checklist

Use this checklist only for risky or broad refactors. Keep routine refactors in `SKILL.md` to avoid unnecessary context.

## Scope control

- Is the target boundary explicit?
- Are all changed files necessary for the target?
- Did the refactor avoid unrelated formatting, comment churn, and opportunistic cleanup?
- Are generated files, lockfiles, snapshots, or migrations changed only when expected?

## Behavior preservation

- Are public APIs, props, events, routes, return shapes, thrown errors, and data contracts preserved or intentionally migrated?
- Are edge cases from existing tests, fixtures, stories, or bug comments still covered?
- Are analytics, logging, telemetry, feature flags, permissions, and localization keys preserved?
- Are loading, empty, disabled, error, and retry states still reachable?

## Simplicity and abstraction

- Did the change reduce the number of concepts a reader must hold?
- Is any new helper, hook, class, or component named after a real domain concept?
- Does each abstraction have more than one reason to exist, or one very strong reason?
- Could 100 lines replace 1000 lines without losing clarity?
- Is dead code removed after the new path works?

## Frontend-specific review

- Are React/Vue/Svelte/etc. lifecycle rules preserved?
- Are effect dependencies correct and cleanup functions retained?
- Is state derived instead of duplicated where possible?
- Are memoization and callback wrappers justified?
- Are labels, roles, alt text, focus order, keyboard behavior, and ARIA attributes preserved?
- Are layout shifts, hydration behavior, and server/client boundaries considered where relevant?

## Testing and validation

- Were characterization tests added or updated when behavior was not otherwise pinned down?
- Did focused tests pass?
- Did typecheck, lint, build, or storybook checks run when relevant?
- If a check was not run, is the reason concrete rather than convenience-based?
- Does the final response distinguish verified facts from assumptions?

## Comprehension debt

- Can a human reviewer summarize the new flow quickly?
- Does the final response explain the important change without requiring the reviewer to reverse-engineer the whole diff?
- Are tradeoffs and rejected alternatives explicit when they matter?
- Is any remaining risk tied to a file, behavior, or validation gap?
