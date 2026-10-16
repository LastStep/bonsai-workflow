---
name: changelog
description: "Write a changelog for a range of work, from the project's own records: the range (commits, tags or dates), four forms (text, visual, short, plain words for the person), the sources in order, every line traced, and where it is written. Open it when the person asks what changed, or a release or a part closes."
---

<!-- skills/changelog/SKILL.md: workflow's changelog protocol, loaded as workflow:changelog; opened by whoever the
person asks, usually the orchestrator or a builder it briefs. It is a skill only, not a document kind. Fields:
README.md. -->

# Changelog

What changed, for someone who was not there, written from the project's records and nothing else.

## The range

A range is named before any line is written, by **commits** (`a1b2c3d..e4f5a6b`), **tags** (`v1.2.0..v1.3.0`) or
**dates** (`2026-10-01..2026-10-15`). If the person names none, take the end of the existing changelog's last range up
to now. The range is written in the file (below).

## Sources, in this order

1. The tasks **done** in the range (title, `done_when`).
2. Their commits (`git log` over the range).
3. Run reports and decisions of those tasks.
4. The ladder results of those tasks, for any number you quote.

A line comes from the first source that holds it. Earlier sources win over later.

## The rules for every form

- **Every line traces to a task or a commit in the range.** Put the id or short hash at the line's end. A line that
  traces to nothing is deleted, however true it seems.
- **Nothing private:** no person's name, home path, machine name, address or secret. Write `~/...` for a home path.
  Read the finished file for these before you stop.
- **No adjective you cannot defend.** Numbers and what a person can now do.
- **Images are checked:** each one comes from a real run, shows no private text, and its before and after are the same
  view, size and data.
- **Do not judge:** say what changed, never that it is better, nicer or fun.

## The four forms

Ask which the person wants; with none named, write **text**.

**Text** (the Keep a Changelog layout): newest range on top, one heading per range, entries under Added, Changed,
Fixed, Removed, Security (only the ones used).

```markdown
# Changelog

## [Unreleased] - range: a1b2c3d..e4f5a6b (2026-10-01 to 2026-10-15)

### Added
- `bonsai status` prints a short summary of the active task (T-0412)

### Fixed
- `bonsai check` no longer fails on a path with spaces (T-0417, 9f8e7d6)
```

**Visual:** one section per change a person sees, with a before image and an after image side by side, the same view
for both, a caption of one line, the task or commit.

```markdown
### Status screen (T-0412)
| Before | After |
|---|---|
| ![before](images/status-before.png) | ![after](images/status-after.png) |
Lists the active task first; the log moved below it.
```

**Short:** at most seven lines, each a sentence a person reads in five seconds, most important first.

```markdown
- Status shows the active task first (T-0412).
- A path with spaces no longer breaks check (T-0417).
```

**Plain words for the person:** no ids, command names or jargon; what they can do now that they could not, or what no
longer goes wrong. The ids live in the text form; this form links to it.

```markdown
- You can now see what you are working on the moment you open the status screen.
- A folder name with a space in it no longer stops the check.
```

## Where it is written

Where the person names, else `CHANGELOG.md` at the project's top. If the file exists, **continue it**: read its last
range line, start the new range there, and add the new section above the older ones, keeping everything below as it is.
Each section opens with its range as `range: <from>..<to>`.

## Never

- Write a line the range's records do not show.
- Rewrite or reorder an older section.
- Publish, tag or push: this skill writes a file.
