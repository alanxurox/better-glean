# better-glean

**Agent Skill that teaches AI coding agents to use Glean MCP precisely.**

Your Glean MCP server has 12 search parameters. Most AI agents use one: `query`. This means every internal search returns 16 generic results when it could return exactly what you need.

## Before & After

```python
# Before: generic search
search(query="deployment automation")
# → 16 mixed results from Slack, Confluence, Jira, GitHub...

# After: targeted Slack search
search(query="migration", app="slack", updated="past_week")
# → This week's Slack discussions about the migration

# After: everything you authored recently
search(query="*", owner="me", sort_by_recency=true)
# → Your recent documents across all sources
```

## Install

### Claude Code — Plugin Marketplace (recommended)

```bash
claude plugin marketplace add https://github.com/alanxurox/better-glean.git
claude plugin install better-glean@better-glean
```

Or one-line clone + auto-discover:

```bash
git clone https://github.com/alanxurox/better-glean.git ~/.claude/skills/glean-search
```

Claude Code auto-discovers skills in `~/.claude/skills/`.

### OpenSkills (any agent)

```bash
npx openskills install https://github.com/alanxurox/better-glean.git
```

### GenDigital internal

```bash
git clone https://git.int.avast.com/Alan-Xu/better-glean.git ~/.claude/skills/glean-search
```

### Cursor

```bash
git clone https://github.com/alanxurox/better-glean.git ~/.cursor/skills/glean-search
```

Reference in `.cursorrules` or load via Read tool when needed.

### Manual (any agent)

The skill is a single markdown file (`SKILL.md`). Copy it anywhere your agent can read it. No dependencies, no build step.

### Add to CLAUDE.md (optional)

```markdown
## Glean Search
When searching internal docs, load `~/.claude/skills/glean-search/SKILL.md` for precise query patterns.
```

## What It Covers

| Pattern | Key Parameters |
|---------|---------------|
| Someone's recent work | `from`, `sort_by_recency` |
| Slack discussions | `app="slack"`, `channel` |
| Slack DMs | `app="slack"`, `type="direct message"` |
| Jira tickets | `app="jira"` |
| Confluence docs | `app="confluence"`, `owner` |
| PRs / code reviews | `app="githubenterprise"`, `type="pull"` |
| Recent changes | `updated="past_week"` |
| Date range | `after`, `before` |
| Presentations | `type="slides"` |
| Team's work | `from="myteam"` |
| Exhaustive list | `exhaustive=true` |
| Faceted refinement | `dynamic_search_result_filters` |

Plus: 40+ app sources, multi-angle search strategy, anti-patterns, and agent delegation rules.

## Why This Exists

Glean MCP is powerful but under-documented for AI agent use. The [MCP server instructions](https://docs.glean.com/) tell agents the parameters exist but don't teach *when* to use each one. Agents default to simple `query` strings and miss:

- **Source filtering** — searching all of Slack vs. all of Confluence vs. all sources
- **Person scoping** — finding what a specific person wrote or touched
- **Time filtering** — recent vs. historical, date ranges
- **Type filtering** — PRs vs. presentations vs. emails vs. DMs
- **Faceted refinement** — using discovered filters to narrow results
- **Multi-angle strategy** — searching 3 ways for thorough coverage

This skill closes that gap.

## Format

This skill follows the [Agent Skills](https://agentskills.io) open standard — the portable format for extending AI agents with specialized knowledge.

```
better-glean/
├── SKILL.md              # Agent skill (the instructions)
├── README.md             # Human documentation
└── .agentskills.json     # Skill metadata & compatibility
```

**Compatible with:** Claude Code, Cursor, Windsurf, Cline, and any agent that supports the Agent Skills format.

## Related Skills

| Skill | What It Does | Repo |
|-------|-------------|------|
| **content-dev-skills** | IPM style guides as Agent Skills for content development | [git.int.avast.com/ipm/content-dev-skills](https://git.int.avast.com/ipm/content-dev-skills) |

Building more skills? The Agent Skills spec is at [agentskills.io/specification](https://agentskills.io/specification).

## Requirements

- Glean MCP server configured and authenticated
- An AI coding agent (Claude Code, Cursor, Windsurf, etc.)
- That's it. Single file, no dependencies.

## Contributing

Found a query pattern that works well? Open a PR. The goal is a community-maintained reference of precise Glean query patterns.

## License

MIT
