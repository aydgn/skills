---
name: agentic-refactoring
description: refactors, rewrites, simplifies, modernizes, or de-bloats existing code, functions, components, modules, or documentation sections using an agentic coding workflow. use when the user points at specific code and asks to improve it while preserving behavior, reduce complexity, optimize with correctness, update tests, avoid unrelated side effects, surface assumptions and tradeoffs, and validate changes with lint, typecheck, build, or tests.
---

# Agentic Refactoring

## Operating stance

Treat refactoring as a large, reviewable code action: use the agent's stamina to loop until the success criteria are met, but act like a careful senior reviewer is watching the diff.

Default to declarative leverage: convert the user's request into success criteria, then choose the smallest implementation that satisfies them. Do not blindly execute a vague rewrite.

Watch for the common agent failure modes this skill is designed to prevent: wrong assumptions, hidden confusion, unnecessary abstractions, bloated APIs, unrelated edits, dead-code leftovers, silent behavior changes, and comprehension debt.

## Standard workflow

Use this workflow for every targeted refactor or rewrite.

Progress:
- [ ] define the target and scope
- [ ] establish success criteria
- [ ] inspect current behavior and tests
- [ ] choose the simplest safe strategy
- [ ] implement targeted changes
- [ ] validate and iterate
- [ ] report the diff in human-reviewable terms

### 1. Define the target and scope

Identify the exact section, component, function, module, route, hook, class, or file the user pointed to.

Read enough surrounding context to avoid local correctness with global breakage:
- direct callers and consumers
- tests and stories
- type definitions, schemas, props, events, and return values
- related documentation or comments
- project conventions already present nearby

Set a scope boundary before editing. Keep the refactor inside that boundary unless a dependency forces a small supporting change.

If the target is ambiguous, do not stop by default. State the safest assumption, work on the smallest plausible target, and flag the assumption. Ask a clarifying question only when the ambiguity could cause destructive, security-sensitive, or product-visible behavior changes.

### 2. Establish success criteria

Translate the request into concrete criteria before changing code.

Good criteria include:
- behavior to preserve
- behavior to change, if any
- public API, prop, event, route, return-shape, error, or data-contract constraints
- style or architecture goals such as simpler flow, fewer branches, less duplication, clearer names, or smaller component surface
- validation commands to run, such as focused tests, typecheck, lint, build, or storybook checks

Prefer success criteria over imperative micromanagement. Example: "preserve rendered output and analytics events while reducing duplicated conditional rendering" is better than "move these lines into a helper".

For high-risk or broad changes, present a lightweight plan before editing. Keep the plan short enough to be useful, not performative.

### 3. Inspect current behavior and tests

Before rewriting, understand what the existing code actually guarantees.

Run or inspect the most focused validation available:
- existing unit/integration/component tests
- typecheck or static checks
- snapshots, stories, fixtures, or examples
- current error states, edge cases, and accessibility behavior

When tests are missing and behavior matters, create characterization tests or concrete examples before changing the implementation. If adding tests is not feasible, document the manual verification path you used.

Do not optimize or redesign until correctness is anchored.

### 4. Choose the simplest safe strategy

Bias toward the smallest readable change that removes the actual pain.

Prefer:
- deleting code over adding code
- flattening control flow over introducing frameworks
- explicit names over clever generic abstractions
- local helpers over exported utilities until reuse is real
- pure functions and narrow interfaces where they reduce coupling
- existing project patterns over imported novelty
- a naive obviously-correct implementation before a clever optimized one

Avoid equal-weight menus. Pick a default strategy and mention alternatives only when a meaningful tradeoff exists.

If optimization is requested, preserve correctness first. Establish a simple correct version, then optimize while keeping tests and external behavior stable.

### 5. Implement targeted changes

Edit as though every line must survive human review.

Rules:
- Preserve public contracts unless the user explicitly asked to change them.
- Do not modify unrelated comments, formatting, exports, tests, or files as a side effect.
- Keep comments that explain intent, invariants, business rules, or non-obvious browser/runtime behavior.
- Remove dead code created by the refactor.
- Update all affected call sites when an interface changes.
- Keep naming aligned with the surrounding codebase.
- Do not introduce new dependencies without a strong reason and an explicit note.
- Do not hide risky changes behind broad rewrites.

