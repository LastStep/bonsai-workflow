# Changelog

What changed in the workflow pack, release by release. Earlier releases (1.0.0 to 1.0.2) are described in their GitHub
releases. The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [1.1.0] - 2026-11-24

> **Needs base 1.1.0 and Bonsai 1.1.0.** The pack format has no field for needing another pack or a Bonsai version
> (`needs` holds only `claude_code`), so the need is stated here and in README.md. Take Bonsai first, then base, then this
> pack (`bonsai pack set --all`).

### Changed
- **The pack no longer declares a `bugs` document kind.** Bugs are Bonsai's own documents from Bonsai 1.1.0: one file
  each (`bonsai.bug/1`), filed and moved with `bonsai bug`, and a bugs page generated from the files. The kind and its
  hand-kept register template are gone from `bonsai/pack.yaml`; a project on workflow 1.0.2 with Bonsai 1.1.0 still
  works (Bonsai sets the old declaration aside with the warning `kind-set-aside`).
- **`skills/bugs/SKILL.md` is a pointer to `base:bug`**, which holds the bug file's fields, every `bonsai bug`
  subcommand and the steps that move an old hand-kept register to bug files.
- **Every mention of `workflow:bugs` is rewritten:** the block names `bonsai bug` and `base:bug`; the `correct` skill
  reads bugs by `bonsai bug list`. The README counts five document kinds.

### Release
- The pack is at version 1.1.0, and its CI checks with Bonsai 1.1.0's `bonsai` (`BONSAI_VERSION`, its archives
  pinned by SHA-256).
