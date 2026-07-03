---
name: orchestrate
description: Use when a task is large enough to break into subtasks and you want to control cost by running you (the orchestrator, ideally Opus) as planner and reviewer while delegating the actual execution work to cheaper models (Sonnet/Haiku) via subagents. Triggers on "/orchestrate", "delegate this", "split this across models", or any multi-step build/refactor/migration request where the user cares about model cost.
---

# Model Orchestration

You are acting as the **planner and reviewer**. Do not do the execution work
yourself — decompose it, delegate it to cheaper models, then verify what comes
back. This skill only pays off if you actually push work down; if you end up
doing every subtask's real work yourself, you've defeated the point.

## 0. Sanity check

If you can tell you are not running as Opus (or another top-tier/expensive
model), tell the user this pattern is built to save cost by delegating *down*
from an expensive model — running it from Sonnet/Haiku just adds overhead with
no savings. Ask if they still want to proceed (e.g. they may just want the
structure, not the cost benefit).

## 1. Decompose into a plan

Break the request into an ordered list of discrete, independently-verifiable
subtasks. For each one, assign a tier:

| Tier | Examples | Model |
|---|---|---|
| **Trivial** | boilerplate, mechanical renames, formatting, single-file CRUD, repetitive edits across files, writing straightforward tests for already-defined behavior | `haiku` |
| **Standard** | typical feature implementation, moderate multi-file changes, "normal" bug fixes, refactors with a clear target shape | `sonnet` |
| **Keep for yourself** | ambiguous requirements, architectural decisions, security-sensitive logic, anything where a wrong guess is expensive to unwind | don't delegate — do it yourself or resolve the ambiguity with the user first |

Use `TaskCreate` to track the plan as a task list if it has 3+ steps. Keep
each subtask scoped to something one subagent can finish and that you can
verify independently (a diff, a test run, a file).

## 2. Dispatch to cheaper models

For each Trivial/Standard subtask, use the `Agent` tool with `model` set to
`haiku` or `sonnet` per the tier above (or reference the `implementer-sonnet` /
`implementer-haiku` subagents bundled with this plugin if they fit).

- Brief each dispatch like a colleague with no context: what to build, exact
  file paths, acceptance criteria, and any constraints from the plan. A subagent
  starts cold — it has not seen this conversation.
- Run independent subtasks in parallel (multiple `Agent` calls in one message).
  Run dependent ones sequentially, feeding the prior result forward.
- Prefer background execution for subtasks that don't block the next step;
  use foreground when you need the result before deciding what's next.
- Don't split work more finely than necessary — batching related trivial edits
  into one dispatch is cheaper and more coherent than many tiny ones.

## 3. Review before accepting — this is the job

An agent's summary describes what it *intended* to do, not necessarily what it
did. For every subtask that comes back:

- Read the actual diff or output yourself. Don't accept a summary at face value.
- Check it against the plan's acceptance criteria, not just "did it run."
- Run the real verification available (tests, build, lint, type-check) rather
  than trusting the subagent's own claim that it passed.
- If something's wrong: fix it directly yourself if it's small, or re-dispatch
  with a precise correction (what was wrong, what "correct" looks like) —
  don't just repeat the same prompt.

## 4. Report back

Summarize to the user: what was delegated to which model, what you changed or
fixed yourself, and confirm the review actually passed (tests/build state).
Be honest about anything you couldn't verify (e.g. no way to run the app) —
say so rather than implying it's fully confirmed.
