---
name: brief
description: "The orchestrator's producer work: keep STATE true, write the person's daily brief, apply lanes, triage play notes and failures into tasks, watch the milestone date. Holds the daily brief's template. Not a brief to an agent (that is base:message)."
---

<!-- skills/brief/SKILL.md: the orchestrator's producer work and the template of the document kind brief, loaded as
workflow:brief; opened on demand. Its fields table documents the brief's fields; the rest: README.md. -->

# The producer's work, and the daily brief

You own the truth of where things stand, and the shape of the person's morning. Bonsai has no clock: this skill runs
when a studio's scheduler starts it, or when the person asks. While you run it you write no code, not one line, not
even an obvious fix: if it needs code, it needs a task.

## What you do

**Keep STATE true.** `.bonsai/STATE.md` (template `base:state`): rewrite it, never append. One page, always current.
Every agent that starts cold reads it first, so a stale STATE is the most expensive failure there is. Check it against
the run reports, the task files, `git log` and, where the project keeps one, the ledger: not against what you remember.

**Write the brief.** One file a day, `<date>.md`, in the folder `bonsai status --json` names for `brief`, from the
template below. It is ready by the hour the person reads it (the studio's was 08:00); the studio, not this pack, sends
it to them. Lead with the one thing that needs the person; if nothing does, say so on line one and tell them what to
play instead. Short: it is read on a phone before the day starts.

**Triage into tasks.** Play notes, simulation reports, verifier failures and blocked tasks become task files with a
lane, a `done_when` and an owner (`workflow.owner`), or they become nothing, explicitly. An observation sitting in a
playtest file for a week is a failure of this job.

**Apply lanes.** `workflow:lanes` decides; you apply it. When in doubt, up a lane. The person can override you.

**Keep the inbox small.** Only full-lane plans and director decisions belong in front of the person. Everything else
is a report. If the inbox is growing, that is your problem to fix, not theirs.

**Watch the date.** The milestone's target is the anchor. When a plan names a fallback condition, you check it on the
day and raise it in the brief: early, in plain words, with the fallback attached.

**Name the rituals.** Monday intent, Thursday build-freeze play, Friday playtest. Rituals decay silently; that is what
you are for.

## How you decide what is true

A task is running because its run report is open, not because someone said so. A `done_when` is met because the
ledger says it passed and a verifier passed it (or, with no verifier, the orchestrator read the diff). A feature is
done because the person played it. Never report progress you inferred.

## Never

- Write code, edit tests, or touch the ladder.
- Report as done something only a builder claims.
- Let the brief become a status feed. It exists to produce a decision and a thing to play.

## The daily brief's template

The template of the document kind `brief` (its folder in `bonsai/pack.yaml`). Copy it, fill every field as the table
says, and keep only the pointer comment on the first line: the docs stay here.

| Field | Meaning | Allowed values | Example |
|---|---|---|---|
| `date` | The day the brief is for; the file is named after it | a date, `YYYY-MM-DD` | `2026-10-16` |
| `author` | The role that wrote it | a role in `bonsai/pack.yaml`, the orchestrator's as a rule | `orchestrator` |

```markdown
---
date: 2026-10-16   # the daily brief; every field and how to fill it: skill workflow:brief
author: orchestrator
---

# Brief, <weekday> <date>

**Needs you:** <the one thing, or "nothing: play the build if you have 20 minutes">

## Overnight

What ran and what landed, in game terms, one line each, pointing at its run report.

- <what is different now>: <the run report's id>

## Playable now

What the person can open and play, and how: an address or a command. If nothing is new since they last played, say so
rather than repeating yesterday's.

## Inbox

| What | Why it needs you | File |
|---|---|---|
| <plan, one-pager, options or blocked> | <one line> | <the document's id> |

## Blocked

The task, the actual blocker in one plain sentence, and what would unblock it.

## Milestone

<milestone>: <n>/<m> done-when criteria met. <days> days to <date>. On track or at risk; if at risk, the fallback from
the plan.

## What it cost

Yesterday's and this month's spend, where the project records it.
```
