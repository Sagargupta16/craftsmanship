---
name: audit
description: Use when setting up a new repo, auditing an existing one for hygiene issues, or before publishing a project. Covers .gitignore, .env.example, README, LICENSE, CONTRIBUTING, and detecting committed secrets.
---

# Repository Hygiene Audit

## Essential Files Checklist

| File | Purpose | Why It Matters |
|------|---------|----------------|
| `README.md` | Project overview, quick start | First impression, onboarding |
| `LICENSE` | Legal terms for use | Without one, code is "all rights reserved" by default |
| `.gitignore` | Files to exclude from git | Prevents committing secrets, build artifacts |
| `.env.example` | Template for env vars | Shows structure without revealing secrets |
| `CONTRIBUTING.md` | How to contribute | Reduces friction for contributors |
| `CHANGELOG.md` | Version history | Users track what changed |
| `CODE_OF_CONDUCT.md` | Community expectations | Required for many registries |

## .gitignore Essentials

Minimum patterns for any repo:
```gitignore
# Secrets
.env
.env.local
.env.*.local

# OS
.DS_Store
Thumbs.db
desktop.ini

# Editors
.vscode/
.idea/
*.swp
*.swo
*~

# Logs
*.log
logs/

# Dependencies (language-specific)
node_modules/
__pycache__/
*.pyc
.venv/
venv/
target/
vendor/

# Build artifacts
dist/
build/
out/
*.egg-info/

# Coverage
coverage/
.coverage
htmlcov/
```

Language additions:
- **Node**: `node_modules/`, `dist/`, `.next/`, `.turbo/`
- **Python**: `__pycache__/`, `*.pyc`, `.venv/`, `venv/`, `.pytest_cache/`, `.mypy_cache/`
- **Rust**: `target/`, `Cargo.lock` (for libs, commit for bins)
- **Go**: `vendor/`, `bin/`
- **Terraform**: `.terraform/`, `*.tfstate*`, `*.tfvars` (if contains secrets)
- **Java**: `target/`, `.gradle/`, `*.class`

## README Essentials

Minimum sections:
```markdown
# Project Name

One-line tagline.

## Why
What problem does this solve?

## Install
Exact commands to install.

## Quick Start
Minimal working example.

## Usage
Common use cases with code examples.

## License
License name with link to LICENSE file.
```

Nice to have:
- Badges (build status, version, license)
- Screenshots/GIFs for visual tools
- Link to docs site
- Contributing section (even if just linking CONTRIBUTING.md)
- Acknowledgments / credits

## .env.example Pattern

```bash
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/dbname

# Auth
JWT_SECRET=change-me-to-a-long-random-string
OAUTH_CLIENT_ID=your-oauth-client-id
OAUTH_CLIENT_SECRET=your-oauth-client-secret

# External APIs
OPENAI_API_KEY=sk-...
AWS_REGION=us-east-1

# Feature flags
ENABLE_BETA_FEATURES=false
```

Rules:
- Every variable the app reads at runtime
- Placeholder values that look like the real thing (so users know the format)
- Comments grouping related vars
- Never real credentials

## Secret Scanning

Before publishing a repo publicly, scan for leaked secrets:

```bash
# Check git history for common patterns
git log -p | grep -iE 'api[_-]?key|secret|password|token|bearer' | grep -v example

# Dedicated tools
gitleaks detect --source=. --verbose
trufflehog git file://. --only-verified

# GitHub's own scanner runs automatically on public repos
```

If secrets are found in history:
1. Rotate them immediately
2. Use `git filter-repo` or BFG to remove from history
3. Force-push cleaned history
4. Enable GitHub secret scanning for the repo going forward

## LICENSE Choice

| License | When to Use |
|---------|-------------|
| **MIT** | Permissive, short, widely used. Best default for most projects |
| **Apache 2.0** | Like MIT but with patent protection. Larger projects, corporate use |
| **GPL-3.0** | Copyleft -- derivatives must also be GPL. Strong software freedom |
| **BSD-3-Clause** | Very permissive, slightly different from MIT |
| **Unlicense** / CC0 | Public domain, no restrictions |
| **Proprietary** | Internal/commercial code. No file = "all rights reserved" |

Generate: https://choosealicense.com

## Config File Hygiene

- Commit: `package.json`, `requirements.txt`, `Dockerfile`, `tsconfig.json`
- Commit with placeholders: `.env.example`, `config.example.yml`
- Do NOT commit: `.env`, `config.yml` (with real values), `node_modules/`, build artifacts
- Lock files (`package-lock.json`, `yarn.lock`, `Cargo.lock`, `poetry.lock`): commit for apps, skip for libs

## Auditing an Existing Repo

```
1. Check for .gitignore -- does it cover your stack?
2. Check for README -- is it accurate and useful?
3. Check for LICENSE -- is there one?
4. Check for .env.example -- if app uses env vars
5. Scan for committed secrets (gitleaks)
6. Check for outdated dependencies
7. Check CI status -- is it green?
8. Check stale branches -- prune or merge
9. Check for TODO/FIXME density -- tech debt signal
10. Check commit message quality -- conventional commits?
```

## Anti-Patterns

| Anti-Pattern | Problem | Do Instead |
|-------------|---------|-----------|
| No README | Users can't onboard | Even a 5-line README helps |
| README listing what code does | Duplicates the code | Focus on WHY and HOW TO USE |
| No LICENSE | "All rights reserved" by default -- users can't legally use it | Pick one from choosealicense.com |
| Committing `.env` | Secrets leaked forever | Use `.env.example`, add `.env` to `.gitignore` |
| Broken links in README | Unprofessional, frustrating | Test links periodically |
| Outdated dependencies with CVEs | Security risk | Automated updates (Renovate, Dependabot) |
| Empty CONTRIBUTING.md | Signals unmaintained | Either write one or remove file |
