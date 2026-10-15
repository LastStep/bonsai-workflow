---
name: verification-ladder
description: "The verification ladder: how a claim becomes a fact. The project's rungs, how to climb them with bonsai ladder, the rules of a climb, the three-failures circuit breaker, and what done means. Read it before you prove work or judge a proof."
---

<!-- skills/verification-ladder/SKILL.md: workflow's ladder protocol, loaded as workflow:verification-ladder;
preloaded by the builder, the verifier and the runner. Fields: README.md. -->

# The verification ladder

Generation is cheap. Verification is the bottleneck, and games have the worst verification signal in software. The
ladder is how a claim becomes a fact.

## How it works

Rungs are numbered, cheapest first. **Each project defines its own rungs** in its `bonsai.yaml` (each rung's command,
whether it is required, what its failure means), with a floor of rungs every task climbs and ratchets that may not
fall. A task adds rungs beyond the floor in its `bonsai.ladder` label, never fewer. Climb them:

```bash
bonsai ladder --task <id> --json
```

The ladder writes its result to `.bonsai/local/ladder/<task>.json` in the main checkout (a worktree's climb too): the
proof a named session's stop gate reads before it lets the session end, and the one a verifier reads. **You never
write that file by hand.** A run report whose green rungs do not match a real result is a failed session, not a
finished one.

**A CI result is never a task's proof.** A climb with `--ci` writes `.bonsai/local/ladder/ci.json`, stamped
`mode: ci`, and may skip the tests the project marks as needing a real checkout. The proof is a local climb, where a
marked test that skips turns its rung red.

## The rules

1. **Climb from the bottom, every time.** Rung 1 failing makes rung 6's result meaningless. The ladder stops at the
   first required failure.
2. **Climb before you believe you are finished, not after you have said so.** Once your work is done, and again after
   each fix: not after every edit.
3. **Paste the evidence.** Test counts, exit codes, the error text, the screenshot path. "Tests pass" is not evidence;
   `287 passed, 0 failed` is.
4. **A red rung is information, not an obstacle.** Report it. Never route round it.
5. **Never worse.** The test count and the project's other ratchets (simulation invariants, build size) may not
   regress. If your change drops the test count, that is a failure even when every rung is green: say why, or fix it.

## The circuit breaker

Three failed attempts at the **same** error and you stop. Not a fourth idea, not a workaround.

1. Write the full error into the run report: the real text, not your summary of it.
2. Report `blocked`, the blocker in one plain sentence (if no one briefed you, set the task to `blocked` and fill
   `blocked_by` with that sentence).
3. Stop.

A builder's budget holds the same line in code: four red climbs in a row (its `red_runs`, unless the project or the
brief sets another) and every call but its report is refused. A blocked task with a good error report is a useful
night's work. Eleven attempts that end in a disabled test are not.

## What "done" means

All of these, in order (step 2 only for big or risky work):

1. Every rung the task needs is green, in one climb, on the final code, with nothing uncommitted.
2. For big or risky work, a **verifier in a fresh context** checked it and passed it. Big or risky: guards, the
   ladder's proof logic, tokens and security, deploys, a public contract, and the full and director lanes. Related
   light tasks share one verifier. The rest close on the green ladder and the orchestrator's read of the diff, and the
   run report says so (the director, 6 Oct).
3. Where the project keeps a ledger of `done_when` entries, each entry's pass is set once the verifier passed it (by
   the orchestrator, since a verifier changes no file), or, with no verifier, once the orchestrator read the diff;
   never by the builder.
4. For anything a player can feel: **the director played it.** No sim, metric or agent opinion substitutes.

Steps 1 to 3 let a task land. Only step 4 closes it.
