# Changelog

Entries are dated, not versioned. There are no git tags or GitHub releases: the
`skills` CLI installs from the default branch, so the newest commit on `main` is
the release.

## 2026-09-06

### Added
- **debug** skill -- reproduce before theorizing, read the actual error, narrow by bisection, one falsifiable hypothesis at a time, instrument instead of guessing, and a stop rule after two failed fixes
- `CONTRIBUTING.md` -- required frontmatter, house format, the agent-agnostic rule, and how to lint a skill before opening a PR
- CI on push and pull request -- `@sagargupta1610/skillcheck` lint (pinned; first-party tooling, maintained by this repo's author) over `./skills`, plus a lychee link check over every markdown file except `CLAUDE.md`
- README section showing what a skill actually looks like
- CI badge in the README, next to the license badge

### Fixed
- **review** -- the "What Makes a Good Comment" example was broken by nested code fences: the inner bare fence closed the outer one, so the vulnerable SQL rendered as prose and the prose rendered inside code blocks. The outer fence is now four backticks.
- README install docs -- the CLI detects installed agents and only prompts for agents when it finds none, symlink-vs-copy is a choice you make during an interactive install, and "Install Individual Skills" now shows the real `--skill` syntax instead of repeating the whole-repo command
- README -- the sibling plugin marketplace has 16 plugins, not 14
- README -- the `skills` ecosystem supports 75+ agents, not 45+; the compatible-agents list is refreshed and now names the upstream table as the authoritative source
- README structure tree -- omitted `CLAUDE.md`, `.gitignore` and `renovate.json`

## 2026-04-19

### Added
- Initial release with 6 skills:
  - **plan** -- implementation planning, interview-then-execute, prototype-over-spec
  - **guard** -- git safety, secret handling, destructive command caution
  - **verify** -- pre-completion verification for backend, frontend, infra, bug fixes
  - **review** -- code review priorities, feedback patterns, self-review checklist
  - **audit** -- repo hygiene, .gitignore, LICENSE, README, secret scanning
  - **ship** -- PR hygiene, commit messages, releases, deploys
