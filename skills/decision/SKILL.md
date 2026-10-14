---
name: decision
description: "The decision record template (document kind decision): context, options, decision and consequences of one choice, so the same argument does not happen twice. Every field, what it may hold, and the template to copy. Use it to write or check a record."
---

# The decision record template

The template of the document kind `decision`, which this pack declares in `bonsai/pack.yaml` (its folder, id pattern,
statuses and moves). When a choice deserves a record, and when it does not, is in `workflow:adr`.

**Where it goes:** one file per record, named after its id and a short slug (`ADR-0123-plain-yaml.md`), in the folder
the project's `bonsai.yaml` gives the kind `decision` under `documents` (`work/decisions` unless the project changed
it). Numbers are sequential and never reused. Copy the template below, fill every field as the table says, and keep
only the pointer comment on the first line: the docs stay here.

| Field | Meaning | Allowed values | Example |
|---|---|---|---|
| `id` | The record's id; the file's name starts with it | `ADR-` and four digits, the pattern `^ADR-[0-9]{4}$` in `bonsai/pack.yaml` | `ADR-0123` |
| `title` | The decision, as a statement | text, one line | `Settings files are plain YAML` |
| `status` | Where the record stands; a new record starts at the first status. A person accepts it; an agent marks it superseded when a later record replaces it | `proposed`, `accepted`, `superseded` | `proposed` |
| `date` | The day it was accepted, set by that move | a date, or `null` until then | `null` |
| `scope` | What the decision is about: the product (the project's code, data, protocol or architecture) or the process (its tooling, workflow or agents' behaviour) | `product` or `process` | `product` |
| `decided_by` | Who made the choice | `person` or `agent` | `agent` |
| `superseded_by` | The record that replaced this one, once one has | a record's id, or `null` | `null` |

```markdown
---
id: ADR-0000   # a decision record; every field and how to fill it: skill workflow:decision
title: <the decision, as a statement>
status: proposed
date: null
scope: product
decided_by: agent
superseded_by: null
---

# ADR-0000: <title>

## Context

What forced the choice. The constraint, not the preamble.

## Options

What was really considered, and the trade-off that decided between them. One or two lines each.

## Decision

What we do. Present tense, unambiguous.

## Consequences

What this now makes easy, and, more importantly, what it makes hard. Name the thing a future agent will be tempted to
do that this rules out, because that is what stops the argument happening twice.
```
