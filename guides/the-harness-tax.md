---
title: "What you pay before your first word"
digest: "Before you type anything, a Claude Code session has already sent tokens - the harness's own prompt and the description of every tool it carries. You pay that on the first call and again, from cache, on every call after it. We call it the harness tax, and we measured its floor."
author: GuildBuild research
published: 2026-10-22
updated: 2026-10-22
section: measured
series: Measured guides
sourceId: KN-05
tags:
  - cost
  - context
  - harness
---

Before you type anything, a Claude Code session has already sent tokens: the harness's own prompt and the description
of every tool it carries. You pay that on the first call and again, from cache, on every call after it. We call it
the harness tax, and we measured its floor.

## The floor

We built a bare agent profile: no instruction file, every built-in tool denied, skills off, memory off, every switch we could
find turned off. What is left is what the harness alone weighs.

| Bare agent profile's first call | Tokens |
|---|---|
| Terminal, Claude Code 2.1.282 | 1,884 |
| Terminal, Claude Code 2.1.283 | 1,887 |
| Desktop app, 2.1.282 | 8,633 |

The terminal floor is small. The desktop app's is more than four times larger, and 58 percent of it is the app's own
servers. No setting we found reaches them.

## What each tool costs

Then we let the built-in tools back in, one at a time, on the command line.

| Tool | Tokens at start |
|---|---|
| PowerShell | 3,369 |
| Grep | 1,269 |
| Bash | 1,069 |
| Read | 569 |
| Edit | 369 |
| Glob | 269 |
| Write | 269 |

PowerShell costs more than Bash, Grep and Read together. It is on by default only on Windows, so a Windows user pays
it without choosing it. A deny rule takes a tool out of the start.

Thirty-six of the 44 built-in tools priced zero on that start. They are deferred until called, already denied in that
agent profile's settings, or absent from a command-line session.

## The tax moves without notice

Until one day our worker agent profiles woke at 56k to 67k tokens. From that day on they woke at 20k to 25k.

| Seat | Before | After |
|---|---|---|
| Worker 1 | 66,130 | 24,209 |
| Worker 2 | 67,427 | 23,484 |
| Worker 3 | 57,562 | 23,769 |
| Worker 4 | 56,203 | 23,149 |
| Worker 5 | 58,447 | 21,221 |
| The leader | 60,106 | 39,815 |

Different instruction files, the same step, the same day. So the cause sits in a shared layer. Three things changed
together: a harness release, our own settings, and the model. We have not pinned which one it was.

That is the lesson. A start can halve or double under you. If you do not measure it, you find out from the bill.

## Your own block is a second tax

The harness is not the only thing loaded on every call. So is whatever you make resident.

An earlier fleet of ours carried a 58 KB shared instruction file, about 47,000 tokens, loaded by every agent profile. Every
agent in it could still work. By its owner's account, and by the bill, the fleet froze on cost.

The arithmetic is start tokens times agent profiles times turns. A large block does not fail on capability. It fails on the
plan.

## What to do with it

1. Measure a bare start on your own machine, on the instrument you use: terminal and app are different numbers.
2. A agent profile's own weight is its start minus that floor, at the same version.
3. Deny the tools a agent profile never uses. On Windows, start with PowerShell.
4. Re-measure at each release. The floor is a per-version number.
5. Before adding to a resident file, multiply it by agent profiles and turns.

## How we know

1. One Windows 10 machine, late September 2026. The numbers will differ on a Mac or on Linux: PowerShell alone
   changes the start.
2. The floor was measured at two versions. It held, 1,884 and 1,887.
3. Tool prices come from the harness's own context report, good to about 50 tokens each.
4. The wake table is 117 sessions. The step's cause is still open.
5. The frozen fleet is its owner's judgment, with the bill as the instrument. It was not a controlled test.