For frontend components, also verify:
- props remain intentional and minimal
- state is not duplicated across sources of truth
- effects have clear dependencies and cleanup
- rendering stays accessible: labels, roles, keyboard flow, focus, alt text, and disabled/loading/error states
- event handlers, analytics, routing, and data-fetch lifecycles are preserved
- memoization is used only when it solves a measured or plausible re-render problem

### 6. Validate and iterate

After edits, run the tightest validation loop available.

Default loop:
1. Run focused tests for the changed area.
2. Run typecheck or lint if the project has it and the change touches typed or linted code.
3. Run build or broader tests when the change affects exports, routing, shared components, data contracts, or configuration.
4. Inspect the diff for unrelated edits, bloat, deleted intent comments, and behavior drift.
5. Fix failures and repeat until the relevant checks pass or there is a clear external blocker.

If validation fails, use the failure as feedback. Do not hand-wave it away. If a failure appears unrelated, verify that claim and explain why.

### 7. Report the result

Final responses should make review easy. Use this structure unless the user asked for another format:

```markdown
## changed
- [short bullets naming the core refactor]

## preserved
- [behavior, API, accessibility, tests, or contracts kept intact]

## validation
- `[command]` - passed
- `[command]` - failed because [reason]
- not run: [reason]

## assumptions / tradeoffs
- [only include meaningful assumptions, risks, or alternatives]
```

For a very small refactor, compress the structure into a short paragraph while still mentioning validation.

## Assumption and confusion management

Surface uncertainty early. Do not silently run with fragile assumptions.

Use this ladder:
1. **safe assumption**: proceed, state it briefly, and keep the change reversible.
2. **meaningful tradeoff**: present the chosen default and the rejected alternative.
3. **blocking ambiguity**: ask one direct question or stop before destructive work.
4. **bad request**: push back when the requested rewrite would reduce security, accessibility, correctness, maintainability, or clarity.

Do not be sycophantic. It is acceptable to say that a requested abstraction is premature, a rewrite is too broad, or the existing code is already close to the simpler solution.

## Refactor heuristics

### De-bloating

Use these moves before inventing architecture:
- inline single-use helpers that obscure intent
- collapse boolean flag combinations into explicit states when state space matters
- replace duplicated branches with data-driven rendering only if it stays readable
- remove pass-through props and wrapper components that add no semantic value
- delete stale code, unused exports, unreachable branches, and obsolete comments
- reduce configuration surfaces that have only one real caller

### API and abstraction design

Introduce or keep an abstraction only when it has a real job:
- it names a domain concept
- it isolates a volatile dependency
- it removes repeated, error-prone logic
- it makes tests simpler and more meaningful
- it narrows an interface rather than expanding it

Avoid abstractions that merely move complexity, create generic option bags, or require readers to jump through more files for the same idea.

### Function rewrites

For functions, optimize for a reader who is reviewing behavior:
- make inputs and outputs obvious
- keep side effects explicit
- prefer early returns when they reduce nesting
- name intermediate values by domain meaning
- separate parsing, validation, transformation, and I/O when they are tangled
- preserve thrown errors, return sentinels, logging, and telemetry unless intentionally changed

### Component rewrites

For UI components:
- separate data preparation from rendering when it reduces JSX noise
- keep conditional rendering legible
- avoid derived state when values can be computed from props or source state
- preserve controlled/uncontrolled behavior
- keep accessibility behavior as part of the contract
- prefer composition when it simplifies call sites; avoid composition that spreads responsibility across too many files

### Documentation or text section rewrites

When rewriting a documentation section instead of code:
- preserve the original intent and factual claims unless asked to correct them
- remove repetition and vague advice
- make the structure scannable
- use concrete examples where they reduce ambiguity
- do not invent unsupported product behavior, APIs, or metrics

## Gotchas

- Do not treat "rewrite" as permission to redesign the whole subsystem.
- Do not change formatting across a file unless formatting is the task or an automated formatter did it as part of validation.
- Do not delete comments just because they look verbose; distinguish intent comments from noise comments.
- Do not add tests that only mirror implementation details.
- Do not leave both old and new paths active unless the transition is intentional.
- Do not introduce a shared utility from a single use case unless the duplication risk is immediate and concrete.
- Do not claim a performance win without evidence or a clear reasoning chain.
- Do not skip validation because the change "looks simple" when validation is available.

## Use the deep checklist for risky refactors

For large, cross-file, public API, state-management, accessibility-sensitive, security-sensitive, or performance-sensitive refactors, read `references/deep-review-checklist.md` before implementation and again before final response.
