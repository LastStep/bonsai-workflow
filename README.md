# workflow, a Bonsai pack

A way of working for AI agents in a software or game project, as a [Bonsai](https://github.com/LastStep/Bonsai) pack:
six roles in two kinds, three lanes each running its pipeline, the protocols every agent follows, and the templates
of the documents they write. It is a Claude Code plugin (its roles and skills) with a `bonsai/` folder (what Bonsai's
engine reads when a project links the pack). It carries no code that runs.

**Where it came from.** It is a game director's way of working: the roles, protocols and templates of their studio as
they stood on 9 Oct 2026, rewritten for Bonsai's formats and commands, and redesigned on 14 Oct 2026 around six roles,
a message format between agents and limits per role. Its prose says "the director" for the person who directs the work, and keeps
their game wording (players, play sessions, "is it fun"): it is a game studio's workflow, and it works the same for
other software. Bonsai's formats call the director `person`.

## What a project gets

- **Six roles** (`agents/`), loaded as `workflow:<role>`. **Leads** own the work: the `orchestrator` (the session the
  person talks to), the `builder` and the `verifier`. **Helpers** are shared, and call no one: the `researcher` (the
  web), the `auditor` (the project's own code, documents and data) and the `runner` (long or mechanical runs).
- **Three lanes and their pipelines** (`skills/lanes/`): light work runs Build; a feature (full) runs Plan, then
  Build; a decision (director) runs Decide.
- **The protocols, as skills**: the session-start checklist every session reads (the one always-on file), and
  `lanes`, `verification-ladder`, `reward-hacking-guards`, `reporting` and `adr`, read when a role needs them.
- **The orchestrator's skills**: `orchestrate` (its work), `brief` (the producer's work and the person's daily brief)
  and `interview` (after a play session). **The verifier's**: `lenses`.
- **Eight document kinds**, each with a documented template: plans, one-pagers (`feature`), decision records, options
  write-ups, specs, playtests, daily briefs and the bugs register.
- **Six labels** (`workflow.owner`, `workflow.model`, `workflow.feature`, `workflow.milestone`, `workflow.spec`,
  `workflow.estimate_h`) and **walls** for a studio's own secret files.

It is meant to be used with `base`, Bonsai's own pack, which holds the templates for Bonsai's documents (a task, a run
report, STATE, a memory note, a message between agents) that these roles write.

## How a project takes it

A first link, with no `bonsai.yaml` yet (a person's command, in the project's checkout):

```sh
bonsai init --name <project> --source https://github.com/LastStep/bonsai-workflow --ref <40-character commit> --yes
```

A project already linked adds the pack as an entry in its `bonsai.yaml`, after `base`:

```yaml
packs:
  - id: workflow                      # the pack
    source: "https://github.com/LastStep/bonsai-workflow.git"   # where it is fetched from
    path: null                        # the pack is at its repository's top
    ref: "<40-character commit>"      # the exact commit
    roles: null                       # this project's limits for the pack's roles; null keeps the pack's
```

then runs `bonsai update`, which previews every change. Because the pack declares roles, a Bonsai that enforces them
writes two guard lines for them (one that also judges the calls that start agents, and one on every other tool): a
change to Bonsai's own hook lines, so the person types `bonsai update --allow-exec --yes` themselves. A first link
writes them with `--yes`.

**Its own limits.** Each role has three limits: `minutes` (since the agent's first call, or since it read its newest
brief), `actions` (tool calls, refused ones counted) and `red_runs` (failed ladder runs in a row). Each has a
`default` (what an agent gets when its brief names none) and a `max` (the most a brief may give it). The pack ships
each max equal to its default, so a brief can lower a limit but not raise it; a project raises a max in its own
`bonsai.yaml` before any brief can give more, at any time (it is not a loosening of the checks):

```yaml
    roles:                            # under the pack's entry
      builder:                        # a role the pack declares
        minutes:                      # minutes since its first call, or since it read its newest brief
          default: 240                # what a builder gets when its brief names none
          max: 480                    # the most a brief may give it
```

`bonsai status --json` lists each role's limits in force and where each came from (`roles`). Claude Code's own stop,
the role file's `maxTurns`, sits above them: a project's max of actions past a role's `maxTurns` meets Claude Code's
stop first, and `bonsai check` warns about it.

## The always-on file

`bonsai/files/session-start.md` is written into the project's protocols folder (`documents.protocols` in its
`bonsai.yaml`, `work/protocols` unless the project changed it) and imported by the block in `CLAUDE.md`, so every
session and every agent reads it. It says: what you are (a session the person talks to is the **orchestrator**, which
opens `workflow:orchestrate` when work starts; a quick question is answered as it is, and "do this one yourself" from
the person is done in that session; a session started as a role is that role; an agent started by a call reads its
brief first), what to read and in what order, the lane, the ground, the run report, and what to do before you stop.
It stays at or under 4.6 KB, its docs one comment line pointing here.

## The roles

| Kind | Roles | May call | Changes files |
|---|---|---|---|
| The person's session | `orchestrator` | any lead or helper | yes: plans, tasks, records |
| Lead | `builder`, `verifier` | helpers only | the builder its task's files; the verifier nothing |
| Helper | `researcher`, `auditor`, `runner` | no one | nothing |

**How each starts.** The orchestrator is a plain `claude` in the project (the always-on file tells it what it is), or
`claude --agent workflow:orchestrator` to take over from another orchestrator or to run an interview in a fresh
session. The orchestrator starts every lead, and never asks one agent to start another lead for it: through the Agent tool
(`subagent_type: workflow:<role>`) with its brief's id in the prompt, or as a named session (`claude -p --agent
workflow:builder` with `BONSAI_TASK` and `BONSAI_ROLE` set). A lead calls its own helpers the same way. Bonsai's guard holds
who may call whom, refuses every file edit to a role that changes nothing, and stops an agent past its limits but for
its report; `skills/reporting/` tells each role what it does then.

**Briefs and reports** between agents are messages, written with `bonsai message` (templates: `base:message`): a
brief says what the recipient lacks, the objective, the output's shape and the boundaries; a report gives a status
(`done`, `blocked`, `needs-input`) and points to files rather than retelling them.

## The three pipelines

`skills/lanes/SKILL.md` holds them in full. In short: **Plan** (the full lane; a change that fits in one sentence
skips it): an auditor and a researcher look, the person answers questions one at a time, the orchestrator writes the
plan, a verifier's facts lens checks it, the person approves. **Build**: the task, a builder (with its helpers), the
ladder green, verifier lenses by risk (light work: the orchestrator reads the diff), landed, and the person looks if
it is something they see. **Decide** (the director lane): a researcher and an auditor, two to four options with one
recommended, the person chooses, a decision record.

## Where each list lives

None is copied here:
- the roles, their kinds and their limits: `bonsai/pack.yaml`, `roles`;
- each role's preloaded skills: its own file's `skills:` line;
- the lanes and their two rules: `bonsai/lanes.yaml`; what lands in each, and the pipelines: `skills/lanes/SKILL.md`;
- the labels: `bonsai/labels.yaml`;
- the document kinds, their folders, ids, statuses and moves: `bonsai/pack.yaml`, `documents`; each kind's fields:
  its template skill's fields table;
- the walls: `bonsai/pack.yaml`, `deny`, each rule with its `why`.

`bonsai check --schema bonsai.pack` (and `bonsai.labels`, `bonsai.lanes`) prints every field of those formats.

## The folder

| File | What it is |
|---|---|
| `.claude-plugin/plugin.json` | The plugin manifest Claude Code reads; documented below, under "plugin.json" |
| `agents/` | The six roles, one file each, loaded as `workflow:<name>` |
| `skills/` | The skills, one folder each with its `SKILL.md`, to the open Agent Skills standard, loaded as `workflow:<name>` |
| `bonsai/pack.yaml` | The Bonsai manifest (format `bonsai.pack/1`): id, version, needs, the always-on file, the walls, the document kinds and the roles with their limits |
| `bonsai/labels.yaml` | The six labels (format `bonsai.labels/1`), in the namespace `workflow` |
| `bonsai/lanes.yaml` | The three lanes (format `bonsai.lanes/1`) |
| `bonsai/block.md` | The pack's part of the instruction block in a project's `CLAUDE.md` |
| `bonsai/files/session-start.md` | The always-on protocol, written into the project's protocols folder |
| `ci/check.sh` | Every check the pack has, in one script; its header documents each subcommand |
| `.github/workflows/pack.yml` | The pack's CI and release, calling `ci/check.sh` |
| `.gitattributes` | Every file is text with LF line endings on every checkout, Windows included |
| `LICENSE` | MIT |
| `README.md` | This page |

## Role and skill files

Each file in `agents/` and `skills/` has one row here, and each row names a file there: `ci/check.sh` fails otherwise.

| File | What it is |
|---|---|
| `agents/orchestrator.md` | The orchestrator: the session the person talks to. Thin: its work is in `skills/orchestrate/` |
| `agents/builder.md` | The builder, a lead: one task in its own worktree, the ladder climbed, a report with its branch's HEAD |
| `agents/verifier.md` | The verifier, a lead: one lens a run, in a fresh context, pass or fail with evidence; fixes nothing |
| `agents/researcher.md` | The researcher, a helper: one question from the web, every claim linked and quoted; the options for a decision |
| `agents/auditor.md` | The auditor, a helper: one scoped question about the project, file-and-line evidence, found kept apart from not found |
| `agents/runner.md` | The runner, a helper: runs what its brief names and returns counts, failing names, exit codes and a log's path |
| `skills/orchestrate/SKILL.md` | The orchestrator's work: the pipelines it runs, briefs and reports, landing, the hand-over past a heavy context |
| `skills/lanes/SKILL.md` | The three lanes, their pipelines, what an agent decides alone, how a decision reaches the person |
| `skills/verification-ladder/SKILL.md` | The ladder: climbing it with `bonsai ladder`, its rules, the three-failures circuit breaker, what done means |
| `skills/reward-hacking-guards/SKILL.md` | What counts as gaming the checks, and what to do instead |
| `skills/reporting/SKILL.md` | Reports to the person and between agents, their statuses, and what to do when a budget is spent or a call refused |
| `skills/adr/SKILL.md` | When a choice deserves a decision record, where it goes and its shape |
| `skills/lenses/SKILL.md` | The verifier's four lenses: tests and correctness, QA, security, facts |
| `skills/brief/SKILL.md` | The producer's work (STATE, the daily brief, triage, lanes, the date), with the template of the kind `brief` |
| `skills/interview/SKILL.md` | The interview after a play session, in a fresh session: its six questions and rules, the write-up, proposed tasks |
| `skills/plan/SKILL.md` | The template of the kind `plan`: how one task will be built |
| `skills/feature/SKILL.md` | The template of the kind `feature`: a one-pager, what a player gets |
| `skills/decision/SKILL.md` | The template of the kind `decision`: a decision record |
| `skills/options/SKILL.md` | The template of the kind `options`: an options write-up for the person's decision |
| `skills/spec/SKILL.md` | The template of the kind `spec`: a design bigger than one feature |
| `skills/playtest/SKILL.md` | The template of the kind `playtest`: one play session's record |
| `skills/bugs/SKILL.md` | The template of the kind `bugs`: the project's register of known bugs |

**What each role preloads**, and why its body names those skills too: each role file lists its skills on its
`skills:` line by the plugin's name (`workflow:lanes`). Measured on Claude Code 2.1.300: a role started as a subagent
through the Agent tool gets them preloaded (the bare `lanes` form loaded too); a session started as
`claude --agent workflow:<role>` gets none, in either form. So each role's body names the skills to open when they are
not already loaded, and the orchestrator's body says it in so many words. The same measurement found `maxTurns`
honoured for a subagent (its caller gets the partial output, marked as cut at the limit) and not applied to an
`--agent` session.

### How each kind of file documents itself

Every file the pack ships says its purpose, when to use it, and every field: its meaning, its allowed values (or where
the list is defined) and an example.

- **A YAML file** (`pack.yaml`, `labels.yaml`, `lanes.yaml`, `pack.yml`): a short header, and a `#` comment on every
  key, at its line's end or on the line just above. `bonsai check --pack` fails a key in `bonsai/` without one.
- **A template skill** (`plan`, `feature`, `decision`, `options`, `spec`, `playtest`, `bugs`, and `brief` after its
  procedure): a fields table, `| Field | Meaning | Allowed values | Example |`, above its template, the first fenced
  block after the table. `bonsai check --pack` holds the table and the template's fields equal, both ways, and each
  allowed-values cell that lists values to the pack's own list (a kind's statuses, the lanes, a choice label's
  values). A document made from a template keeps only the pointer comment on its first line.
