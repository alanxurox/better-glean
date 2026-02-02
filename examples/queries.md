# Query Cookbook

Real-world Glean query patterns, tested against a live instance. Copy-paste these as starting points.

## Daily Standup: What did I touch recently?

```python
search(query="*", from="me", sort_by_recency=true)
# Returns your recent activity across all sources
```

## Find this week's Slack discussions on a topic

```python
search(query="migration", app="slack", updated="past_week")
# Targeted Slack search with time filter
```

## Find my recent Slack messages

```python
search(query="*", app="slack", from="me", sort_by_recency=true)
# Your Slack messages across all channels
```

## Find PRs in a specific repo

```python
search(query="content-dev-skills", app="githubenterprise", type="pull")
# All PRs mentioning this repo name
```

## Find PRs I authored (not just commented on)

```python
search(query="*", app="githubenterprise", type="pull", owner="me")
# owner = authored. from = touched (includes reviews/comments)
```

## Find what someone wrote about a topic

```python
search(query="Vertex AI", from="Person Name")
# Everything this person touched related to Vertex AI
```

## Find Confluence docs by a specific author

```python
search(query="architecture", app="confluence", owner="Person Name")
# Docs they authored (not just commented on)
```

## Find Jira tickets I'm involved in

```python
search(query="style guide", app="jira", from="me")
# Tickets you created, commented on, or are assigned to
```

## Find Slack DMs about a topic

```python
search(query="handover notes", app="slack", type="direct message")
# Note: DM indexing depends on your org's Glean privacy settings
```

## Find everything from a specific Slack channel

```python
search(query="*", app="slack", channel="p-sdlc")
# All indexed messages from this channel
```

## Find docs updated in a date range

```python
search(query="AI SDLC", after="2026-01-01", before="2026-02-01")
# January 2026 docs about AI SDLC
```

## Find team activity

```python
search(query="*", from="myteam", sort_by_recency=true)
# Only "me" and "myteam" work as special values
```

## Exhaustive search for compliance/audit

```python
search(query="GCP tenant config", exhaustive=true)
# Returns ALL matching results, not just top ones
# Use sparingly — expensive query
```

## Refine with discovered filters

```python
# Step 1: Initial search
search(query="Norton VPN")
# Check matching_filters in results

# Step 2: Refine with discovered filters
search(query="Norton VPN", dynamic_search_result_filters="brand:Norton|product:VPN")
# Only use key:value pairs you actually saw in step 1 results
```

## Multi-angle research pattern

For thorough research, run 3 searches with different angles:

```python
# Angle 1: Specific artifact names
search(query="content-dev-skills repo", app="githubenterprise")

# Angle 2: Person + domain
search(query="style guide automation", from="Alan Xu")

# Angle 3: Recent discussions
search(query="agent skills", app="slack", updated="past_week")
```

## Full document retrieval workflow

```python
# Step 1: Find the document
results = search(query="email automation architecture", app="confluence")

# Step 2: Get full content of the best match
read_document(urls=["https://confluence.example.com/display/ENG/Email+Automation"])

# Step 3: Ask a synthesis question if needed
chat(message="Based on the email automation architecture doc, what are the key dependencies?")
```
