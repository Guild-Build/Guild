---
title: "The cost curve of a long session, and when to compact"
digest: "A coding session gets more expensive as it goes, and not in a straight line. Every call sends the whole context again. So the cost of reading grows with the square of the turns. The prompt cache makes each re-read cheap. It does not change the shape."
author: GuildBuild research
published: 2026-10-22
updated: 2026-10-22
section: measured
series: Measured guides
sourceId: KN-04
tags:
  - cost
  - cache
  - compaction
---

A coding session gets more expensive as it goes, and not in a straight line. Every call sends the whole context
again. So the cost of reading grows with the square of the turns. The prompt cache makes each re-read cheap. It does
not change the shape.

Two things move the bill: what a cached token costs, and when you compact.

## The prices moved

These are the API prices per million tokens, from
[Anthropic's pricing page](https://platform.claude.com/docs/en/about-claude/pricing).

| | Input | One-hour cache write | Cache read |
|---|---|---|---|
| Opus 5 | $5 | $10 | $0.50 |
| Opus 5.5 | $4 | $8 | $0.20 |

A cache read on Opus 5.5 costs 0.05 times input, where it was 0.1. So the curve is flatter: a long warm session is
cheaper than it was.

The ratio that moved the other way is cold against warm. A one-hour write was 20 times a read. Now it is 40. Missing
the cache costs twice as much, relative to staying warm, as it did.

## A compact pays back at once

We drew the cost curve for nine sessions, 313 calls.

In one session a compaction took the tokens sent per call from 382k to 38k. The saving shows on the very next call.

Then the context refilled, from 38k to 172k in eleven calls. In that session the refill was new work, not re-reading:
twelve web fetches, two searches, a file read, and the model's own long output coming back as input.

The app's "saved" figure overstates the real drop. It reports the context before minus the summary after. The first
call after a compact also carries the fixed prefix, so the real drop was about 29k smaller. We saw the same gap in
three compacts.

In a Codex fleet we read 196 compactions. The median one cut the context by 87 percent.

## Manual or auto matters less than when

We compared sessions by how their stretches ended. One agent profile's manual compacts came earlier than its automatic ones,
and its calls read less cached context. Another agent profile's manual compacts came later, and its calls read more. The
trigger did not decide the cost. The timing did.

A compact is not free. In Claude Code each one cost a wait of 9 to 57 seconds. In Codex it took from about one minute
to about three. A tight automatic window means more waits and a lost thread mid-task.

## Keeping a agent profile warm

Take an idle agent profile holding 100k of context on Opus 5.5.

1. A ping once an hour re-reads the context at the cache price: about 2 cents.
2. Letting it go cold and returning writes the whole context again: about 80 cents.
3. Clearing it and starting fresh from a short handoff costs a small start once.

So cold is never the cheapest. Ping for a while, and clear when the agent profile will be idle for most of a day.

## What to do with it

1. Compact when the work changes, not when the window is full.
2. Judge a compact on the next calls, not on the "saved" figure.
3. Do not let a large context go cold if you will come back within hours.
4. Before a long break, compact first. A cold return then costs a small context.

## How we know

1. Prices were read on the pricing page in October 2026. They are API list prices. How a subscription's limits weigh
   cache reads is not published, and we do not know it.
2. The cost curve is nine sessions of one agent profile, late September 2026. The refill finding is one session.
3. The manual-against-auto comparison is two agent profiles.
4. The Claude Code wait times are one day's compactions. The Codex times are nine compactions in three chats.
5. The keep-warm figures are arithmetic on the list prices, not a measurement.
