---
name: prototype
description: "Observe, don't ask: when a question is a run's to answer, a throwaway spike settles it before the person is asked. Who writes it, where it lives, its variants behind one switch, what it measures, where the result goes. Open it before you ask the person which approach, look or number is better and a program could show you."
---

<!-- skills/prototype/SKILL.md: workflow's protocol for a throwaway spike, loaded as workflow:prototype; opened by the
orchestrator when it classifies a question, and by the builder it briefs. Its output goes into an options write-up
(workflow:options). Fields: README.md. -->

# Prototype to answer, not to ask

A spike is a small program written to be thrown away. Its output is evidence for a decision; it is never code that lands.
The rule behind it: **if the answer is a fact you could observe by running something, it is not the person's to answer.**
Run it, then bring the person what you saw.

## When a question is a run's to answer

Classify the question before you ask it.

| A run's to answer | Not a run's to answer |
|---|---|
| Which of two approaches is smaller, faster or simpler, measured | Taste: which one the person likes the look of (a spike may show them the options; they choose) |
| Whether a library does what its page says, on this project's own data | Priorities: what to build next, what to cut |
| What a design looks like with real data, drawn five ways, so the person picks from pictures | Money, and any risk the person owns |
| Whether a limit holds (a size, a time, a count) | The pillars, the feel, the fun: only the person says |

Where it is both (a look the person judges, a size they weigh), the spike shows the look and measures the size, and the
person decides with both in front of them. A spike informs a decision; it never makes one that is the person's.

## Who does it, and where

- **A builder writes the spike.** A runner, auditor, researcher or verifier changes no file and cannot. A runner may
  **run** a spike that is written (long runs, timings, many variants).
- **Where:** a scratch folder outside the repository, one per task, such as `<scratch>/<task>/spike/`, with its own
  module or project file. Never inside the repository, never committed, never on a branch. In a studio's own words the
  folder is named in its brief.
- **Its life:** throw it away, or keep it as scratch so a decision can be re-checked. Never port it into the product.
  If the answer says to build the thing, that is a task with its own plan, written fresh.

## The steps

1. **Write the question** as one sentence, and the decision it feeds. If you cannot name the decision, the question is
   not ready.
2. **Check it is a run's** with the table above. If it is not, stop: ask the person (`workflow:lanes`).
3. **List the variants** (two to five) and what each must show. A variant is a different answer to the question, not a
   different amount of polish.
4. **Make the scratch folder** and write the spike there, so every variant is picked by **one switch** (a flag or
   an argument, `--variant a`). Every output then comes from the same program, on the same data.
5. **Use real data.** The project's own files, output or fixtures, copied or read in place. Never invented sample data:
   an invented input answers a question nobody asked.
6. **Measure beside the look,** wherever the decision weighs it: sizes, times, counts (dependencies, binary size, start
   time, lines). Take them from the built program, not from an estimate. Write the command that produced each.
7. **Run every variant** and keep the raw output. Every image or capture shown to the person comes from one of these
   real runs, never drawn by hand or redrawn.
8. **Put the result where the decision is made:** in the options write-up (`workflow:options`) as each option's evidence,
   or on the page the person reads. Show the variants side by side, name the switch value under each, and give the
   measurements in one table.
9. **Finish clean.** Stop every process the spike started and check none remain. Say whether the folder is thrown away
   or kept, and where.

## The shape of the report

Your report to your caller holds: the question and the decision it feeds; the variants and the switch that picks each;
the data used and where it came from; the measurements, each with its command; the paths of the raw outputs; what did
not work (a variant dropped, a library that failed); what you did not run; and whether the folder is thrown away or kept.
Describe what the output shows, never that one variant is better, nicer or fun: the person judges that.

## Never

- Commit the spike, put it in the repository, or build the product from it.
- Invent data, or show an image that did not come from a run.
- Settle a question of taste, priority, money or risk by what the spike measured. Measure, show, and let the person decide.
- Keep a spike running after you stop.
