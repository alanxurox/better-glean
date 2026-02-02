# Contributing

Found a Glean query pattern that works well? Contributions welcome.

## What to Contribute

**Query patterns** — The most valuable contribution. If you found a parameter combination that reliably returns useful results, add it to:

- `SKILL.md` — Reference patterns (concise, one per section)
- `examples/queries.md` — Cookbook patterns (detailed, with context)

**Agent improvements** — If you've tested `agents/glean-researcher.md` and found it fails on certain query types, fix the planning logic or parameter selection rules.

**App filter discoveries** — If your Glean instance indexes sources not listed in the app filter reference, add them.

## How to Contribute

1. Fork the repo
2. Test your pattern against a live Glean instance
3. Add it with a clear description of what it finds
4. Open a PR with the query and expected behavior

## Quality Bar

- **Tested**: Every pattern must be tested against a live Glean MCP instance
- **Specific**: Include the exact parameters, not vague descriptions
- **Honest**: Note limitations (e.g., "works for Slack, untested on Teams")

## What Not to Contribute

- Patterns that only work with specific company data (keep them generic)
- External search patterns (Glean is internal-only)
- Untested patterns marked as "should work"
