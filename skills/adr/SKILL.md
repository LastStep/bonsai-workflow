---
name: adr
description: "Decision records: when a technical choice deserves one (so the same argument does not happen twice), when it does not, where it goes and its shape. Read it when a choice is made that a future agent could argue again."
---

<!-- skills/adr/SKILL.md: workflow's protocol for decision records, loaded as workflow:adr; preloaded by the researcher.
The record's template is workflow:decision. Fields: README.md. -->

# Decision records

A decision record exists to stop the same argument happening twice. That is its whole purpose, and it is the test for
whether to write one.

## Write one when

- A future agent, reading the code cold, would reasonably propose the opposite and be wrong.
- The choice was between real alternatives, and the loser is still tempting.
- It constrains future work: a data format, a protocol's shape, a dependency, a layering rule.
- Rohan chose from an options write-up. The record says what he chose and why the others lost.

## Do not write one when

- There was no real alternative.
- It is a naming or style choice.
- It is already stated in the design page or in the project's `CLAUDE.md`.
- You are recording that you did the obvious thing.

Ceremony is the failure here. Ten records nobody reads are worse than three that everybody does.

## Where

| About | Where it goes |
|---|---|
| The project's code, data, protocol or architecture, or its tooling, process and agents | a decision record (kind `decision`, template `workflow:decision`), in the folder `bonsai status --json` names for it; or the project's own decision log, where it keeps one |
| Game design: a rule, a mechanic, a number | the design page's decision log, **not** a record |

Numbers are sequential and never reused. A record starts `proposed`; Rohan accepts it. A superseded record is
not deleted or rewritten: it moves to `superseded`, names the record that replaced it, and stays.

## Shape

Context (what forced the choice), Options (what was really considered, with the trade-off that decided it), Decision,
Consequences, including what this now makes hard. The template is `workflow:decision`.

Four sentences in the right place beat two pages in the wrong one.
