---
title: "What an effort level buys, and where it stops paying"
digest: "Claude Code lets you set how hard the model thinks - medium, high, xhigh, max. Most people pick one and never look again. We ran one fixed job at every level on three models and read what each step bought."
author: GuildBuild research
published: 2026-10-22
updated: 2026-10-22
section: measured
series: Measured guides
sourceId: KN-03
tags:
  - cost
  - effort
  - measurement
---

Claude Code lets you set how hard the model thinks: medium, high, xhigh, max. Most people pick one and never look
again. We ran one fixed job at every level on three models and read what each step bought.

On that job, the cost ran about 20 times from the cheapest setting to the dearest. For looking things up, everything
past high bought nothing.

## What effort does to the output

The job was a 20-question lookup test. We counted how much of each model's output was thinking.

| Share of output that is thinking | medium | high | xhigh | max |
|---|---|---|---|---|
| Opus 5.5 | 17% | 29% | 50% | 79% |
| Fable 5.1 | 13% | 20% | 37% | 58% |
| Sonnet 5 | 21% | 35% | 60% | 74% |

Effort moves the share in steps, in every model, and never out of order.

Measured as thinking tokens per visible token, each step multiplied the ratio by 1.4 to 3.7, most often 2 to 3. It is
closer to a log scale than a straight line.

The visible answer grows too, but far less. On Opus, the visible part went from 617 tokens a question at medium to
1,680 at max, 2.7 times. The thinking went from 125 to 6,184, 49 times.

## What it costs

At list price, the same 20 questions cost $0.44 on Sonnet at medium and $8.54 on Opus at max.

For scale, we also grew an agent's instruction file to the size of a team leader's. That added about 8,000 tokens to
a 27,000-token start. Tuning the file is small money. Model and effort are the big levers.

## Where it stops paying, for lookup

The test scores whether the agent found the right rule or note.

1. Medium is too low. Rules reached through an index mostly did not fire.
2. High is the knee. Two of three models scored 16 of 20.
3. xhigh found no more, at 48 to 87 percent more cost.
4. max found no more than high, at 2.1 to 6.2 times high's cost.

## What this does not say

It does not say high is enough for hard thinking. This test measures finding, not judging.

We ran one small judgment test: six questions, twice each, two models at three efforts, 72 answers graded blind by
two graders. Every comparison tied. But the test had a ceiling: one grader gave 29 of the 72 a perfect score. A tie
under a ceiling is not proof of no difference.

Quality by effort is Anthropic's own study, on a far larger sample
([their engineering blog](https://claude.dev/blog/spending-your-effort)). Read that for quality. Read this for cost
and lookup.

One more thing we believe and did not measure: the kind of job moves spend as much as the setting does. That is field
experience.

## What to do with it

1. Name the job before the setting. A lookup job and a judgment job want different effort.
2. Do not run a agent profile that relies on routed rules below high.
3. Before paying for max on a routine job, run your own job at high and at max and compare.
4. Fix model and effort before you trim instruction files.

## How we know

1. One run per model and effort, late September 2026. A gap of one or two questions between single runs is noise.
2. One job family: questions on one agent's memory store.
3. Costs are the harness's list-price field. A subscription draws usage, not dollars.
4. The thinking shares were read from session transcripts. The stream files cut each call's output count short, so
   we did not use them.
