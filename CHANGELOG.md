# Changelog

All notable changes to better-glean.

## [0.2.0] - 2026-02-02

### Added
- `agents/glean-researcher.md` — autonomous agent that plans and executes multi-angle Glean searches
- `examples/queries.md` — tested query cookbook with real-world patterns
- Rate limiting guidance in SKILL.md
- DM privacy note in SKILL.md
- `from` vs `owner` clarification throughout
- CONTRIBUTING.md, LICENSE, .gitignore

### Changed
- Hero example: simplified to tested patterns (slack+updated, owner+wildcard)
- Version: 0.1.0 → 0.2.0

### Fixed
- Previous hero example returned 0 results (too many simultaneous filters)
- `from="me"` misleadingly described as "your PRs" when it means "PRs you touched"

## [0.1.0] - 2026-02-02

### Added
- Initial SKILL.md with 12 query patterns
- App filter reference (40+ sources)
- Multi-angle search strategy
- Anti-patterns guide
- Agent delegation rules
- Plugin marketplace and OpenSkills install methods

### Fixed
- Parameter count: "15+" corrected to "12"
- Hero examples simplified to tested patterns

## [0.0.1] - 2026-02-01

### Added
- Initial release with SKILL.md, README.md, .agentskills.json
- Basic Glean search patterns
- App filter reference
