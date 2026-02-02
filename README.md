# better-glean

**Agent Skill that teaches AI coding agents to use Glean MCP precisely.**

Your Glean MCP server has 15+ search parameters. Most AI agents use one: `query`. This means every internal search returns 16 generic results when it could return exactly what you need.

## Before & After

```python
# Before: generic search
search(query="deployment automation")
# → 16 mixed results from Slack, Confluence, Jira, GitHub...

# After: precise query
search(query="deployment automation", app="githubenterprise", type="pull", from="me", sort_by_recency=true)
# → Your recent PRs about deployment automation
```

```python
# Before: hoping Glean figures it out
search(query="what did the team discuss about the migration")
# → Stale Confluence pages from 2024

# After: targeted Slack search
search(query="migration", app="slack", channel="team-engineering", updated="past_week")
# → This week's Slack thread about the migration
```

## Install

### Claude Code (Agent Skills)

```bash
# Copy to your skills directory
cp -r better-glean/ ~/.claude/skills/glean-search/
```

Or add to your `CLAUDE.md`:
```markdown
## Glean Search
When searching internal docs, load `~/.claude/skills/glean-search/SKILL.md` for precise query patterns.
```

### Cursor

```bash
cp -r better-glean/ ~/.cursor/skills/glean-search/
```

Reference in `.cursorrules` or load via Read tool when needed.

### Any AI Coding Agent

The skill is a single markdown file (`SKILL.md`). Feed it to any agent that has Glean MCP access. It works by teaching the agent what parameters exist and when to use them.

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

Plus: 16 app sources, multi-angle search strategy, anti-patterns, and agent delegation rules.

## Why This Exists

Glean MCP is powerful but under-documented for AI agent use. The [MCP server instructions](https://docs.glean.com/) tell agents the parameters exist but don't teach *when* to use each one. Agents default to simple `query` strings and miss:

- **Source filtering** — searching all of Slack vs. all of Confluence vs. all sources
- **Person scoping** — finding what a specific person wrote or touched
- **Time filtering** — recent vs. historical, date ranges
- **Type filtering** — PRs vs. presentations vs. emails vs. DMs
- **Faceted refinement** — using discovered filters to narrow results
- **Multi-angle strategy** — searching 3 ways for thorough coverage

This skill closes that gap.

## Requirements

- Glean MCP server configured and authenticated
- An AI coding agent (Claude Code, Cursor, Windsurf, etc.)
- That's it. Single file, no dependencies.

## Contributing

Found a query pattern that works well? Open a PR. The goal is a community-maintained reference of precise Glean query patterns.

## License

MIT
