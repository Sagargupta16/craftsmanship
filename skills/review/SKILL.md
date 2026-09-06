---
name: review
description: Use when reviewing code changes, examining PRs, or checking your own work before submitting. Covers what to look for (correctness, security, performance, style), how to review systematically, and how to give actionable feedback.
---

# Code Review

## What to Look For

Review in this priority order:

### 1. Correctness (highest priority)
- Does the code do what the PR description claims?
- Are there off-by-one errors, null/undefined cases, race conditions?
- Does it handle errors properly?
- Are edge cases covered (empty arrays, zero, negative, very large)?
- Does it break existing behavior?

### 2. Security
- Is user input validated at trust boundaries?
- Are queries parameterized (no SQL injection)?
- Is output encoded (no XSS)?
- Are secrets absent from code and logs?
- Are authentication/authorization checks in place?
- Are dependencies up to date and trusted?

### 3. Performance
- Are there N+1 queries?
- Is pagination used for list endpoints?
- Are loops doing synchronous I/O?
- Any unnecessary re-computation or re-renders?
- Are caches invalidated correctly?

### 4. Maintainability
- Does the code match existing patterns in the codebase?
- Are names clear and specific?
- Is complexity proportional to the problem?
- Are comments explaining WHY (not WHAT)?
- Is there duplicated logic that should be extracted?

### 5. Testing
- Are there tests for the new behavior?
- Do tests actually test behavior, not implementation details?
- Are both happy path AND error cases covered?
- Do existing tests still pass?

### 6. Style (lowest priority, automated)
- Let linters/formatters handle this
- Don't nitpick on whitespace or import order in review comments

## How to Review Systematically

```
1. Read the PR description -- what is this trying to do?
2. Check the test diff first -- does the tested behavior match the description?
3. Read the implementation diff
4. Run the code locally if the change is significant
5. Check CI status -- what's passing/failing?
6. Consider: how would this break in production?
7. Write comments grouped by severity
```

## Giving Good Feedback

### Categories of Comments

| Tag | Meaning | Example |
|-----|---------|---------|
| `blocking:` | Must fix before merge | `blocking: SQL injection in line 42` |
| `needs-discussion:` | Design concern worth debating | `needs-discussion: should we use JWT or sessions?` |
| `suggestion:` | Optional improvement | `suggestion: this could use Array.flatMap` |
| `nit:` | Trivial / stylistic | `nit: typo in comment` |
| `question:` | Asking to understand | `question: why did we choose this approach?` |
| `praise:` | Call out good work | `praise: nice error handling here` |

### What Makes a Good Comment

````markdown
blocking: SQL injection risk in `getUserByEmail`

The query uses string concatenation:
```
db.query(`SELECT * FROM users WHERE email = '${email}'`)
```

Use parameterized queries instead:
```
db.query('SELECT * FROM users WHERE email = $1', [email])
```

This prevents attackers from injecting SQL via crafted email values.
````

**Good comments**:
- State the problem
- Explain the impact
- Suggest a specific fix
- Link to docs or examples if helpful

**Bad comments**:
- "This is wrong" (no explanation)
- "I don't like this" (not actionable)
- Demanding changes without explaining why
- Restating what the code does

## Self-Review Before Submitting

Before marking your own PR ready:

| Check | Why |
|-------|-----|
| Re-read your own diff | Catches obvious issues |
| Run the feature end-to-end | Verifies it actually works |
| Remove debug logs, commented code, TODOs | Clean up |
| Update or add tests | Behavior without tests is untested |
| Update docs if behavior changed | Future-you will forget |
| Check for secrets in diff | Last chance to catch them |
| Write a clear PR description | Reviewers need context |

## PR Description Template

```markdown
## Summary
One sentence: what does this PR do?

## Why
The motivation / context / ticket link.

## Changes
- Bullet list of key changes
- Include new dependencies
- Flag breaking changes

## Testing
How was this tested? Include screenshots/logs for UI/output changes.

## Notes for Reviewer
Anything non-obvious? Trade-offs made? Open questions?
```

## Reviewing Others' PRs

Before leaving comments:
1. Read the comments already left -- don't duplicate
2. Check CI status -- is the author already fixing things?
3. Check how far behind the base branch -- outdated PRs may need rebase first
4. Assume good intent -- ask questions before assuming mistakes

## Anti-Patterns

| Anti-Pattern | Problem | Do Instead |
|-------------|---------|-----------|
| Rubber-stamping without reading | Bugs ship to production | Actually review the code |
| Reviewing only lines shown in diff | Miss broken callers | Check related files too |
| Nit-picking style | Wastes time, demoralizes | Let linters handle style |
| Blocking on opinion disagreements | Gridlock | Distinguish blocking from suggestion |
| Vague comments ("this is bad") | Author can't act | State problem + impact + fix |
| Approving while CI is red | Merges broken code | Wait for CI green |
| Reviewing while tired | Miss obvious bugs | Batch reviews, review fresh |
