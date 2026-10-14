---
name: feature
description: "The one-pager template (document kind feature): what a player gets from a feature, in their words, for Rohan to approve before any plan. Every field, what it may hold, and the template to copy. Use it to write or check a one-pager."
---

# The one-pager template

The template of the document kind `feature`, which this pack declares in `bonsai/pack.yaml` (its folder, id pattern,
statuses and moves). A one-pager says what a player gets from a feature, in plain words. In the full lane Rohan
approves it before any plan is written; its `done_when` lines become the tasks' `done_when`, and each task names it in
its `workflow.feature` label.

**Where it goes:** one file per one-pager, named after its id (`F-room-codes.md`), in the folder the project's
`bonsai.yaml` gives the kind `feature` under `documents` (`work/features` unless the project changed it). Copy the
template below, fill every field as the table says, and keep only the pointer comment on the first line: the docs
stay here.

| Field | Meaning | Allowed values | Example |
|---|---|---|---|
| `id` | The one-pager's id; the file is named after it | `F-` and a short slug, the pattern `^F-[a-z0-9-]+$` in `bonsai/pack.yaml` | `F-room-codes` |
| `title` | What a player gets, in their words | text, one line | `Join a friend's room with a code` |
| `status` | Where it stands; a new one-pager starts at the first status. A person approves it, closes it once played, and may cut it; agents move it through building and verified | `draft`, `approved`, `building`, `verified`, `done`, `cut` | `draft` |
| `lane` | The lane its work runs in: a feature is full-lane work | `light`, `full` or `director`, the pack's lanes (bonsai/lanes.yaml); always full here | `full` |
| `design_anchor` | The section of the project's design page this feature's rules live in, by the name or anchor the project uses | text, or `none yet` | `#room-codes` |
| `created` | The day it was started | a date, `YYYY-MM-DD` | `2026-10-16` |
| `approved` | The day the person approved it, set by that move | a date, or `null` until then | `null` |
| `labels` | The one-pager's labels, by name | a mapping of label names to values; `{}` for none | `{}` |
| `labels.workflow.milestone` | The milestone it counts toward (defined in `bonsai/labels.yaml`) | the project's name for a milestone, at most 60 characters, or left out | `M2` |

```markdown
---
id: F-<slug>   # a one-pager; every field and how to fill it: skill workflow:feature
title: <what a player gets, in their words>
status: draft
lane: full
design_anchor: none yet
created: <YYYY-MM-DD>
approved: null
labels:
  workflow.milestone: <milestone>
---

# <Title>

**One sentence:** what a player can do after this that they cannot do now.

## Why now

Two or three sentences. What it unblocks, or what is broken without it. If the honest answer is "it would be nice",
this is not ready to be approved.

## What the player sees

Describe it as an experience, in order, as if watching over someone's shoulder. No class names, no file paths, no
"the system will". This is the part Rohan approves.

## Rules it introduces or changes

Cite the design page's section for each. If a rule is not in the design page, it does not exist yet: add it there as
proposed and say so here. **Never invent a rule in a one-pager.**

## Done when

Checkable statements, each one either true or false. These become the tasks' `done_when` (and, where the project
keeps one, its ledger's entries). Write them so a verifier who did not build it can tell.

- [ ] <observable, checkable>
- [ ] <observable, checkable>

## Not in this

What a reader will assume is included and is not. Be specific: this list is what stops scope creep during the build.

## Open questions for Rohan

- <question, with a recommendation if you have one>

## Cost

Rough size (hours or sessions), and which ladder rungs it will need.
```
