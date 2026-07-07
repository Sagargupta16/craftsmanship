# CLAUDE.md

> This file stacks on top of the workspace root at `C:\Code\GitHub\`:
> - Root [`CLAUDE.md`](../../CLAUDE.md) -- voice, rules, routing map, references, skills, slash commands, conventions.
> - Root [`MEMORY.md`](../../MEMORY.md) -- live facts across repos.
> - Root [`STATUS.md`](../../STATUS.md) -- live PR/CI/security dashboard.
> - [`.claude/resources/`](../../.claude/resources/README.md) -- deep reference for collaboration, workflow, git, OSS, debugging, voice.
>
> Read those first. The guidance below only adds **repo-specific context** -- it does not override anything in the root.

## Project

Agent-agnostic AI coding skills that encode engineering discipline (plan, guard, verify, review, audit, ship). Public OSS, MIT, consumed by any agent supporting the SKILL.md format via the skills.sh ecosystem.

Distribution: `npx skills add sagargupta16/craftsmanship` -- the CLI reads this repo straight from GitHub and fans skills out to each agent's skills directory.

## Stack

- **Language**: Markdown only (YAML frontmatter + body). No code, no dependencies, no package.json.
- **Framework**: SKILL.md format (skills.sh convention)
- **Database**: none
- **Package manager**: none (consumers use `npx skills`)
- **Deploy target**: GitHub repo itself is the artifact; nothing to build or publish

## Run

Nothing to run. Edit markdown, commit, push. Consumers pull the latest from GitHub.

## Test

No test suite. Verification is manual: install into an agent and confirm the skill activates on its trigger.

## Entry points

- `skills/<name>/SKILL.md` -- each of the 6 skills is a single self-contained file (plan, guard, verify, review, audit, ship)
- `README.md` -- public docs, install instructions, skill table

## Key files

- `README.md` -- source of truth for the skill list and philosophy; update the table when skills change
- `CHANGELOG.md` -- dated entries per release (initial release 2026-04-19)
- `renovate.json` -- extends `github>Sagargupta16/shared-workflows` preset

## Gotchas

- Skill names (audit, verify, review) collide with skills already available in local sessions (user-level `audit`, built-in `verify`/`review`) -- when editing, confirm you are in `community/craftsmanship/skills/`, not `~/.claude/skills/`.
- No CI workflows in-repo; Renovate config comes from the shared-workflows preset.
- Content must stay agent-agnostic (targets 45+ agents) -- no Claude Code-specific tool names or paths inside skill bodies.

## Repo-specific rules

- Every skill follows the existing format: YAML frontmatter (`name`, `description` with activation triggers), markdown body, closing Anti-Patterns table. Only `plan` and `verify` also have a Quick Reference table up top. Match the existing files for new skills.
- Adding or renaming a skill requires updating the README skill table and CHANGELOG in the same commit.
- New skills go through an issue first per CONTRIBUTING guidance in README (discuss scope: new skill vs addition to existing).
