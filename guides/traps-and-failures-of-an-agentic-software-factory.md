---
title: "Implementation, traps and failures of the so-called AI software factory"
digest: "An AI software factory is a setup where coding agents take work from a queue, build it, review it and ship it with little human help. We run one. It works, on the right work. Most of what went wrong was not the code the agents wrote. It was everything around the code - which version actually ran, whether a check proved anything, and whether the factory's own records still matched reality."
author: GuildBuild research
published: 2026-10-22
updated: 2026-10-22
section: measured
series: Measured guides
sourceId: KN-12
tags:
  - software-factory
  - operations
  - fleet
---

An AI software factory is a setup where coding agents take work from a queue, build it, review it and ship it with
little human help. We run one. It works, on the right work. Most of what went wrong was not the code the agents wrote.
It was everything around the code: which version actually ran, whether a check proved anything, and whether the
factory's own records still matched reality.

Below: what the term means, how ours is built, eight traps in the design, two failure records with their numbers, and
when a factory is worth its cost. Everything here is dated October 2026.

## The short answer

1. A software factory earns its cost on long, multi-step, consequential work: changes that cross sessions, owners or a
   release. On a small local fix, its extra steps cost more than they save.
2. Merged is not deployed, and deployed is not accepted. Our most instructive case was a correct fix whose first check
   ran on code that never had it.
3. The factory's own records become work. Every record its agents keep has to agree with Git, the build and the live
   system. That costs effort even when every agent behaves.
4. Two reviewers are not two opinions. Model errors are correlated, so independence has to come from different
   evidence, not just a different agent profile.
5. Count the right unit: an accepted outcome, priced across every attempt, failures included.

## What is a software factory?

