# craftsmanship

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Skills that encode engineering discipline -- think before coding, verify before claiming done, ship with care.

These are agent-agnostic skills compatible with 75+ AI coding agents (Claude Code, Cursor, Codex, Cline, Gemini CLI, GitHub Copilot, and more) via the [`skills`](https://skills.sh) ecosystem.

## Install

```bash
npx skills add sagargupta16/craftsmanship
```

The CLI auto-detects the coding agents you have installed and fans the skills out to each one; if it finds none, it prompts you to pick. Skills are symlinked into each agent's conventional skills directory -- pass `--copy` if you want real files instead.

Other flags worth knowing:

```bash
npx skills add sagargupta16/craftsmanship -g   # user-level instead of project-level
npx skills update                              # refresh installed skills
npx skills list                                # see what is installed
```

## The Skills

| Skill | When It Activates |
|-------|-------------------|
| [**plan**](skills/plan/SKILL.md) | Implementing features spanning 3+ files, unclear requirements, or multi-step work |
| [**guard**](skills/guard/SKILL.md) | Git operations, handling secrets, or running destructive commands |
| [**debug**](skills/debug/SKILL.md) | A test fails, a bug is reported, or a fix did not work |
| [**verify**](skills/verify/SKILL.md) | Before claiming work is done, before opening a PR |
| [**review**](skills/review/SKILL.md) | Reviewing code changes, examining PRs, checking your own work |
| [**audit**](skills/audit/SKILL.md) | Setting up a new repo or auditing an existing one for hygiene issues |
| [**ship**](skills/ship/SKILL.md) | Preparing to ship -- opening a PR, cutting a release, deploying |

## What a Skill Looks Like

Each file is YAML frontmatter plus a markdown body written to be read by an agent mid-task. The tables are the load-bearing part -- they turn a vague instruction ("verify it works") into a lookup. From [`skills/verify/SKILL.md`](skills/verify/SKILL.md):

| Change Type | Minimum Verification |
|-------------|---------------------|
| Backend API | Run server, hit endpoints, check responses |
| Frontend UI | Start dev server, test in browser, check edge cases |
| Database migration | Run migration, verify schema, test rollback |
| Infrastructure/IaC | Plan/validate, check for destructive changes |
| Bug fix | Reproduce bug, apply fix, confirm fix, check for regressions |

The `description` field in the frontmatter is what the agent matches against, so it names the triggers explicitly ("Use when completing a task, before claiming work is done, or before creating a PR"). The body is what the agent reads once triggered.

## Philosophy

Four rules run through the skills:

1. **Think first for big changes.** A plan for a 10-file feature costs 15 minutes; debugging the wrong implementation costs a day.
2. **Never claim done from tests alone.** Tests verify code correctness, not feature correctness. Actually run the thing.
3. **Reproduce before you theorize.** A bug you cannot trigger on demand is a bug you cannot confirm you fixed.
4. **Small reversible actions are free. Destructive actions need confirmation.** `rm -rf`, `git push --force`, `DROP TABLE` -- pause and confirm.

## Install Individual Skills

Use `--skill` to install a subset instead of the whole set:

```bash
npx skills add sagargupta16/craftsmanship --skill plan
npx skills add sagargupta16/craftsmanship --skill debug verify review
npx skills add sagargupta16/craftsmanship --list   # see what is available first
```

`--agent` narrows the install the same way (`--agent claude-code cursor`), and `--agent '*'` targets every agent the CLI knows about.

## Compatible Agents

Installed skills work with any agent that reads `.agents/skills/` or an agent-specific directory. The CLI handles the fan-out:

- Claude Code (`.claude/skills/`), Cursor, Codex, GitHub Copilot, Gemini CLI, OpenCode
- Cline, Windsurf, Zed, Warp, Amp, Antigravity, Continue, Kilo Code, Roo Code, Trae
- Augment, Goose, Droid, Junie, Qwen Code, Grok Build, Kiro CLI, OpenHands, Devin for Terminal
- Any other agent that supports the SKILL.md format

The full, authoritative list of agents and the exact install paths each one uses lives in the upstream [supported agents table](https://github.com/vercel-labs/skills#supported-agents).

## Structure

```
craftsmanship/
├── README.md
├── CONTRIBUTING.md
├── CHANGELOG.md
├── CLAUDE.md
├── LICENSE
├── .gitignore
├── .lycheeignore
├── renovate.json
├── .github/workflows/ci.yml
└── skills/
    ├── plan/SKILL.md
    ├── guard/SKILL.md
    ├── debug/SKILL.md
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
3. Follow the existing format: body, then Anti-Patterns table

See [CONTRIBUTING.md](CONTRIBUTING.md) for the required frontmatter, the house format, and how to lint a skill before opening a PR.

## Also By This Author

- [`sagar-dev-skills`](https://github.com/Sagargupta16/claude-skills) -- Claude Code plugin marketplace with 16 plugins including hooks, agents, and commands
- [`claude-cost-optimizer`](https://github.com/Sagargupta16/claude-cost-optimizer) -- Save 30-60% on Claude Code costs

## License

MIT. See [LICENSE](LICENSE).
