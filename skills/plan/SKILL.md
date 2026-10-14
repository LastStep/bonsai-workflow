---
name: plan
description: "The plan template: how one task will be built, its first part in plain words for the person to approve, the rest for the builder and the verifier. Every field, what it may hold, and the template to copy. Use it to write or check a plan."
---

# The plan template

The template of the document kind `plan`, which this pack declares in `bonsai/pack.yaml` (its folder, id pattern,
statuses and moves). A plan says how one task will be built. The full lane needs one, approved by the person, before
code; the orchestrator writes it, its only writer, in the Plan pipeline (`workflow:lanes`).

**Where it goes:** one file per plan, named after its id (`P-T-1234.md`), in the folder the project's `bonsai.yaml`
gives the kind `plan` under `documents` (`work/plans` unless the project changed it). Copy the template below, fill
every field as the table says, and keep only the pointer comment on the first line: the docs stay here.

| Field | Meaning | Allowed values | Example |
|---|---|---|---|
| `id` | The plan's id: `P-` and its task's id, so one plan per task; the file is named after it | the pattern `^P-T-[0-9]{4,6}$` in `bonsai/pack.yaml` | `P-T-1234` |
| `task` | The task the plan is for, by its id | a task's id | `T-1234` |
| `status` | Where the plan stands; a new plan starts at the first status. A person moves draft to approved or rejected; agents move rejected to draft and approved to superseded | `draft`, `approved`, `rejected`, `superseded` | `draft` |
| `author` | The role that wrote it | a role in `bonsai/pack.yaml`, the orchestrator's as a rule | `orchestrator` |
| `created` | The day the plan was started | a date, `YYYY-MM-DD` | `2026-10-16` |
| `approved` | The day the person approved it, set by that move | a date, or `null` until then | `null` |
| `labels` | The plan's labels, by name | a mapping of label names to values; `{}` for none | `{}` |
| `labels.workflow.feature` | The one-pager the plan's task belongs to (defined in `bonsai/labels.yaml`) | a one-pager's id, `F-` and a slug, or left out | `F-room-codes` |

```markdown
---
id: P-T-0000   # a plan; every field and how to fill it: skill workflow:plan
task: T-0000
status: draft
author: orchestrator
created: <YYYY-MM-DD>
approved: null
labels:
  workflow.feature: F-<slug>
---

# Plan: <the task's title>

Two readers. **The person** reads the first part only (down to Size) and reads no code: it is in plain words. The
**builder and verifier** read the rest, and it may carry code where it helps: paths, signatures, short snippets, exact
commands. The person writes, reviews and verifies no code, so the agents own it end to end.

## For the person (plain words)

**What this will do.** One paragraph, in plain words.

**Choices that are yours.** Only a real alternative that the person decides, or "none". A director-lane choice means
this plan does not go ahead: it becomes an options write-up instead.

**Risk.** What could go wrong, and the point at which the task stops and reports blocked.

**Size.** The estimated session length, and anything that must be set up first (a running server, an editor open).

## For the builder and verifier

### Approach

The order of work, as steps. Each step says what exists afterwards that did not before, and names the files,
signatures or commands that make it so.

1. ...
2. ...

### Choices made, and what lost

Any technical choice with a real alternative.

| Choice | Alternative | Why this one |
|---|---|---|

### How it gets proved

Which ladder rungs, plus any new test or check this adds. Map each `done_when` to the thing that will show it.

| Done-when | Proved by |
|---|---|

### Risk in the code

What could go wrong, what the fallback is, and the point at which the task should stop and report blocked rather than
push on.
```
