# Agent Skills

A curated collection of **Agent Skills** built on the
[Open Agent Skills Standard](https://agentskills.io/integrate-skills),
focused on **Go backend engineering, DevOps, and production reliability**.

## Structure

Each skill lives in its own folder and contains a `SKILL.md` file: `<skill-name>/SKILL.md`

Parent folders (if any) are for human organization only and do not affect
agent discovery.

## Skill Categories

### Git & Code Lifecycle
- compose-commit
- summarize-diff

### Go Engineering
- go-code-review
- go-concurrency-audit
- generate-go-tests

### DevOps & Infrastructure
- dockerize-go-service
- k8s-manifest-review

### Reliability
- incident-triage

## Usage

These skills are designed to be loaded by Codex or other agents that
support the Agent Skills standard.

The agent selects a skill based on:
- `name`
- `description`
- frontmatter metadata

## Contributing

- Folder name must match `name` in `SKILL.md`
- Use lowercase letters, numbers, and hyphens only
- Keep skills small, focused, and composable
- Avoid embedding secrets or environment-specific assumptions

## License

MIT