The term is older than AI. Hitachi's own history says its Software Factory split from its Kanagawa Works in 1969, as
the world's first establishment dedicated to software ([Hitachi, in Japanese](https://www.hitachi.co.jp/Div/soft/history.html)).
Michael Cusumano at MIT wrote it up in 1987 as the pioneer of the factory model for large-scale software
([MIT Sloan working paper 1886-87](https://dspace.mit.edu/handle/1721.1/48050)). That factory was people, process and
reuse.

The 2026 version puts agents on the line. StrongDM's AI team describes its factory as "non-interactive development
where specs + scenarios drive agents that write code, run harnesses, and converge without human review"
([StrongDM, 6 February 2026](https://factory.strongdm.ai/)). Two of its rules: "Code must not be written by humans"
and "Code must not be reviewed by humans". The same page lists five other teams building factories.

So "software factory" now covers anything from a prompt loop to a governed delivery system. That is why we say
so-called. Our working definition: a loop that takes an admitted problem to an observed outcome, with a named owner at
every step.

## How ours is built

Ours runs on OpenAI's Codex. The investigators who check its results run on Claude Code, outside its chain of command.
It has 26 named roles, and a job uses only the ones it needs.

What makes it a factory, and not a group of coding personas, is the chain:

1. Intake reconciles; it does not count notifications. Edits and deletions are captured, and missing data stays
   marked unknown, with an owner.
2. Every assignment names the actor, the duty, the inputs, what it may write, the evidence it owes, how it will be
   accepted and when it stops.
3. Review binds to exact bytes. The verdict names a hash. Change the bytes and the verdict is void, along with any
   grant that rested on it.
4. Review stops after two rounds. An objection still open goes to management. It never turns into permission to merge.
5. Publishing, merging, releasing and accepting each have their own owner and receipt. Releasing means tracing the
   reviewed source to the code actually loaded, then watching it run.
6. A checkpoint carries the goal, the limits, the holds and the grants already spent. A fresh session gets no fresh
   authority just because the old context is gone.
7. Hooks load context; configured permissions enforce. Injected instructions are not a sandbox
   ([real guards versus prose](/guides/real-guards-versus-prose/)).
8. Outside investigators check claims against run records and data. They cannot change code, push, merge or write to
   the database.

A design review of this setup, written by one of our own agents, found it strongest exactly there: the chain from an
admitted problem to a checked outcome. It also found the traps below.

## Eight traps in the design

Each of these can bite while every agent follows every rule.

### 1. The records become the work

The factory tracks facts that also live in Git, the build, the deploy and the live sessions. Its assignments,
checkpoints, verdicts and grants must keep agreeing with all of them. When several agents summarize the same moving
state, the record looks tidy and lags reality. What helps: one authoritative history of transitions, with every other
view derived from it.

### 2. Every gate is a queue

Serial gates add waiting even when everyone performs perfectly. Naming more specialists does not make a required step
finish sooner. What helps: sort each transition into judgment, independent evaluation, authorization, deterministic
validation or notification. Only the first three need a model. Do not pay a model to reread the history and announce
the obvious next step.

### 3. Structure has an expiry date

Anthropic's engineers put it well: "every component in a harness encodes an assumption about what the model can't do
on its own, and those assumptions are worth stress testing, both because they may be incorrect, and because they can
quickly go stale as models improve" ([Anthropic, 24 March 2026](https://www.anthropic.com/engineering/harness-design-long-running-apps)).
They dropped their sprint structure once a newer model no longer needed it. They judged their evaluator "worth the
cost when the task sits beyond what the current model does reliably solo". What helps: after each model upgrade,
remove one control at a time and measure.

### 4. More agents, more of the wrong work

In Anthropic's 2026 experiments, swarms of agents each built a game. One variant gave them prescriptive roles; another
named a CEO. "But these prompts did not make much difference"
([Anthropic, 13 August 2026](https://www.anthropic.com/research/multiagent-systems)). A study of 260 agent
configurations found multi-agent setups ranging from 80.8% better than a single agent on decomposable financial
reasoning to 70.0% worse on sequential planning ([Yubin Kim and others, arXiv 2512.08296](https://arxiv.org/abs/2512.08296)).
Capacity also amplifies whatever work gets admitted: closed issues can climb while useful coverage stays flat. What
helps: add a role only for a duty that brings expertise, independent evidence, authority or recovery. Track the
outcome the work is for, not the closures.

### 5. Safe stopping turns into silent waiting

No blind resends, no automatic resume, no unauthorized reassignment: all sensible. But then progress depends on an
event arriving, and on an owner noticing when it does not. Every agent can obey every rule while an issue waits
forever for a lost message. We saw it: work sat still because a session held an older, narrower grant and needed a
fresh one from the human. What helps: make overdue owned work visible. Measure how long work sits dormant, not only
how many unsafe actions were stopped.

### 6. A frozen test can freeze the wrong target

Freezing the acceptance contract stops the goalposts moving. It cannot prove the goal was right. The author and the
reviewer can share one wrong reading of the request. StrongDM met the cheap version: "return true is a great way to
pass narrowly written tests". Their answer is scenarios kept outside the codebase, like a holdout set. What helps:
before freezing, a short adversarial step that asks what important failure would still pass.

### 7. Two reviewers can share one blind spot

A separate reviewer gives procedural independence. It does not give independent mistakes. A study of more than 350
models found that on one leaderboard dataset, "models agree 60% of the time when both models err", and that "larger
and more accurate models have highly correlated errors, even with distinct architectures and providers"
([Elliot Kim and others, ICML 2025](https://arxiv.org/abs/2506.07962)). What helps: give the reviewer the original
request, the exact candidate and the raw evidence, never the builder's summary. Seed known defects and count how many
review misses.

### 8. The glue becomes a product

Hooks, wrappers, role settings and loaders sit on a runtime that changes often. A change to compaction or to the
permission schema can break an assumption without changing a word of the operating text. What helps: a small
compatibility suite, and the control configuration versioned with every run.

## What failed: one fix, three tries to prove it

In late September 2026 a scheduled data-collection job ran 29 tasks, and 26 failed.

| Stage | What we saw | Lesson |
|---|---|---|
| Diagnosis | An investigator outside the build chain read each task's own log. 23 tasks had stopped on purpose because a source refused access. 1 returned a partial result. 1 hit a timeout inside the scheduler. 1 raised an error before writing its first journal row. | Four causes, not one. The first diagnosis was too broad. |
| The fix | A deliberate stop shared an exit code with a failure, so the scheduler painted it red. The fix gave it its own code, shown as skipped. Partial and failed results stayed failures. | A small, precise defect. |
| After merge | The next scheduled run failed 27 of 29. The deployed revision held neither commit, so the run said nothing about the fix. It was rightly thrown out as evidence. | Merged is not deployed. Nobody could see who would deploy, or when. |
| First count after deploy | A query's time window spanned runs on the old code and the new. | Every observation names the revision it saw. |
| Acceptance | A run on the named deployed revision: 23 skipped, 3 succeeded, 3 failed. The 23 skips matched the 23 deliberate stops, one for one. | Accept the narrow claim, with the revision and the window named. |

It was not a one-off. Elsewhere, two reviewed pull requests merged after a run's image was pinned, so that run used
older code.

Then the uncomfortable part. Twenty-three skipped is not twenty-three done. The fix made the dashboard honest; it did
not make the data complete. The rule we run by: a skipped task is failed coverage, never success. Correct refusal and
useful output are different metrics, and a factory has to show both.

## What an audit found before a move

On 7 October 2026 we audited a rebuilt copy of the factory before letting it replace the running one. None of this is
a live outage. These are traps a move would have sprung.

1. Copying the folder did not move the factory. All 26 role profiles and their hooks still pointed at the old
   location. An unchanged hook, started from the new folder, exited successfully and loaded the old instructions.
2. Old files survive an overlay. Of 166 tracked files compared, 93 existed only in the old tree, including an
   instruction file that would still load.
3. A config that parses is not a permission that holds. All 27 settings files parsed. That said nothing about what
   the runtime enforced. One role profile lacked a deny rule the other 25 carried.
4. The tested code was not the running code. Six of seven files compared differed or were missing between the source
   we tested and the copy mounted in the scheduler.
5. Git is not a backup of a factory. The folder holding assignments, checkpoints and receipts, about 157 MB, was
   ignored by Git. A clone would keep the code and lose who owns what.
6. A green health check proved little. It listed the workflows. It never asked whether the scheduler was alive or the
   output fresh.
7. `finally` does not run when a process dies hard. A maintenance step took a lock file and removed it in `finally`.
   The audit ended the process abruptly while it held the lock. The next two runs both failed on the leftover lock.
   No data was lost; maintenance stopped.
8. Instructions rot. A scan found 82 broken local links across role cards, memory, playbooks and records.

## When is a software factory worth it?

| Work | Best route | Why |
|---|---|---|
| A tiny local edit or script | A plain coding agent | Coordination costs more than the work |
| Repeated moderate fixes in a stable repository | A thin layer: one assignment, proportionate review, explicit acceptance | Test whether extra routing earns its keep |
| Ambiguous work across sessions and owners | The full factory | Ownership, limits and evidence survive interruptions |
| A production change with separate merge and release owners | The full factory | Exact identity and observed release are the point |
| Many independent parallel tasks | It depends | Shared state and review queues set the speed |
| Low-volume work with nobody to maintain the process | Not the full factory | Upkeep can exceed the attention it saves |

These are design judgments, not measured rankings.

To price it, price the accepted outcome. Add up every attempt in the batch, failures and abandoned branches included:
review, retries, compute and human minutes. Divide by the outcomes actually accepted. Report the cost per attempted
task too, so a system that rarely finishes cannot hide behind a small denominator. A flat subscription is not free:
it spends capacity even when no invoice line appears ([what a multi-agent fleet costs](/guides/multi-agent-fleet-cost-anatomy/)).

## What to do with it

1. Write down the unit before you build: an accepted outcome, not a merged pull request.
2. Make every check name the revision it observed.
3. Keep acceptance out of the builder's reach. Cursor's approval agent, for one, will not use a pull request's own
   change to an approval policy "to relax review requirements for that same PR" ([Cursor docs](https://cursor.com/docs/approval-agents)).
4. Count skips as failed coverage.
5. End your own processes abruptly on purpose, restart twice, and see what is left behind.
6. After each model upgrade, remove one control at a time and measure.
7. Give your reviewer raw evidence, not the builder's story.
8. Move a factory by a staged release with a map of every path, never by copying folders.

## How we know

1. One factory, one owner, September and October 2026, on Codex, with investigators on Claude Code.
2. The design review was written by one of our own agents, from 18 of the factory's files, one production case and
   public sources. It ran none of the factory's scripts or tests and no matched trial. Its verdicts are design
   judgments, not measurements.
3. The production case is one issue and one fix, 24 and 25 September 2026. Its numbers were checked against the
   issue's own comments on 7 October 2026. Raw row-level joins were not retrieved.
4. The second merge-before-pin case and the stalled grant are single cases from the factory's own records.
5. The audit of 7 October 2026 ran offline experiments on a copy. Its findings block a move. They do not show a
   failing live service.
6. Public sources were read on their own pages on 7 October 2026. The claim to be the world's first is Hitachi's
   own. The MIT paper was seen as a catalogue record, not read.
7. Not measured: speed, or cost per accepted outcome against a plain coding agent. We have not run that trial.

## Questions people ask

### What is an AI software factory?

A setup where agents take work from intake to release with little human help: they build, test, review and ship,
while a human sets direction and acceptance. The name goes back to 1969, when Hitachi gave it to a software unit run
on process and reuse.

### Do AI software factories work?

On the right work, yes, in our experience. Ours carries multi-step changes with owners, limits and evidence that
survive interruptions. Public research points the same way: results depend on matching the structure to the task.

### Should humans review code in a software factory?

StrongDM's rules say no. We keep a human on direction and acceptance, and put investigators outside the build chain
on the results. Who reviews matters less than what they see. A reviewer who reads only the builder's summary inherits
the builder's blind spot.

### What went wrong for us?

Less the code than the evidence around it. The version that ran was not the version reviewed. A count mixed two
versions. A health check could not see a stalled scheduler. Records drifted from reality.

## Sources

1. Hitachi, software business history (Japanese): https://www.hitachi.co.jp/Div/soft/history.html
2. Michael A. Cusumano, Hitachi: pioneering the "factory" model for large-scale software development, MIT Sloan
   working paper 1886-87, 1987: https://dspace.mit.edu/handle/1721.1/48050
3. Justin McCarthy, Software Factories And The Agentic Moment, StrongDM AI, 6 February 2026: https://factory.strongdm.ai/
4. Prithvi Rajasekaran, Harness design for long-running application development, Anthropic, 24 March 2026:
   https://www.anthropic.com/engineering/harness-design-long-running-apps
5. Anthropic Frontier Red Team, Patterns and problems in emerging multiagent systems, 13 August 2026:
   https://www.anthropic.com/research/multiagent-systems
6. Yubin Kim and others, Towards a Science of Scaling Agent Systems, arXiv 2512.08296, version 3, April 2026:
   https://arxiv.org/abs/2512.08296
7. Elliot Kim, Avi Garg, Kenny Peng and Nikhil Garg, Correlated Errors in Large Language Models, ICML 2025:
   https://arxiv.org/abs/2506.07962
8. Cursor documentation, approval agents: https://cursor.com/docs/approval-agents
