---
name: bugs
description: "The bugs register template (document kind bugs): the one file of known bugs someone has seen or proven, open and fixed. Its fields, its tables, and the template to copy. Use it to start the register, or to add or close a bug."
---

# The bugs register template

The template of the document kind `bugs`, which this pack declares in `bonsai/pack.yaml`: one file per project, with
no id and no statuses of its own. The register holds the bugs someone has **seen or proven** and nobody has fixed yet,
and those fixed since. What goes wrong comes first, then where it lives and where it was found.

**Where it goes:** the one file the project's `bonsai.yaml` gives the kind `bugs` under `documents` (`work/bugs.md`
unless the project changed it). Copy the template below once, when the first bug is found, fill every field as the
table says, and keep only the pointer comment on the first line: the docs stay here.

| Field | Meaning | Allowed values | Example |
|---|---|---|---|
| `title` | The register's title | text, one line | `Known bugs` |
| `updated` | The day a row was last added or moved | a date, `YYYY-MM-DD` | `2026-10-16` |

**The rows.** Each bug is a row of the Open table: its id (`BUG-` and three digits, the next free one, never reused),
what goes wrong in plain words, where it lives (a file and the function, with the commit the line was read at: code
moves, so name the function too) and who or what found it (a task, a run report, a playtest). **Adding one:** the next
free id, plain words first, a source someone can open. **Fixing one:** move its row to Fixed with the commit and the
task, never delete it. A design question is not a bug: it goes to the person (`workflow:lanes`).

```markdown
---
title: Known bugs   # the bugs register; every field and how to fill it: skill workflow:bugs
updated: <YYYY-MM-DD>
---

# Known bugs

The defects someone has seen or proven and nobody has fixed yet. How to add or fix one: skill workflow:bugs.

## Open

| Id | What goes wrong | Where | Found by |
|---|---|---|---|
| **BUG-001** | <what goes wrong, in plain words> | <file, function, commit> | <task, run report or playtest> |

## Fixed

| Id | What went wrong | Fixed in | Task |
|---|---|---|---|

## Known, not bugs

Behaviour that looks wrong and is meant, so nobody files it.
```
