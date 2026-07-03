# claude-plugins

A Claude Code plugin marketplace for team skills/agents.

## Install (for teammates)

```
/plugin marketplace add mohammadAlshalabi/claude-plugins
/plugin install model-orchestrator@shalabi-plugins
/plugin install test-planner@shalabi-plugins
```

> The GitHub repo is `claude-plugins`, but the marketplace's internal **name is
> `shalabi-plugins`** (Claude Code rejects marketplace names that look like
> official Anthropic ones). That's why plugin IDs use the `@shalabi-plugins`
> suffix.

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

### test-planner

Drafts a structured test plan grounded in the actual code plus a short
interactive Q&A (scope boundaries, risk priorities, audience), then publishes
it to **Google Docs** if the Google Drive connector is enabled — otherwise
writes a local Markdown file with import instructions, so it works on every
surface.

- `/test-plan <feature>` — draft and publish a test plan
- Skill `test-plan` — gather (code + user) → draft → publish/fallback
- `skills/test-plan/references/template.md` — the section structure
  (IEEE 829-aligned, Agile-weighted); self-contained so it's portable to
  Claude Desktop/web too

> Note: native Google Docs creation only works where the Google Drive connector
> is enabled. Without it the skill falls back to a local Markdown file you can
> import into Docs — it's never a dead end.
