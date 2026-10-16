---
name: lanes
description: "Lanes and authority: the three lanes (light, full, director), the pipeline each runs (Plan, Build, Decide), what an agent decides alone, and how a decision reaches the director. Read it before you start a task or decide whether something is yours to decide."
---

<!-- skills/lanes/SKILL.md: workflow's lanes and their pipelines, loaded as workflow:lanes; preloaded by the
orchestrator and the builder. The lanes' two rules Bonsai checks are in bonsai/lanes.yaml. Fields: README.md. -->

# Lanes and authority

Every task runs in one of three lanes, its `lane:` field (their rules Bonsai checks: `bonsai/lanes.yaml`). The
orchestrator assigns the lane when it writes the task; the director can override it; an agent that finds itself in the
wrong lane says so and stops.

## The three lanes

| Lane | What it is | Gate before code | Gate before landing |
|---|---|---|---|
| **light** | Bug fixes, tweaks, refactors, test additions, doc updates. Roughly: under about 200 changed lines, no new wire or data format, nothing a player notices | none | the ladder green; a fresh verifier only for big or risky work (below), else the orchestrator's read of the diff |
| **full** | A feature: anything a player will notice, anything that changes the wire format, the data schema or the server's contract | a one-pager approved by the director, then a plan approved by the director | the ladder green and a verifier; then the director plays it |
| **director** | A decision, not a task (below) | none: the write-up is the work | the director chooses from the write-up |

When in doubt between light and full, it is full. When in doubt between full and director, it is director.

**A fresh verifier for big or risky work only:** guards, the ladder's proof logic, tokens and security, deploys, a
public contract, and the full and director lanes. Related light tasks share one verifier. The rest close on a green
ladder and the orchestrator's read of the diff, and the run report says which.

## The pipelines, one per lane

- **Plan** (the full lane, before Build; skipped when the change fits in one sentence): the orchestrator scopes the
  question; an auditor (the project) and a researcher (the web) look side by side, and get follow-up briefs if needed;
  The director gets questions one at a time, each with two to four options and a recommendation, never one a file
  answers; the orchestrator writes the plan (`workflow:plan`), its only writer, its notes in one file; a verifier's
  facts lens checks it, every blocking finding pointing at evidence; the director approves it.
- **Build** (the light lane, and the full lane once its plan is approved): the task; a builder, calling an auditor or
  a runner as it needs; the ladder green; verifier lenses by risk (tests and correctness for code; QA for what a person
  sees; security for what could be attacked), or for light work the orchestrator's read of the diff; the orchestrator
  lands it; the director looks, if it is something they see.
- **Decide** (the director lane): a researcher and an auditor; two to four options with one recommended
  (`workflow:options`); the director chooses; it is recorded (`workflow:decision`).

Where the director comes in: questions while a plan is made; approving the one-pager and the plan; the choice in
Decide; the look or the play at the end, and closing a full or director task. Everything else is a report.

## The director lane: what stops and becomes a write-up

The director owns these, and nobody else touches them:
- **The pillars and the anti-pillars:** what the game is, and what it refuses to be.
- **Design and feel:** a new rule, a mechanic, a number that changes how it plays, UX, art direction, pacing.
- **Scope and priorities:** what gets built next, what gets cut.
- **Any irreversible technical choice:** engine, service, dependency, hosting, data format, anything architecture-level,
  anything that would cost a week to undo.
- **The fun verdict.** Only the director plays and says whether it is good. No agent may conclude this from a sim, a metric or
  its own judgement.

The output is an options write-up (`workflow:options`): two to four options, honest trade-offs, a recommendation, and
what it would cost to reverse each one. Not a recommendation dressed as the only option.

## What an agent decides alone

- How to carry out an approved plan.
- Small technical choices inside a task: a helper's shape, a data structure, a test's name. Record one
  (`workflow:adr`) only if a future agent would otherwise argue it again.
- Whether to land: when the ladder is green and the verifier passed it (or, for light work that needs no verifier, the
  orchestrator read the diff and said so in the run report).
- What to do when blocked: stop and report `blocked`. Never improvise round a blocker in the director lane.

## How a decision reaches the director

As a file, never as a question buried in chat:

| What | The file | The director sees |
|---|---|---|
| A new feature | a one-pager (kind `feature`), at `draft` | the one-pager to approve |
| How to build it | a plan (kind `plan`), at `draft` | the plan to approve |
| A choice | an options write-up (kind `options`), at `open` | the options |
| Blocked | the task, at `blocked` | the blocker, with the full error |

Observe before you ask: a question whose answer a throwaway run can show is settled by a spike first, and the director
is given its output as the evidence for the options (`workflow:prototype`). What a spike cannot settle (taste,
priorities, money, risk the director owns) is still asked.

While the director is not in the session, a question goes through `bonsai ask`. Keep the inbox small: only full-lane
plans and director decisions belong there. Everything else is a report, not a request: the director reading fewer things is
what keeps them reading properly.
