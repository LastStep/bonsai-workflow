---
name: correct
description: "Turn a mistake seen twice into a check that stops it: the levels to fix it at, highest first, and the rule that a new check must be shown failing on the real past mistake. Run at each part's end, by the orchestrator, from verifier findings, fix rounds, reverts and guard refusals. Open it when the same mistake has come back."
---

<!-- skills/correct/SKILL.md: workflow's protocol for turning repeated mistakes into enforcement, loaded as
workflow:correct; opened by the orchestrator at each part's end. Its output is proposed tasks, never edits. Fields:
README.md. -->

# Correct: a mistake seen twice gets a check

Prose that nothing enforces is the weakest fix: an agent that skips it fails nothing. So a mistake that came back is
fixed where it can no longer happen, or at least where something fails when it does.

## When

- **At each part's end**, by the orchestrator, over the part's records.
- Any time the same mistake is seen a second time.

## The evidence it reads

Only what is recorded, never memory of the work: verifier findings and fix rounds in the run reports, reverts and
"fix" commits in the log of the repository, guard and ladder refusals in the log (`bonsai logs`), bugs in the register.

## The steps

1. **Collect** each mistake the evidence shows: one line, with where it was seen (the run report, the commit, the
   refusal).
2. **Group** the mistakes into classes by cause, not by wording. A class seen once is left alone and listed as
   "seen once", so the next run can count it again.
3. **For each class seen twice or more, choose the highest level that works.** Highest first:
   1. **A schema or a format:** the wrong thing cannot be written (a field that is required, a value from a list).
   2. **A rung or a guard:** the climb or the guard refuses it.
   3. **A check:** `bonsai check` or a script in the project reports it.
   4. **A test:** a test fails when it happens.
   5. **The pack's prose, last,** and only for a judgment call no program can make (a rule of taste, a choice of words).
   Say why each level above the chosen one does not work here.
4. **Prove the check on the real past mistake.** A new check is accepted only once it has been shown to fail on the
   mistake as it was made: the old commit or the old input, run through the check, and its failing output pasted. A
   check that was only ever seen passing proves nothing. If the old mistake cannot be rebuilt, say so, and the check is
   proposed as "unproven" with that stated.
5. **Write the proposals as tasks for the orchestrator** (`base:task`): for each class, the mistake, the evidence, the
   level chosen, the check, the failing run that proves it, and what it may change. The orchestrator decides which
   become tasks; a change to a gate (`bonsai.yaml`, a guard, the ladder) goes by the project's own rule for gates,
   never as a side effect.

## Output

A list of proposals, one block per class; then the classes seen once; then the mistakes you could not class. Nothing is
edited, and no check is added by this skill: it proposes, the orchestrator tasks, a builder builds.

## Never

- Add or loosen a check to make a run green. This skill only ever tightens.
- Propose prose as the fix when a higher level works.
- Accept a check that was not shown failing on the real mistake.
- Class a mistake from one occurrence.
