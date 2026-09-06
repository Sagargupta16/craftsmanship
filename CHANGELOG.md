# Changelog

Entries are dated, not versioned. There are no git tags or GitHub releases: the
`skills` CLI installs from the default branch, so the newest commit on `main` is
the release. Read that as deliberate rather than an oversight.

## 2026-09-06

### Added
- **debug** skill -- reproduce before theorizing, read the actual error, narrow by bisection, one falsifiable hypothesis at a time, instrument instead of guessing, and a stop rule after two failed fixes
- `CONTRIBUTING.md` -- required frontmatter, house format, the agent-agnostic rule, and how to lint a skill before opening a PR
- CI on push and pull request -- `skillcheck lint` over `./skills` plus a link check over the markdown
- README section showing what a skill actually looks like

### Fixed
- **review** -- the "What Makes a Good Comment" example was destroyed by nested code fences: the inner bare fence closed the outer one, so the vulnerable SQL rendered as prose and the prose rendered inside code blocks. The outer fence is now four backticks.
- README install docs -- skills are symlinked by default (`--copy` opts out), agents are auto-detected rather than prompted for, and "Install Individual Skills" now shows the real `--skill` syntax instead of repeating the whole-repo command
- README -- the sibling plugin marketplace has 16 plugins, not 14
- README -- the `skills` ecosystem supports 75+ agents, not 45+; the compatible-agents list now points at the upstream table instead of a hand-maintained subset
- README structure tree -- listed only 4 of the tracked files

## 2026-04-19

### Added
- Initial release with 6 skills:
  - **plan** -- implementation planning, interview-then-execute, prototype-over-spec
  - **guard** -- git safety, secret handling, destructive command caution
  - **verify** -- pre-completion verification for backend, frontend, infra, bug fixes
  - **review** -- code review priorities, feedback patterns, self-review checklist
  - **audit** -- repo hygiene, .gitignore, LICENSE, README, secret scanning
  - **ship** -- PR hygiene, commit messages, releases, deploys
