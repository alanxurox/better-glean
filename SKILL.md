---
name: glean-search
description: Precise Glean MCP query patterns — stop getting 16 generic results, start getting exactly what you need
triggers:
  - glean
  - internal search
  - find in slack
  - find in jira
  - find in confluence
  - who wrote
  - company docs
---

# Glean Search Patterns

Your Glean MCP has 15+ parameters. Most AI agents use one: `query`. This skill teaches precise queries.

## The Problem

```
# What most agents do:
search(query="deployment automation")
→ 16 generic results across all sources

# What this skill teaches:
search(query="deployment automation", app="githubenterprise", type="pull", from="me", sort_by_recency=true)
→ Your recent PRs about deployment automation
```

## Three Tools

| Tool | When | Token Cost |
|------|------|------------|
| `mcp__glean_default__search` | Find documents, get snippets + URLs | Low |
| `mcp__glean_default__chat` | Ask questions, get synthesized answers | Medium |
| `mcp__glean_default__read_document` | Get full content of known URLs | Variable |

**Workflow:** `search` → find URLs → `read_document` for full content. Use `chat` when you need synthesis across sources.

## Query Patterns

### Find someone's recent work
```
search(query="*", from="Person Name", sort_by_recency=true)
```
`from` = updated by, commented on, or created by. Use `owner` to restrict to docs they created (not just touched).

### Find Slack discussions
```
search(query="content dev skills beta", app="slack")
```
Add `channel="channel-name"` for a specific channel. Add `sort_by_recency=true` for latest.

### Find Slack DMs
```
search(query="handover notes", app="slack", type="direct message")
```

### Find Jira tickets
```
search(query="style guide automation", app="jira")
```
Add `from="me"` for tickets you're assigned to or commented on.

### Find Confluence docs
```
search(query="email automation architecture", app="confluence")
```
Add `owner="Person Name"` for docs a specific person authored.

### Find PRs / code reviews
```
search(query="content-dev-skills", app="githubenterprise", type="pull")
```
Combine with `from="me"` for your PRs, or `from="Person Name"` for theirs.

### Find recent changes
```
search(query="deployment skills", updated="past_week")
```
Options: `today`, `yesterday`, `past_week`, `past_2_weeks`, `past_month`, or month names like `"January"`.

### Find docs in a date range
```
search(query="AI SDLC", after="2026-01-01", before="2026-02-01")
```

### Find presentations
```
search(query="quarterly review", type="slides")
```
Types: `pull`, `spreadsheet`, `slides`, `email`, `direct message`, `folder`.

### Find your team's work
```
search(query="*", from="myteam", sort_by_recency=true)
```
Only `"me"` and `"myteam"` work as special values. Don't use other team names.

### Exhaustive search
```
search(query="GCP tenant", exhaustive=true)
```
Use when you need "all", "every", or "each" result. Not just "more results."

### Refine with discovered filters
After a first search, check each result's `matching_filters`. Rerun with:
```
search(query="original query", dynamic_search_result_filters="brand:Norton|product:VPN")
```
Format: `key:value` pairs joined by `|`. Only use keys/values you saw in actual results.

## App Filter Reference

| Value | Source |
|-------|--------|
| `slack` | Slack messages & threads |
| `confluence` | Confluence pages |
| `jira` | Jira tickets |
| `githubenterprise` | GitHub repos, PRs, issues |
| `o365` | Office 365 docs |
| `o365sharepoint` | SharePoint sites |
| `o365onedrive` | OneDrive files |
| `outlook` | Outlook emails |
| `microsoftteams` | Teams messages |
| `zoom` | Zoom recordings/transcripts |
| `notion` | Notion pages |
| `trello` | Trello boards |
| `servicenow` | ServiceNow tickets |
| `datadog` | Datadog monitors |
| `tableau` | Tableau dashboards |
| `people` | People directory |

Not all apps may be indexed in your instance. If a filter returns nothing, try without it.

## Multi-Angle Strategy

For thorough research, search 3 times with different angles:

1. **Specific terms:** Project names, repo names, tool names → finds artifacts
2. **Person + domain:** `"Person Name Vertex AI"` → finds discussions, reviews
3. **Entity names:** System IDs, config names, ticket numbers → finds configs, PRs

Specific technical terms beat broad role queries. `"OPSAI tenant GCP"` > `"my cloud work"`.

## Anti-Patterns

| Don't | Do Instead |
|-------|------------|
| Generic query, no filters | Add `app`, `from`, or date filter |
| `sort_by_recency` on every query | Only when "latest" or "most recent" matters |
| `exhaustive` to get more results | Refine query or add filters instead |
| Search for public/external info | Use WebSearch — Glean is internal only |
| Guess `dynamic_search_result_filters` | Only use values from actual results |
| Broad queries ("my work") | Specific terms ("content-dev-skills repo") |

## Agent Delegation

When spawning sub-agents for research:
- **Parent agent** queries Glean directly (keeps context in main conversation)
- **Sub-agents** handle external web research
- This prevents token drain — Glean results stay where they're most useful
