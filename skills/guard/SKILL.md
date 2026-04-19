---
name: guard
description: Use when making git operations, handling secrets, committing code, or dealing with credentials. Enforces git safety rules (no force-push to main, no amend of published commits), secret-handling discipline (never commit .env, use .env.example), and destructive-command caution.
---

# Safety Guardrails

## Git Safety

| Rule | Why |
|------|-----|
| Never force push to main/master | Destroys team history, breaks CI |
| Never `git reset --hard` without explicit confirmation | Irreversible data loss |
| Never amend published commits | Creates divergent history for collaborators |
| Never skip pre-commit hooks (`--no-verify`) | Hooks catch real issues |
| Stage specific files by name | `git add .` risks committing secrets or binaries |
| Never commit .env, credentials, API keys | Secrets in git history are permanent |
| Verify current branch before pushing | Avoid pushing to wrong branch |
| Never force-push to someone else's branch | Only to your own fork branches |

## Destructive Commands

These commands need explicit user confirmation before running:

| Command | Risk |
|---------|------|
| `rm -rf` | Permanent file deletion |
| `git reset --hard` | Discards uncommitted changes |
| `git push --force` | Overwrites remote history |
| `git clean -fdx` | Deletes untracked files including gitignored |
| `DROP TABLE` / `TRUNCATE` | Irreversible data loss |
| `kill -9` on shared processes | May corrupt state |
| Modifying CI/CD pipelines | Visible to whole team |

## Secrets and Credentials

### Never Commit
- `.env` files, `.env.*` (except `.env.example`)
- API keys, access tokens, bearer tokens
- Passwords, connection strings with credentials
- Private keys (`.pem`, `.key`, `id_rsa`)
- OAuth client secrets
- Cloud provider credentials (AWS, GCP, Azure)

### Use Placeholders
`.env.example` with placeholder values is safe to commit:
```bash
DATABASE_URL=postgresql://user:password@localhost:5432/dbname
API_KEY=your-api-key-here
AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
```

### If Real Credentials Are in Git History
1. Rotate the credential immediately -- git history is permanent and public
2. Use `git filter-repo` or BFG to remove from history
3. Force-push cleaned history (coordinate with team)
4. Audit access logs for suspicious use

### Scan Before Committing
```bash
# Check staged changes for common secret patterns
git diff --cached | grep -iE 'api[_-]?key|secret|password|token|bearer' | grep -v example

# Tools
gitleaks detect --source=. --verbose
trufflehog git file://. --only-verified
```

## Code Security Basics

| Rule | Why |
|------|-----|
| Validate user input at system boundaries | Prevents injection attacks |
| Use parameterized queries, never string concat | SQL injection |
| Sanitize data before rendering in templates | XSS |
| Use bcrypt/argon2 for password hashing | MD5/SHA are broken for passwords |
| Set CORS origins from env, never wildcard in prod | Cross-origin attacks |
| HTTPS only, no mixed content | MITM attacks |
| Rate-limit authentication endpoints | Credential stuffing / brute force |

## Dependency Safety

- Check vulnerabilities before adding new packages (`npm audit`, `pip-audit`, `cargo audit`)
- Prefer well-maintained packages with active security response
- Pin dependency versions in lock files
- Review package permissions (scripts, post-install hooks)
- Watch for typosquatting: `reqeusts` vs `requests`, `expresss` vs `express`

## Working with Shared Systems

Actions that affect others need confirmation:
- Pushing code (affects CI, teammates)
- Creating/closing/commenting on PRs or issues
- Sending messages (Slack, email, GitHub)
- Modifying shared infrastructure
- Deploying to staging/production

## Third-Party Uploads

Uploading content to external tools publishes it:
- Pastebins, gists, diagram renderers
- Cloud storage with sharable links
- AI chatbots (data may be retained/trained on)

Before uploading: is this content sensitive? PII, internal architecture, credentials?

## Anti-Patterns

| Anti-Pattern | Problem | Do Instead |
|-------------|---------|-----------|
| `git add .` | Accidentally stages secrets, binaries | Stage specific files by name |
| `--no-verify` to skip hooks | Bypasses real safety checks | Fix the hook failure, or ask why it's failing |
| Force push to shared branch | Destroys teammates' work | Rebase locally, only force-push your own branches |
| Committing secrets "just temporarily" | Git history is permanent | Never, always use .env.example |
| "It's just a test credential" | Real bad actors scan GitHub | Rotate and remove from history |
| `rm -rf` based on LLM suggestion | LLMs hallucinate paths | Verify path manually, dry-run with `ls` first |
