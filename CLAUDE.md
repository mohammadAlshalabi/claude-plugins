# claude-plugins

A Claude Code plugin marketplace for sharing team skills/agents. Public repo:
`github.com/mohammadAlshalabi/claude-plugins`.

## Structure

- `.claude-plugin/marketplace.json` — root manifest listing every plugin in
  this repo and its subdirectory (`source`).
- Each plugin is its own top-level folder with:
  - `.claude-plugin/plugin.json` — name/version/description
  - `skills/<name>/SKILL.md` — workflow logic, triggered by description match
  - `agents/*.md` — subagent definitions (frontmatter sets `model`, `tools`)
  - `commands/*.md` — slash commands (frontmatter `description`, body uses
    `$ARGUMENTS`)

## Adding a new plugin

1. Create `<plugin-name>/.claude-plugin/plugin.json`.
2. Add an entry to the root `.claude-plugin/marketplace.json` `plugins` array
   pointing `source` at `./<plugin-name>`.
3. Fill in `skills/`, `agents/`, `commands/` as needed.
4. Validate both JSON files parse, commit, push.

## Distribution note

Plugins (commands/hooks/bundled MCP/subagents) only work in **Claude Code**.
Claude Desktop and claude.ai only understand the portable `SKILL.md` format —
no plugin/marketplace concept there. If a skill needs to reach Desktop/web
teammates too, keep its `skills/<name>/` folder self-contained (no dependency
on this repo's commands/agents) so it can be uploaded standalone — ideally by
a workspace admin, org-wide, rather than per-person.

## Plugins in this repo

### model-orchestrator

Opus acts as planner/reviewer; delegates execution to cheaper models
(Sonnet = standard work, Haiku = trivial/mechanical work) via subagents, then
reviews the actual diff/output before accepting.

- `/orchestrate <task>` — entry point
- `skills/orchestrate/SKILL.md` — decompose → tier → dispatch → review logic
- `agents/implementer-sonnet.md`, `agents/implementer-haiku.md` — pinned-model
  execution subagents the skill dispatches to

### test-planner

Drafts a structured test plan grounded in the code + a short interactive Q&A,
then publishes to Google Docs if the Google Drive connector is available, else
falls back to a local Markdown file.

- `/test-plan <feature>` — entry point
- `skills/test-plan/SKILL.md` — gather (code inspection + user gaps) → draft →
  publish (detect `*Google_Drive__create_file`, else local `.md` + import steps)
- `skills/test-plan/references/template.md` — section structure (IEEE 829-aligned,
  Agile-weighted). Keep self-contained so the skill stays portable to Desktop/web.
