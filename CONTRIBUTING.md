# Contributing

Issues and PRs welcome. These skills are meant to be short, opinionated, and correct -- a skill an agent reads mid-task, not a wiki page.

## Proposing a Skill

1. Open an issue describing the skill and when it should activate
2. Discuss scope -- is this a new skill or an addition to an existing one?
3. Once the scope is agreed, open a PR following the format below

Additions to an existing skill do not need an issue first. Fixing a wrong claim, a broken link, or a mangled code block never does.

## Required Frontmatter

Every skill is one file at `skills/<name>/SKILL.md`. It opens with YAML frontmatter carrying exactly two keys:

```yaml
---
name: debug
description: Use when a test fails, a bug is reported, an error is pasted in, or a fix did not work. Enforces reproduce-before-theorize, reading the actual error, narrowing by bisection, one falsifiable hypothesis at a time, and a stop rule instead of guess-loops.
---
```

- `name` matches the directory name, lowercase, one word where possible
- `description` is what the agent matches against, so it must name the triggers explicitly. Start it with "Use when ..." and list the concrete situations, then summarise what the skill covers. A description that only says what the skill is about will not fire.

## House Format

Read two or three existing skills before writing one. The shape is:

1. `# Title` -- the discipline, not the filename
2. Optional `## Quick Reference` table -- a situation-to-action lookup. Only `plan`, `debug`, and `verify` have one; add it when the skill has a natural "which case am I in" entry point.
3. Body sections -- `##` headings, tables and fenced blocks over prose. Numbered `code` blocks are used for ordered procedures.
4. Required closing `## Anti-Patterns` table -- three columns: `Anti-Pattern | Problem | Do Instead`. This is not optional; it is where most of the value lands.

Other conventions to match:

- Write `--` for a dash. No em-dashes or en-dashes anywhere.
- No emojis.
- Tables are pipe-delimited with a `|---|` separator row, no alignment colons.
- If an example contains a fenced code block, open and close the outer example with four backticks so the inner three-backtick fences do not close it early. See the worked example in `skills/review/SKILL.md`.

## Stay Agent-Agnostic

These skills install into 75+ different agents, so the content cannot assume any one of them.

- No agent-specific tool names, slash commands, or config paths
- No agent-specific file layout (`.claude/`, `.cursor/` and friends are the CLI's business, not the skill's)
- Shell commands, git, and language tooling are fine -- they are what every agent drives
- Prefer "the agent" over naming a product

## Check Before Opening a PR

Lint the skills against the Agent Skills spec:

```bash
npx -y @sagargupta1610/skillcheck lint ./skills
```

Expect `0 error(s), 0 warning(s)`. CI runs the same command on every push and pull request, plus a link check over the markdown.

Two things the linter cannot catch, so check them by hand:

- **Nested code fences.** Render your file on GitHub (or push the branch and look at it) and confirm each example is one block. A bare three-backtick fence inside a fenced markdown example silently closes it and scrambles everything after.
- **Claims about tools.** If you document a command or a flag, run it first. `--help` output beats memory.

Keep the diff scoped: one skill or one fix per PR.
