---
name: debug
description: Use when a test fails, a bug is reported, an error is pasted in, or a fix did not work. Enforces reproduce-before-theorize, reading the actual error, narrowing by bisection, one falsifiable hypothesis at a time, and a stop rule instead of guess-loops.
---

# Systematic Debugging

## Quick Reference

| Situation | First Move |
|-----------|-----------|
| Error message pasted in | Read the whole message and the top of the stack, not the first line |
| "It doesn't work" | Get a reproduction before touching any code |
| Intermittent failure | Find what varies between runs (order, timing, state, data) |
| Worked before, broken now | Bisect history to the first bad commit |
| Fix didn't fix it | Your model of the cause is wrong -- go back to reproducing |
| Two fixes already failed | Stop. State your assumptions and what would disprove them |

## The Loop

```
1. Reproduce -- make the failure happen on demand
2. Read -- the actual error, the actual stack, the actual input
3. Narrow -- bisect until the failure is in a small known region
4. Hypothesize -- one falsifiable statement about the cause
5. Test the hypothesis -- instrument or probe, do not fix yet
6. Fix -- the cause, not the symptom
7. Confirm -- same reproduction steps now pass
8. Regression-test -- a test that would have caught this
```

Steps 1-5 are the work. Skipping to 6 is why bugs get "fixed" twice.

## Reproduce Before You Theorize

A bug you cannot trigger on demand is a bug you cannot confirm you fixed.

- Write down the exact steps, input, and environment that trigger it
- Reduce to the smallest reproduction that still fails (fewer files, less data, fewer steps)
- If it only fails in CI or production, find the difference: version, env var, data volume, timezone, concurrency, file system, locale
- If it is intermittent, run it in a loop and record what differs between pass and fail

No reproduction means no verification later. Say so instead of guessing.

## Read the Actual Error

Pattern-matching an error to a familiar shape is the most common wrong turn.

- Read the full message, not the first line -- the cause is often in a nested or wrapped error
- Read the stack from the top frame in your own code, not the framework frames above it
- Check the line number against the file you are actually running (stale build, wrong virtualenv, cached bundle)
- Distinguish the error that surfaced from the error that caused it -- "connection refused" is a symptom, not a cause
- If the message contradicts the code, suspect what you are running, not what you are reading

## Narrow by Bisection

Halve the search space instead of scanning it.

| Unknown | Bisection |
|---------|-----------|
| Which commit broke it | `git bisect start` / `git bisect bad` / `git bisect good <sha>` |
| Which code path | Comment out or short-circuit half, re-run |
| Which input row | Binary-search the dataset until one record fails |
| Which dependency | Pin half the upgrades back, re-run |
| Which config | Start from empty config, add settings until it breaks |
| Which layer | Probe each boundary (client, network, handler, query, DB) in turn |

Ten halvings cover a thousand candidates. Reading a thousand lines does not.

## One Hypothesis at a Time

State the cause as a sentence that can be proven wrong, then try to prove it wrong.

```
Weak:   "something is wrong with the cache"
Strong: "the cache key omits the tenant id, so tenant B reads tenant A's row"
        Disproof: log the computed key for two tenants -- if they differ, wrong.
```

- One hypothesis per experiment, or you cannot tell which change mattered
- Change one variable at a time and re-run between changes
- Predict the result before you run it -- a surprise means your model is wrong, which is information
- Write the hypothesis down for anything that takes more than a few minutes

## Instrument, Don't Guess

When you do not know a value, measure it.

- Log or print the actual values at the boundary you suspect -- types, not just values
- Use a debugger or breakpoint when state is complex; use logs when timing or ordering matters
- Assert your assumptions inline so a wrong one fails loudly instead of flowing downstream
- For performance bugs, profile before optimizing -- the slow part is rarely where it feels
- For concurrency bugs, log with timestamps and thread/task ids, or the ordering stays invisible
- Remove the instrumentation before you ship, or promote it to real logging on purpose

## Fix the Cause, Not the Symptom

| Symptom Fix | Cause Fix |
|-------------|-----------|
| Wrap in try/except and continue | Handle the condition that made it throw |
| Add a null check at the crash site | Find why the value is null there |
| Add a retry | Find why the first attempt fails |
| Increase the timeout | Find what got slow |
| Add a sleep before the assertion | Wait on the actual condition |
| Silence the warning | Fix what it is warning about |

Symptom fixes are sometimes the right call under pressure. When you take one, say it is a mitigation and leave the real cause written down.

## The Stop Rule

After two failed fix attempts, stop editing code.

```
1. State what you believed the cause was
2. State what you changed and what actually happened
3. List the assumptions you have not verified
4. Ask for the missing information, or start over from reproduction
```

A third guess after two wrong ones is not debugging, it is shotgunning. The cost of shotgunning is a diff nobody can review and a bug that comes back.

## After the Fix

- Confirm with the original reproduction steps, not a variant of them
- Write the regression test -- the one that fails before the fix and passes after
- Check for the same mistake elsewhere in the codebase (same wrong pattern, other call sites)
- Note the root cause in the commit body, not just the change

## Anti-Patterns

| Anti-Pattern | Problem | Do Instead |
|-------------|---------|-----------|
| Fixing before reproducing | Cannot tell whether the fix worked | Reproduce first, always |
| Changing several things at once | No idea which one mattered | One variable per run |
| Reading the first line of the stack only | Real cause is deeper in the trace | Read the whole error |
| Assuming the error means what it usually means | Same message, different cause | Verify against this run's data |
| Guess-and-check loops | Burns time, produces unreviewable diffs | Stop after two failures, state assumptions |
| Deleting the failing test | Bug still ships, now silently | Fix the code or fix the wrong assertion |
| `print()` archaeology with no plan | Noise without a hypothesis | Instrument the specific boundary you suspect |
| Blaming the framework first | Usually your code, occasionally not | Prove it with a minimal reproduction |
| Fixing without a regression test | Same bug returns next quarter | Test that fails before, passes after |
