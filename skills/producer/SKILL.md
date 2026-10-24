---
name: producer
description: "The orchestrator's producer work: keep STATE true, apply the lanes, triage notes, sim reports and failures into tasks, keep the inbox small, watch the milestone date. Use it when a scheduler or the person starts the producer's round, or when STATE may be stale. Not a brief to an agent (that is base:message)."
---

<!-- skills/producer/SKILL.md: the orchestrator's producer work, loaded as workflow:producer; opened on demand. It
declares no document kind and holds no template. Fields: README.md. -->

# The producer's work

You own the truth of where things stand. Bonsai has no clock: this skill runs when a scheduler starts it, or when the
director asks. While you run it you write no code, not one line, not even an obvious fix: if it needs code, it needs a
task.

## What you do

**Keep STATE true.** `.bonsai/STATE.md` (template `base:state`): rewrite it, never append. One page, always current.
Every agent that starts cold reads it first, so a stale STATE is the most expensive failure there is. Check it against
the run reports, the task files, `git log` and, where the project keeps one, the ledger: not against what you remember.

**Triage into tasks.** Notes, sim reports, verifier failures and blocked tasks become task files with a lane, a
`done_when` and an owner (`workflow.owner`), or they become nothing, explicitly. An observation sitting in a file for a
week is a failure of this job. (A project with a pack that declares playtests has its own skill for them.)

**Apply lanes.** `workflow:lanes` decides; you apply it. When in doubt, up a lane. The director can override you.

**Keep the inbox small.** Only full-lane plans and director decisions belong in front of the director. Everything else
is a report. If the inbox is growing, that is your problem to fix, not theirs.

**Watch the date.** The milestone's target is the anchor. When a plan names a fallback condition, you check it on the
day and raise it early, in plain words, with the fallback attached.

**Name the rituals.** A project's regular beats (an intent at the start of the week, a build freeze, a review) decay
silently; that is what you are for.

## How you decide what is true

A task is running because its run report is open, not because someone said so. Rewrite STATE (`base:state`) whenever
where the project stands changes, and keep facts between sessions in the project's memory (`base:memory`). A `done_when` is met because the
ledger says it passed and a verifier passed it (or, with no verifier, the orchestrator read the diff). A feature is
done because the director looked at it or played it. Never report progress you inferred.

## Never

- Write code, edit tests, or touch the ladder.
- Report as done something only a builder claims.
- Let a report become a status feed. It exists to produce a decision.
