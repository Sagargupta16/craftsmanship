---
name: ship
description: Use when preparing to ship a feature -- opening a PR, writing release notes, cutting a version, or coordinating a deploy. Covers PR hygiene, commit messages, release process, and post-ship verification.
---

# Shipping Work

## Before You Open a PR

| Check | Why |
|-------|-----|
| Tests pass locally | CI shouldn't catch obvious failures |
| Build succeeds | Broken builds block the whole pipeline |
| No debug logs / console.log / `print()` left over | Noise in production |
| No commented-out code | Git history preserves, comments pollute |
| Self-review your own diff | You catch things on re-read |
| Actually use the feature end-to-end | Tests don't verify user experience |
| No secrets in diff | Last chance to catch them |
| Commit messages are clean | Squash-merge uses them, or reviewers read them |
| Branch is rebased on main | Avoid merge-conflict noise |

## Commit Messages

### Conventional Commits Format

```
<type>(<scope>): <short description>

[optional body explaining WHY]

[optional footer with breaking changes, issue refs]
```

Common types:
- `feat:` new feature
- `fix:` bug fix
- `refactor:` code restructuring without behavior change
- `docs:` documentation only
- `test:` adding/updating tests
- `chore:` tooling, dependencies, maintenance
- `perf:` performance improvement
- `style:` formatting (rarely used -- let formatters do this)

### Good vs Bad Messages

Good:
```
fix: prevent race condition in order submission

Two concurrent submits could both pass the duplicate check
before either was saved. Added row-level lock on order_id.

Fixes #1234
```

Bad:
```
update
fix stuff
wip
asdf
```

### Rules
- Subject line under 72 characters
- Imperative mood: "add feature" not "added feature"
- Focus on WHY in the body, WHAT is in the diff
- Reference issues/tickets when relevant
- One logical change per commit

## PR Description

```markdown
## Summary
One sentence: what does this PR do?

## Why
Motivation, context, or ticket link.

## Changes
- Key change 1
- Key change 2
- Breaking changes flagged

## Testing
How was this tested? Screenshots/logs for visible changes.

## Rollout
Any special deploy considerations? Feature flags? Migrations?

## Notes for Reviewer
Trade-offs, open questions, things you're unsure about.
```

## Size Matters

| PR Size | Typical Review Quality |
|---------|----------------------|
| < 100 lines | Thorough review, fast merge |
| 100-400 lines | Solid review, reasonable time |
| 400-1000 lines | Cursory review, bugs slip through |
| > 1000 lines | Rubber-stamped, risky |

If a PR is growing past 400 lines, split it:
- Separate refactors from feature changes
- Land infrastructure before the feature that uses it
- Ship scaffolding first, fill in later

## Release Process

### Semantic Versioning

`MAJOR.MINOR.PATCH` (e.g., `2.4.1`)

- **MAJOR**: breaking changes (incompatible API changes)
- **MINOR**: new features (backwards compatible)
- **PATCH**: bug fixes (backwards compatible)

### Pre-Release Suffixes
- `1.0.0-alpha.1` -- internal testing
- `1.0.0-beta.1` -- external testing
- `1.0.0-rc.1` -- release candidate

### Release Checklist

```
1. Update CHANGELOG.md with all changes since last release
2. Bump version in package.json / Cargo.toml / pyproject.toml
3. Commit with "chore(release): vX.Y.Z"
4. Tag the commit: git tag -a vX.Y.Z -m "Release vX.Y.Z"
5. Push tag: git push origin vX.Y.Z
6. Create GitHub release with changelog content
7. Publish to package registry (npm, PyPI, crates.io)
8. Announce (Twitter, Slack, blog, etc.)
9. Monitor for issues in first 24-48 hours
```

### CHANGELOG Format

```markdown
# Changelog

## [2.4.1] - 2026-04-19

### Fixed
- Memory leak in event listener cleanup (#1234)
- Typo in error message

### Changed
- Improved startup time by 40%

## [2.4.0] - 2026-04-01

### Added
- New `/api/v2/search` endpoint
- Support for Redis as cache backend

### Deprecated
- `/api/v1/search` -- use v2 instead, v1 removed in 3.0.0
```

## Deploying

### Pre-Deploy Checklist
- [ ] All tests pass in CI
- [ ] Migrations run cleanly on staging
- [ ] Rollback plan documented
- [ ] On-call aware
- [ ] Feature flags configured (if applicable)
- [ ] Monitoring dashboards open

### During Deploy
- Watch logs and metrics in real time
- Have rollback one command away
- Don't deploy Friday afternoon (unless fixing a production issue)

### After Deploy
- Verify feature works in production (not just "it deployed")
- Check error rates didn't spike
- Check latency didn't regress
- Check that dependent systems still work

## Post-Ship

```
1. Close related issues
2. Update project board/tracker
3. Mention in standup / team channel
4. Write a brief retro if non-trivial: what went well, what would we do differently
5. Follow up on any customer-facing changes (support, docs, marketing)
```

## Anti-Patterns

| Anti-Pattern | Problem | Do Instead |
|-------------|---------|-----------|
| Giant PRs | Unreviewable, bugs ship | Split into < 400 line PRs |
| "Update" commit messages | Useless in git history | Conventional commits with WHY |
| Shipping without trying it | Broken features ship | Actually use the feature |
| Friday afternoon deploys | Weekend incidents | Deploy early in the week |
| No rollback plan | Incidents drag on | Know how to roll back before deploying |
| Version bumps without CHANGELOG | Users don't know what changed | Update CHANGELOG with every release |
| Skipping announcements | Users don't know new features exist | Even small ships deserve a mention |
| Merging with failing CI | Ships bugs, erodes trust in CI | Wait for green |
