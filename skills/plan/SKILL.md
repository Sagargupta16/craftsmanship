---
name: plan
description: Use when implementing features spanning 3+ files, when requirements are unclear, or when structuring multi-step work. Covers when to plan vs just code, plan structure, interview-then-execute, and prototype-over-spec patterns.
---

# Implementation Planning

## Quick Reference

| Approach | When to Use | Artifact |
|----------|-------------|----------|
| Direct execution | Small change, <3 files | Code |
| Lightweight plan | Medium feature, 3-5 files | Mental model + steps |
| Full written plan | Large feature, 5+ files | Plan document |
| Interview-then-execute | Unclear requirements | Q&A session, then plan |
| Prototype first | Exploratory / uncertain | Working prototype |

## Decision Tree

```
How big is this change?
├── 1-2 files, well-defined → Just do it (no plan needed)
├── 3-5 files, clear scope → Lightweight plan in conversation
├── 5+ files or architectural → Full written plan
├── Requirements unclear → Interview first
└── Feasibility unknown → Prototype first
```

## When to Plan Formally

- Architectural changes (new auth system, database migration)
- Multi-service changes (frontend + backend + database)
- Changes where ordering matters (migrations before code)
- When you need team alignment before coding
- Unfamiliar codebases -- planning forces understanding first

## What a Plan Should Contain

1. **Scope** -- what's changing and what's not
2. **Steps** -- numbered, ordered by dependency
3. **Files** -- specific files to create or modify per step
4. **Verification** -- how to test each step
5. **Risks** -- what could go wrong, rollback strategy

### Plan Template

```markdown
## Plan: [Feature Name]

### Scope
- Adding: [new capabilities]
- Modifying: [existing files/behavior]
- Not touching: [explicitly out of scope]

### Steps

1. **[Step name]**
   - Create/modify: `path/to/file.ts`
   - Details: [what specifically to do]
   - Verify: [how to confirm this step works]

2. **[Step name]**
   - Depends on: Step 1
   - Create/modify: `path/to/file.ts`
   - Verify: [test command or check]

### Risks
- [Risk]: [Mitigation]
```

## Executing Plans

```
1. Execute one step at a time
2. Verify after each step (run tests, check build)
3. Don't skip ahead -- dependencies matter
4. If a step fails, fix before proceeding
5. Update the plan if reality diverges from it
```

## Interview-Then-Execute

When requirements are unclear, have the agent interview you before coding.

### Pattern
```
1. "I need [vague feature]. Interview me to understand the requirements."
2. Agent asks 5-10 targeted clarifying questions
3. You answer, refining scope and constraints
4. Agent produces a plan based on answers
5. Review, adjust, then execute in a fresh session
```

### Why Separate Sessions
- Planning session fills context with requirements discussion
- Execution session gets clean context with only the finalized plan
- Produces better code than doing both in one session

## Prototype Over Spec

For exploratory work where requirements are uncertain, build instead of specifying.

### Pattern
```
Instead of: Write a 10-page PRD → review → implement → discover it's wrong
Do:         Build rough prototype → iterate 5-10 times → extract spec from what works
```

### When to Prototype
- New product ideas (you don't know what you want yet)
- UI/UX exploration (seeing beats describing)
- API design (try using it before specifying it)
- Algorithm selection (benchmark, don't theorize)

### When NOT to Prototype
- Well-defined bug fixes (just fix it)
- API contracts between teams (spec first)
- Database migrations (plan carefully, prototyping is dangerous)
- Security-critical code (spec and review first)

## Plan Review

Before executing a non-trivial plan, consider second-opinion review.

### Self-Review Checklist
- Are steps in correct dependency order?
- Does each step have clear verification?
- Are there any missing steps?
- Is scope creep controlled (not adding extras)?
- Can each step be independently tested?

### Second Opinion Pattern
```
1. Write the plan
2. Start a fresh session
3. "Review this plan as a staff engineer. What's missing, what could go wrong?"
4. Incorporate feedback
5. Execute in yet another session (clean context)
```

## Anti-Patterns

| Anti-Pattern | Problem | Do Instead |
|-------------|---------|-----------|
| Planning small changes | Overhead exceeds benefit | Just do it for <3 files |
| No plan for large changes | Miss dependencies, ordering issues | Plan for 5+ files |
| Planning and executing in one session | Planning context pollutes execution | Separate sessions |
| Over-specifying plans | Plans become rigid, resist reality | High-level steps, details during execution |
| Ignoring plan during execution | Plan was wasted effort | Follow it or explicitly update it |
| No verification steps in plan | Can't tell if steps succeeded | Every step needs a "verify" section |
