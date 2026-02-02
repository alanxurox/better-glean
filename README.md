# better-glean

**Skill + Agent for precise Glean MCP searches.**

Your Glean MCP server has 12 search parameters. Most AI agents use one: `query`. This means every internal search returns 16 generic results when it could return exactly what you need.

```
better-glean/
├── SKILL.md                        # Query patterns reference (install this)
├── agents/
│   └── glean-researcher.md         # Autonomous research agent
├── examples/
│   └── queries.md                  # Tested query cookbook
├── .agentskills.json               # Skill metadata & compatibility
├── CHANGELOG.md
├── CONTRIBUTING.md
└── LICENSE
```

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

## Two Ways to Use

### 1. Skill (Reference Patterns)

Install `SKILL.md` and your agent learns 12 query patterns, 40+ app sources, anti-patterns, and multi-angle search strategy. It reads the file when it needs to search Glean.

### 2. Agent (Autonomous Researcher)

Install `agents/glean-researcher.md` and get a subagent that takes a question, plans 2-3 search angles, executes them, follows up on top results with `read_document`, and returns a synthesized answer with sources.

```
You: "What's the current state of the GCP tenant migration?"

Agent:
  → search(query="GCP tenant migration", app="confluence")          → 4 docs
  → search(query="GCP tenant", app="slack", updated="past_week")    → 7 threads
  → read_document(urls=["https://confluence.../GCP+Migration+Plan"])
  → Returns: structured answer + sources + gaps identified
```

## Install

### Skill Only (Claude Code)

```bash
# Plugin Marketplace (recommended)
claude plugin marketplace add https://github.com/alanxurox/better-glean.git
claude plugin install better-glean@better-glean

# Or one-line clone (auto-discovered)
git clone https://github.com/alanxurox/better-glean.git ~/.claude/skills/glean-search
```

### Skill Only (Other Agents)

```bash
# OpenSkills (any agent)
npx openskills install https://github.com/alanxurox/better-glean.git

# Cursor
git clone https://github.com/alanxurox/better-glean.git ~/.cursor/skills/glean-search

# Manual — copy SKILL.md anywhere your agent can read it
```

### Agent (Claude Code)

```bash
# Copy the agent definition to your agents directory
cp agents/glean-researcher.md ~/.claude/agents/

# Then use it via Task tool:
# Task(subagent_type="glean-researcher", prompt="What's the state of the GCP migration?")
```

### GenDigital Internal

```bash
git clone https://git.int.avast.com/Alan-Xu/better-glean.git ~/.claude/skills/glean-search
```

### Add to CLAUDE.md (optional)

```markdown
## Glean Search
When searching internal docs, load `~/.claude/skills/glean-search/SKILL.md` for precise query patterns.
For autonomous research, use the `glean-researcher` agent.
```

## Query Patterns

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

See `examples/queries.md` for a full tested cookbook.

## Key Concepts

**`from` vs `owner`**: `from` = updated by, commented on, or created by (broad). `owner` = created by (narrow). Use `owner="me"` for "my PRs", `from="me"` for "PRs I touched."

**`sort_by_recency`**: Only use when the user's primary intent is "latest" or "most recent." Default relevance ranking is better for topical searches.

**`query="*"` wildcard**: Valid for "all documents matching these filters." Always combine with at least one filter.

**Rate limiting**: Glean enforces rate limits. Batch filters into fewer queries, cache URLs, and prefer `search` over `chat` for lower cost. See the Rate Limiting section in SKILL.md.

## Why This Exists

Glean MCP is powerful but under-documented for AI agent use. The MCP server instructions tell agents the parameters exist but don't teach *when* to use each one. Agents default to simple `query` strings and miss source filtering, person scoping, time filtering, type filtering, faceted refinement, and multi-angle strategy.

This skill closes that gap. The agent takes it further — autonomous research without manual query crafting.

## Format

Follows the [Agent Skills](https://agentskills.io) open standard — the portable format for extending AI agents with specialized knowledge.

**Compatible with:** Claude Code, Cursor, Windsurf, Cline, and any agent that supports Agent Skills or custom agent definitions.

## Contributing

Found a query pattern that works well? Open a PR. See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## License

[MIT](LICENSE)
