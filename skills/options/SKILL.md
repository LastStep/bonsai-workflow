---
name: options
description: "The options write-up template (document kind options): two to four real options for a decision the director makes, with criteria, costs, reversibility and one recommendation. Every field, what it may hold, and the template to copy. Use it in the director lane."
---

# The options write-up template

The template of the document kind `options`, which this pack declares in `bonsai/pack.yaml` (its folder, id pattern,
statuses and moves). A write-up makes it easy for the director to decide well and quickly, once: it is the director
lane's output (`workflow:lanes`), written by a researcher as its report's body and filed by the orchestrator. The
rules that make it useful are the researcher's (`agents/researcher.md`): criteria before options, two to four real
ones, reversibility, one recommendation and what would change it.

**Where it goes:** one file per write-up, named after its id and a short slug (`OPT-0123-hosting.md`), in the folder
the project's `bonsai.yaml` gives the kind `options` under `documents` (`work/options` unless the project changed
it). Copy the template below, fill every field as the table says, and keep only the pointer comment on the first
line: the docs stay here. Once the director chooses, a decision record (`workflow:decision`) says what and why.

| Field | Meaning | Allowed values | Example |
|---|---|---|---|
| `id` | The write-up's id; the file's name starts with it | `OPT-` and four digits, the pattern `^OPT-[0-9]{4}$` in `bonsai/pack.yaml` | `OPT-0123` |
| `title` | The decision, phrased as a question | text, one line | `Where should the game server run?` |
| `lane` | The lane: a write-up is director-lane work | `light`, `full` or `director`, the pack's lanes (bonsai/lanes.yaml); always director here | `director` |
| `status` | Where it stands; a new write-up starts at the first status. The person decides; agents defer one nothing waits on, and open it again | `open`, `decided`, `deferred` | `open` |
| `author` | The role that wrote it | a role in `bonsai/pack.yaml`, the researcher's as a rule | `researcher` |
| `created` | The day it was started | a date, `YYYY-MM-DD` | `2026-10-16` |
| `decided` | The day the person chose, set by that move | a date, or `null` until then | `null` |
| `chosen` | The option the person chose, by its letter | a letter, or `null` until then | `null` |

```markdown
---
id: OPT-0000   # an options write-up; every field and how to fill it: skill workflow:options
title: <the decision, phrased as a question>
lane: director
status: open
author: researcher
created: <YYYY-MM-DD>
decided: null
chosen: null
---

# <The decision, as a question>

## Why this is being asked now

What forced it. What is blocked until it is answered. If nothing is blocked, this can wait: say so and mark it
deferred.

## What matters here

The two or three criteria that actually decide it, in the director's terms: feel, time to the playtest date, cost, how
hard it is to undo. Named before the options, so the comparison is honest.

## Options

### A. <name>

What it is, in a sentence a non-programmer follows. Then:

- **Gets us:** ...
- **Costs:** time, money, complexity, what it makes harder later.
- **Reversible?** How long it would take to back out, and what would be stranded.

### B. <name>

### C. <name>

## Comparison

| | A | B | C |
|---|---|---|---|
| <criterion> | | | |
| Time to working | | | |
| Cost | | | |
| Reversibility | | | |

## Recommendation

One option, named, with the single reason that decides it. Say what would change the recommendation: that is what
makes it a recommendation rather than a preference.

## What happens next, either way

The first task that follows from each choice, so the decision turns into work at once.

## Sources

Each source with its link and date, and the passage quoted; inference marked as inference.
```
