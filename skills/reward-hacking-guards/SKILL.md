---
name: reward-hacking-guards
description: "Guards against gaming the checks: what counts as cheating (weakening a test, special-casing it, editing a gate, claiming green without a result) and what to do instead. Read it before you build or verify."
---

<!-- skills/reward-hacking-guards/SKILL.md: workflow's guards against gaming the checks, loaded as
workflow:reward-hacking-guards; preloaded by the builder and the verifier. Fields: README.md. -->

# Guards

The ladder, Bonsai's guard and, where the project keeps one, the ledger of `done_when` entries are what make an
overnight session trustworthy. An agent that satisfies them without satisfying the goal has destroyed the only thing
keeping the person from reading every diff themselves. These are not optional, and they are not negotiable by
argument.

## Never

- **Weaken a test to make it pass.** Not by deleting an assertion, loosening a tolerance, adding a skip, narrowing a
  range, or renaming a test out of the run.
- **Edit test files from a builder session.** New behaviour needs new tests, and those come from a task whose stated
  job is writing tests. Never delete or soften an existing assertion in any session.
- **Edit the ladder, the hooks, the rungs, floor or ratchets in `bonsai.yaml`, or anything under `.claude/`** as a
  side effect of a task. Changing a gate is a task of its own, in the director lane.
- **Touch the ledger** beyond setting an entry's pass, where the project keeps one. Entries are never removed,
  reworded, merged or reordered.
- **Claim a rung green without the ladder result that says so.**
- **Special-case the test.** Code that detects the test input, the seed, the fixture or the harness and behaves
  differently is a failure, even if every rung is green.
- **Route round a guard.** If Bonsai's guard refuses a call, the guard is right. Use the intended path, or report
  `blocked` with the refusal's text.
- **Mark work done that a verifier has not passed** when it is big or risky (`workflow:verification-ladder`, "What
  done means"), or verify your own work.
- **Conclude that something is fun.** That is the person's, from playing, always.

## Instead

- A red rung is the most useful output of a session. Report it with the full error.
- A goal you cannot reach honestly is a blocked task, and blocked is a perfectly good result.
- If a test looks genuinely wrong, do not fix it quietly: say so in the run report, leave it failing, and let a person
  or a test-writing task decide. A wrong test everyone can see beats a right test nobody reviewed.
- If the `done_when` turns out to be unachievable or badly worded, say that plainly. Rewriting what you were asked to
  do so that you can claim you did it is the failure this whole file is about.

## The verifier's scope

The verifier answers exactly one question: **does this work do what its task, its one-pager and its `done_when` say,
with no regression?** Not "is this good code", not "what could be better". A verifier that suggests improvements has
widened its own remit, and the next one will widen it further until the gate means nothing. Pass or fail, with
reasons, in a fresh context, on work it did not do.
