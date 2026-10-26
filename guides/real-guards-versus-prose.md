---
title: "Your agent read the rule. Did it follow it?"
digest: "Every team that runs coding agents writes rules for them. Don't touch that folder. Commit through this tool. Never paste a script into the shell. We wanted to know which of those rules hold, so we measured it three ways."
author: GuildBuild research
published: 2026-10-22
updated: 2026-10-22
section: measured
series: Measured guides
sourceId: KN-01
tags:
  - rules
  - hooks
  - measurement
---

Every team that runs coding agents writes rules for them. Don't touch that folder. Commit through this tool. Never
paste a script into the shell. We wanted to know which of those rules hold, so we measured it three ways.

The short answer: a rule the agent holds at start is followed. A rule it has to go and find depends on how hard it is
thinking. A rule backed by a hook holds whether the agent remembers it or not. And a deny rule is weaker than it looks.

## A rule the agent has to find

Most setups keep a short instruction file loaded at start and route the rest through an index: one line that says
when to open which note.

We built a test of 20 questions. Each is a situation that never names a file. Eight need a memory note, four need a
rule reached only through an index line, three a skill, three a script, and two test rules already loaded. We ran it
on three models at three effort levels, one run each.

| Strict score of 20 | Opus 5.5 | Fable 5.1 | Sonnet 5 |
|---|---|---|---|
| medium | 13 | 11 | 6 |
| high | 16 | 16 | 13 |
| xhigh | 17 | 16 | 12 |

The routed rules are the sharp part. Pooled across the three models, a rule reached only through an index line fired
4 of 12 times at medium effort and 9 of 12 at high. The loaded rules held in every run. Sonnet at medium answered 12
of the 20 without looking anything up.

So effort decides more than the model does, and high is where lookup stops improving.

## "Did it open the note" is the wrong score

Then we looked at real work: 31 sessions of one leader agent profile. For every note whose trigger names a file or a tool, we
asked whether the agent profile opened the note before acting.

It did so 3 to 5 percent of the time: 19 of 675 moments by a loose rule, 18 of 398 by a stricter one.

That reads like a disaster. It is not. When we read the misses, most were not misses. Either the trigger was narrower
than the file it named, or the lesson already lived somewhere else. One note says to commit through a particular
tool. The agent profile "missed" that note 53 times, and all 53 were commits made through that tool. The lesson had moved into
the procedure.

A team's lessons reach an agent through scripts, guards and the instruction file as often as through notes. Score the
result, and keep note-opening as a second column.

## A sentence is a request. A hook is a guard.

We are not alone here. A public audit of 481 CLAUDE.md files ([arXiv 2608.23550](https://arxiv.org/abs/2608.23550))
found that only about 4 to 16 percent of the security rules it retrieved had a matching built-in control. The rest
were sentences.

We put one sentence behind a hook and counted what the hook caught. The rule: no inline scripts in the shell. In one
24-hour session the guard refused 41 of 968 shell calls, 4.2 percent. Across ten large sessions the rate sat between
3.6 and 5.1 percent in nine of them.

1. It was habit, not intent. 31 of the 41 were the agent clipping output with `head` or `tail` to save context. Two
   clips need two pipes, and the second pipe is what the guard refuses.
2. The rule was loaded the whole time. The agent profile's own note says a pipeline "feels like a lookup, not a script".
3. A large context did not drive it. The rate was highest in the first calls after a start or a compaction, when the
   agent looks around.
4. The human's push mostly did not drive it either. 25 of the 41 came half an hour or more after his last message.
5. A refusal teaches nothing that lasts. The rate in the five calls after a refusal was the same as everywhere else.

The price was small: 41 extra round trips, 2.0 percent of the session's calls.

## What a guard does not hold

A permission deny matches the text of a tool call. With a shell, the same effect has many spellings. In a test the
human asked for, a short script reached a file that a deny rule named. A second attempt was stopped by the harness's
own classifier.

One more edge is documented: a command hook that times out does not block the call
([hooks reference](https://code.claude.com/docs/en/hooks)). A guard has to decide fast.

What holds a secret is the agent's conduct or a real boundary: operating system permissions, another account, a vault.

## What to do with it

1. Keep the rules that must always apply few, and load them at start.
2. If a rule must hold, put a hook behind it. Then look at what the hook refuses: it shows you a habit.
3. Run routed rules at high effort or above.
4. Protect secrets with a boundary, not a pattern.

## How we know

1. The recall test: late September 2026, headless Claude Code, one run per model and effort. A gap of one or two
   questions between single runs is noise.
2. The observed sessions are one agent profile's. The 3 to 5 percent is a floor on note-opening, not a recall rate, and "most
   misses were not misses" is our reading, with one case counted.
3. The guard counts are one agent profile and one hook, over six days.
4. The audit is someone else's work. We read its abstract, not the paper.
5. Everything ran on one Windows machine.
