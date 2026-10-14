---
name: spec
description: "The spec template (document kind spec): a design for work bigger than one feature, what it builds on and what it changes, for the person to approve. Every field, what it may hold, and the template to copy. Use it to write or check a spec."
---

# The spec template

The template of the document kind `spec`, which this pack declares in `bonsai/pack.yaml` (its folder, id pattern,
statuses and moves). A spec designs work bigger than one feature: a system, a format, a change of how the project
works. The person approves it; tasks then name it in their `workflow.spec` label, and its parts become one-pagers,
plans and tasks.

**Where it goes:** one file per spec, named after its id (`SPEC-2026-10-16-save-sync.md`), in the folder the
project's `bonsai.yaml` gives the kind `spec` under `documents` (`work/specs` unless the project changed it). Copy the
template below, fill every field as the table says, and keep only the pointer comment on the first line: the docs
stay here.

| Field | Meaning | Allowed values | Example |
|---|---|---|---|
| `id` | The spec's id: `SPEC-`, the day it was started and a short slug; the file is named after it | the pattern `^SPEC-[0-9]{4}-[0-9]{2}-[0-9]{2}-[a-z0-9-]+$` in `bonsai/pack.yaml` | `SPEC-2026-10-16-save-sync` |
| `title` | What the spec designs, in one line | text, one line | `Saves sync between machines` |
| `status` | Where it stands; a new spec starts at the first status, and the person approves it | `draft`, `approved` | `draft` |
| `author` | The role that wrote it | a role in `bonsai/pack.yaml` | `orchestrator` |
| `created` | The day it was started | a date, `YYYY-MM-DD` | `2026-10-16` |
| `approved` | The day the person approved it, set by that move | a date, or `null` until then | `null` |
| `basis` | What it builds on: the documents it starts from, by id | a list of document ids, `[]` for none | `[ADR-0123, OPT-0123]` |

```markdown
---
id: SPEC-<YYYY-MM-DD>-<slug>   # a spec; every field and how to fill it: skill workflow:spec
title: <what the spec designs, in one line>
status: draft
author: orchestrator
created: <YYYY-MM-DD>
approved: null
basis: []
---

# <Title>

## For the person (plain words)

What this makes possible, what it changes for a player or for the way the project works, the choices that are the
person's, and the rough size. No code.

## The design

What is built, part by part: each part's purpose, its inputs and outputs, and the rules it follows.

## What it changes

The documents, formats and parts of the project this changes once approved, each named.

## How it gets proved

How each part will be shown to work: its checks and the evidence a verifier will read.

## Not in this

What a reader will assume is included and is not.
```
