---
name: prompt-boost
description: Create or refine task prompts for AI agents. Use when the user asks to improve a prompt or define a complex task; return a ready-to-run Markdown prompt, not an implementation.
---

# Prompt Boost

Produce a prompt the next agent can execute. Do not write implementation code.

1. Identify the objective, scope, context, constraints, deliverables, and **verifiable acceptance criteria**. Research factual gaps with available tools; use subagents for useful independent research when supported.
2. Ask one specific question at a time only when a missing detail would materially change the prompt. Otherwise proceed with explicit assumptions.
3. Return a self-contained Markdown prompt with only the sections the task needs. Order the steps, distinguish requirements from references, and state clear completion criteria.
4. Invite changes or additions.

Done when the prompt makes the task and its success conditions unambiguous without performing it.
