# craftsmanship

Skills that encode engineering discipline -- think before coding, verify before claiming done, ship with care.

These are agent-agnostic skills compatible with 45+ AI coding agents (Claude Code, Cursor, Codex, Cline, Gemini CLI, GitHub Copilot, and more) via the [`skills`](https://skills.sh) ecosystem.

## Install

```bash
npx skills add sagargupta16/craftsmanship
```

You'll be prompted to select which skills to install and which agents to install them to. Skills are copied to each agent's conventional skills directory.

## The Six Skills

| Skill | When It Activates |
|-------|-------------------|
| [**plan**](skills/plan/SKILL.md) | Implementing features spanning 3+ files, unclear requirements, or multi-step work |
| [**guard**](skills/guard/SKILL.md) | Git operations, handling secrets, or running destructive commands |
| [**verify**](skills/verify/SKILL.md) | Before claiming work is done, before opening a PR |
| [**review**](skills/review/SKILL.md) | Reviewing code changes, examining PRs, checking your own work |
| [**audit**](skills/audit/SKILL.md) | Setting up a new repo or auditing an existing one for hygiene issues |
| [**ship**](skills/ship/SKILL.md) | Preparing to ship -- opening a PR, cutting a release, deploying |

## Philosophy

Three rules run through all six skills:

1. **Think first for big changes.** A plan for a 10-file feature costs 15 minutes; debugging the wrong implementation costs a day.
2. **Never claim done from tests alone.** Tests verify code correctness, not feature correctness. Actually run the thing.
3. **Small reversible actions are free. Destructive actions need confirmation.** `rm -rf`, `git push --force`, `DROP TABLE` -- pause and confirm.

## Install Individual Skills

The `skills` CLI lets you pick which skills to install:

```bash
npx skills add sagargupta16/craftsmanship
# Then select: plan, guard, verify, review, audit, ship (space to toggle)
```

## Compatible Agents

Installed skills work with any agent that reads `.agents/skills/` or agent-specific directories. The CLI handles the fan-out:

- Amp, Antigravity, Cline, Codex, Cursor, Deep Agents, Firebender
- Gemini CLI, GitHub Copilot, Kimi Code CLI, OpenCode, Warp
- Claude Code (via `.claude/skills/`)
- Any other agent that supports the SKILL.md format

## Structure

```
craftsmanship/
├── README.md
├── LICENSE
├── CHANGELOG.md
└── skills/
    ├── plan/SKILL.md
    ├── guard/SKILL.md
    ├── verify/SKILL.md
    ├── review/SKILL.md
    ├── audit/SKILL.md
    └── ship/SKILL.md
```

Each skill is self-contained -- a single `SKILL.md` with YAML frontmatter and markdown body.

## Contributing

Issues and PRs welcome. If you've spotted a gap or have a new skill to propose:

1. Open an issue describing the skill and when it should activate
2. Discuss scope -- is this a new skill or an addition to existing?
3. Follow the existing format: Quick Reference table, body, Anti-Patterns table

## Also By This Author

- [`sagar-dev-skills`](https://github.com/Sagargupta16/claude-skills) -- Claude Code plugin marketplace with 25 plugins including hooks, agents, and commands
- [`claude-cost-optimizer`](https://github.com/Sagargupta16/claude-cost-optimizer) -- Save 30-60% on Claude Code costs

## License

MIT. See [LICENSE](LICENSE).
