---
title: "Where the tokens go in a real agent team"
digest: "When a team of agents burns through its allowance, the first guess is always the same - they wrote too much. We read two real fleets call by call. The output was never the story. Almost everything an agent team pays for is reading its own context again."
author: GuildBuild research
published: 2026-10-22
updated: 2026-10-22
section: measured
series: Measured guides
sourceId: KN-02
tags:
  - cost
  - tokens
  - fleet
---

When a team of agents burns through its allowance, the first guess is always the same: they wrote too much. We read
two real fleets call by call. The output was never the story. Almost everything an agent team pays for is reading its
own context again.

## A Codex fleet, 28,600 calls

One fleet of 20 sessions made 28,600 calls.

| | Tokens |
|---|---|
| Input, all | 3,675,632,277 |
| of which read from cache | 3,605,212,416 |
| Fresh input | 70,419,861 |
| Output | 13,854,848 |

98.1 percent of the input was cache reads. Output was under half a percent of what moved.

That is what a long session is. Every call sends the whole conversation again. The cache makes each re-read cheap, and
the count of re-reads makes the bill.

The load was not spread evenly. In the first count of that fleet, six of the 20 chats carried about three quarters of
the fresh input plus output. The leader alone carried 22 percent, and sent 696 of the 2,167 messages between agents.
A leader that sits in every loop pays for every loop.

## A Claude team, eight subagents

Subagents look like free parallel hands. We ran the same small task two ways: one agent profile alone, and eight subagents.

The eight cost 2.4 times the weighted tokens and took 3.6 times the wall clock. The solo agent profile's output was slightly
better.

The cause was one thing. The team had a 58 KB shared instruction file, and every spawned subagent booted all of it:
about 47,000 tokens each, before any work.

In one earlier run the same pattern went wide: 31 subagents fanned out over about 189 repositories and used about 70
percent of a weekly allowance in 39 minutes.

Even so, across that fleet subagents were only 4.3 percent of tokens. The bigger cost was still the main threads
re-reading long sessions. Subagents are ruinous when the fan-out is wide and the shared block is big. Shrink the
block and the answer changes.

## Skills, the quiet line

Each skill's name and description load at every start. That is the cost. The benefit is a skill being called.

Across 2,743 tool calls in six sessions, one called a skill.

How a description gets a skill triggered is a public 650-trial study's work, not ours. Ours is only the count: on one
team, the shelf was carried on every call and used once.

## What it means

1. Look at reads first. A cost review that starts from output is looking at half a percent.
2. Long sessions are the main line. Compact them, or hand off.
3. Keep the leader out of routine loops.
4. Price a subagent as the shared block plus its work, and cap the fan-out.
5. Count skill calls against skills carried.

## How we know

1. The Codex numbers are whole-call counters from the fleet's own logs, September 2026. The 98.1 percent is from the
   corrected count; the six-chat share is from the first count, before the correction.
2. The subagent test is one task, 8 lines long, run once each way.
3. The skill count is one team's six sessions.
4. These counts show where tokens went. They do not show what was waste: that needs the job each call was doing.
