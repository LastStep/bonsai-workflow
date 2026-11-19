---
name: bugs
description: "Where bugs went: from Bonsai 1.1.0 a bug is Bonsai's own document, one file each, filed and moved with bonsai bug, and its template is base's bug skill (base:bug). Open it only to be sent there."
---

<!-- skills/bugs/SKILL.md (workflow:bugs): a pointer, no template. This pack declared a document kind `bugs`, one
hand-kept register file, until its release 1.0.2; from Bonsai 1.1.0 and base 1.1.0 the kinds `bug` (one file per bug)
and `bugs` (the generated page) are Bonsai's own, so the pack no longer declares one and holds no template for it. The
skill stays so that a session that still names `workflow:bugs` finds where the text went. Frontmatter fields:
README.md, "Skill files". -->

# Bugs are Bonsai's own documents

Open **`base:bug`**. It holds the bug file's fields and an example, every `bonsai bug` subcommand, the generated bugs
page, and the steps that move an old hand-kept register (the one this pack's `bugs` template made) to bug files.

In short, and only to point the way:
- **File a bug:** `bonsai bug add --title "<what goes wrong>" --by <role>@<task>`; **move it:** `bonsai bug fix <id>
  --commit <sha>`, `wontfix`, `watch` or `reopen`. **`bonsai bug` is the one way a bug's status changes**, in every
  project; never edit a status line or the generated page by hand.
- **A design question is not a bug:** it goes to the person (`workflow:lanes`).
- **A project with a register kept by hand** is left as it is until its own agent moves it by `base:bug`'s last section;
  `bonsai check` names it with the warning `bugs-not-moved`.

Needs Bonsai 1.1.0 and base 1.1.0 (`bonsai --version`).
