---
name: lenses
description: "The verifier's four lenses (tests and correctness, QA, security, facts): for each, what it reads, what it runs itself, what a blocking finding needs and what it returns. Read the lens your brief names before you start."
---

<!-- skills/lenses/SKILL.md: the verifier's lenses, loaded as workflow:lenses; preloaded by the verifier. Fields:
README.md. -->

# The four lenses

One lens a run, the one your brief names. In every lens: you start from the files, never from anyone's account of the
work; you run the checks yourself; **a blocking finding points at evidence** (a file and line, a command and its
output, a source and its quote, or a concrete failing case), and an opinion is never blocking; blocking findings are
kept apart from risks worth knowing; your verdict cites its evidence first, then says pass or fail. A reviewer told to
find gaps finds some even in sound work: if the evidence holds, pass it.

## Tests and correctness (always, for code)

**Reads:** the task (its `done_when` is the contract); the one-pager (what "correct" means to a player); the approved
plan, and whether the run report declares a deviation from it; the diff, all of it; the ladder's result
(`.bonsai/local/ladder/<task>.json`), not the run report's claims about it.
**Runs:** the ladder itself (`bonsai ladder --task <id> --json`) at the branch's HEAD, and any test a `done_when`
rests on; each `done_when` checked one by one. A CI result (`mode: ci`) is never a task's proof.
**Fail it when:**
- a `done_when` is not demonstrably true, or is true only by the builder's assertion;
- a rung is claimed green that the result does not confirm, or the result is for other code;
- a test was weakened, skipped, deleted or narrowed (look for this in the diff on purpose);
- the ledger, where the project keeps one, changed other than an entry's pass;
- behaviour changed that the one-pager does not cover, however sensible it looks;
- the test count went down, the build got bigger or a sim invariant loosened, with no stated reason;
- the code special-cases the test, the seed or the fixture;
- a player-visible change has no evidence: no screenshot, no URL, nothing to look at.
**Returns:** the verdict; each reason with its evidence; on a fail, precisely what would make it pass, one list.

## QA (for work a person sees)

**Reads first:** the project's QA notes, memory notes the orchestrator adds whenever the person's own look disagreed
with a QA verdict: they calibrate you to the person's judgment. Then the one-pager and the task.
**Runs:** the thing itself, as a person would: in a browser or in the game, never through a unit test or a request
alone. Follow the one-pager's steps in order, then the obvious mistakes a new player makes.
**Blocking finding:** the steps that reproduce it and a screenshot (saved outside the project, pointed to). A problem
you saw is reported, never argued away as "not a big deal": the person decides how much it matters. Say what you could
not see (a native dialog, sound, feel).
**Returns:** the verdict; for each finding its steps, its screenshot's path and what you expected instead.

## Security (adversarial)

**Reads:** the diff, and the inputs, files, commands and permissions it touches.
**Runs:** attempts to break it on purpose: hostile input, paths that climb out, a missing or forged file, a refused
call worked round, a secret printed or written, a check that fails open.
**Blocking finding:** a concrete case that breaks it, with the command or input and what happened.
**Returns:** the verdict, and every case you tried, broken or not, so the next reader knows what was covered.

## Facts (for plans)

**Reads:** the plan, and every source it cites.
**Runs:** each factual claim re-checked against its source: the code (`path:line`, read, not guessed), the document,
or the search re-run. A claim about what exists today is checked in the code, not in a design.
**Blocking finding:** a claim the source contradicts, with the source and its quote, the file and line, or the
failing case; or a step that cannot be checked as written. A preference about the plan's approach is not a finding.
**Returns:** the verdict; blocking findings, each with its evidence; then the claims you checked that held. At most two
rounds on one plan, each by a fresh verifier; the planner fixes the plan or shows the evidence, never argues the
verdict. The orchestrator counts your blocking findings, and which of them held, in the run report.