- **A role file, and a skill that is no template**: an HTML comment just after the frontmatter, a line or two. The
  body is read into the session, so the comment stays short and the fields are documented once, below.
- **The always-on file and the block**: one comment each (the block's stays in the pack); the rest is on this page.
- **`ci/check.sh`**: its header. **`plugin.json`**: this page.

### Role files: frontmatter fields

| Field | Meaning | Allowed values | Example |
|---|---|---|---|
| `name` | The role's name; the file is `agents/<name>.md`, and the role loads as `workflow:<name>` | a lower-case letter, then lower-case letters, digits and dashes | `builder` |
| `description` | When to use the role, as Claude Code shows it to a caller choosing an agent: short, and distinct from the others' | one line of text, quoted | `"Builds one task in its own worktree, ..."` |
| `model` | Claude Code's model for the role's tier (deep `opus`, standard `sonnet`, fast `haiku`); a caller may name another for one call | a Claude Code model alias, a full model id, or `inherit` | `opus` |
| `skills` | The skills loaded into the role's context when it starts as a subagent, each by the name the plugin loads it as | a list of skill names | `[workflow:lanes, workflow:reporting]` |
| `maxTurns` | Claude Code's limit on the role's turns: a backstop where Bonsai's guard does not run, never the limit itself; the orchestrator has none | a whole number, equal to the role's `max_turns` in `bonsai/pack.yaml` (four times its default actions) | `2400` |

### Skill files: frontmatter fields

| Field | Meaning | Allowed values | Example |
|---|---|---|---|
| `name` | The skill's name; the folder is `skills/<name>/`, and the skill loads as `workflow:<name>` | a lower-case letter, then lower-case letters, digits and dashes | `lanes` |
| `description` | What the skill holds and when to open it: Claude reads it to decide whether to load the skill | one line of text, quoted | `"Lanes and authority: ..."` |

## plugin.json

| Field | Meaning | Allowed values | Example |
|---|---|---|---|
| `name` | The plugin's name, which Claude Code puts before every role and skill (`workflow:builder`) | the pack's id, exactly as `bonsai/pack.yaml`'s `id` | `"workflow"` |
| `description` | What the pack is, in a sentence, as Claude Code's plugin lists show it | text | `"A way of working for Claude Code agents, ..."` |
| `author` | Who makes the pack | an object; `name` is the maker's name | `{"name": "Bonsai"}` |
| `repository` | Where the pack's source lives | a URL | `"https://github.com/LastStep/bonsai-workflow"` |
| `license` | The pack's license | an SPDX license id | `"MIT"` |

**Never add `version`** (below). Hooks, MCP or LSP servers and monitors in `plugin.json` or beside it would be code
that runs, which this pack carries none of.

## The block

`bonsai/block.md` is the pack's part of the instruction block Bonsai writes into a linked project's `CLAUDE.md`, after
the label lines: two lines naming the roles and how each starts, the lanes, and the templates. Every session reads
it, so it stays at most four lines; its own comment stays in the pack. With `base`'s block, the label lines and the
imports, the whole block is 20 lines (measured on a project linked to both; Bonsai's cap is 40); linked alone, 14.

## Document kinds and their folders

`bonsai/pack.yaml`'s `documents` gives each kind a default place under `work/`, which `bonsai init` writes into the
project's `bonsai.yaml`; a project moves any of them there. Options write-ups have a kind of their own, apart from
decision records, since one kind has one list of statuses. **Two kinds in one folder** (a project pointing both
`decision` and `options` at one folder): tried at Bonsai's commit `c641c15` in a scratch project holding a record and
a write-up there, and `bonsai init`, `bonsai check`, `bonsai status` and `bonsai ask --doc` all accepted it; their id
patterns (`ADR-`, `OPT-`) keep the two apart.

Moves follow Bonsai's rule for documents: a person approves (a plan, a one-pager, a spec), accepts (a decision
record), decides (an options write-up), closes and cuts; agents make the rest. Where a project's status moves go
through a command (`status_writes: command` in `bonsai status --json`), agents make theirs through it.

## The walls

`bonsai/pack.yaml`'s `deny` stops Claude's file tools reading a studio's three secret files, `~/.trinetra/token`,
`deploy_ed25519` and `salt`: in the session's own home, in the Windows home seen from WSL (`//mnt/c/Users/*/`), and in
a WSL home seen from Windows (`//wsl.localhost/*/home/*/` and `//wsl$/*/home/*/`). `base` cannot name them, since it
names no studio. The rules name the files; nothing secret is in the pack. A deny rule stops the file tools and simple
shell reads, not a program reading its own file, nor a script. The two forms for a WSL home seen from Windows are
the first to try; each rule is tried on both sides, and one that does not hold is replaced or written here.

## Versions and releases

- **A project pins the pack by commit.** `bonsai.yaml` names a tag or commit; Bonsai's lock records the 40-character
  commit and refuses a tag that later moves. The commit is the version Claude Code sees: with a `version` in
  `plugin.json`, Claude Code would keep each machine on its cached copy whatever the commit, so `plugin.json` never
  carries one, and `claude plugin validate --json` warns about that one thing, which `ci/check.sh` allows.
- **`bonsai/pack.yaml`'s `version` names tags only.** A release is a tag `v` and that version (`v0.1.0`). To release:
  raise `version`, commit, push, then tag that commit `v<version>` and push the tag. The CI checks everything again,
  checks that the tag equals the version, and makes the GitHub release with its notes.
- **Adopting a release is a person's step** in each project: change the ref in `bonsai.yaml` and run `bonsai update`,
  whose preview says what changes.

## It carries no code

Roles, skills and templates are prompts and text, not code: the pack has no hook lines (`hooks: []`), no plugin hooks,
MCP or LSP servers, monitors or mods, and no script a skill runs. So a first link needs `--yes` alone. Bonsai's own
guard lines, which the pack's roles bring, are Bonsai's, not the pack's.

## Checks, CI and release

`ci/check.sh` is the one home of the pack's checks (its header documents each subcommand):

- `sh ci/check.sh check <bonsai> <folder> <source> <commit> [<path>]`: `bonsai check --pack` on the folder (the
  declarations, every comment, each template against its fields table, each role against its file, the block's
  length); this page's rows against `agents/` and `skills/`; a scratch project linked to the pack at the commit, with
  `claude` off the PATH; `bonsai check` there. Run locally from the pack's top with the commit to check (committed
  work only): `sh ci/check.sh check "$(command -v bonsai)" . . "$(git rev-parse HEAD)"`.
- `sh ci/check.sh validate <folder>`: `claude plugin validate --json`, failing on every error and warning but the
  missing `version`. It needs no Claude login, so it runs in CI with no secret.
- `sh ci/check.sh release <tag> <folder>`: fails unless the tag is `v` and `pack.yaml`'s `version`.

`.github/workflows/pack.yml` runs on every push and pull request: `check` on Linux and Windows with a `bonsai` built
from the Bonsai commit it pins (`BONSAI_COMMIT`, one that reads `roles` and the protocols folder); `validate` on Linux
with the Claude Code it pins; then `release`, which checks the tag the version needs on every push and, on a `v*` tag
only, makes the GitHub release. Either pin moves only on purpose, in a commit of its own.

### On Windows

Before a `bonsai update` that installs a plugin, open one Claude Code session first, so its login is fresh: a plugin
install meeting a run-out access token has signed the person out of Claude Code.
