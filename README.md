# claude-plugins

A Claude Code plugin marketplace for team skills/agents.

## Install (for teammates)

```
/plugin marketplace add mohammadAlshalabi/claude-plugins
/plugin install model-orchestrator
```

Update later with:

```
/plugin marketplace update
```

## Plugins in this repo

### model-orchestrator

Runs you (ideally Opus) as planner and reviewer while delegating the actual
execution work to cheaper models (Sonnet/Haiku) via subagents, to cut cost on
large multi-step tasks.

- `/orchestrate <task>` — kick off the workflow explicitly
- Skill `orchestrate` — the workflow logic (decompose → tier → dispatch → review)
- Subagents `implementer-sonnet` / `implementer-haiku` — pre-scoped execution
  roles the skill dispatches to
