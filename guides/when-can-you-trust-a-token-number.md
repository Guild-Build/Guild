---
title: "When can you trust a token number?"
digest: "People post token numbers for coding agents all the time. 'My start is 20k.' 'This tool costs 3k.' Most of those numbers are true on one machine on one day. We learned that about our own numbers, and then we found two records of the same session that disagree by a factor of sixteen."
author: GuildBuild research
published: 2026-10-22
updated: 2026-10-22
section: measured
series: Measured guides
sourceId: KN-08
tags:
  - measurement
  - tokens
  - method
---

People post token numbers for coding agents all the time. "My start is 20k." "This tool costs 3k." Most of those
numbers are true on one machine on one day. We learned that about our own numbers, and then we found two records of
the same session that disagree by a factor of sixteen.

## Six conditions

A token number holds only under the conditions it was measured in. We now write six beside every one.

1. **Release.** The harness ships about once a weekday. A start can change overnight.
2. **Operating system.** One built-in tool costs 3,369 tokens and is on by default only on Windows.
3. **Instrument.** Terminal, desktop app and cloud are different starts. A bare agent profile is 1,884 tokens in the terminal
   and 8,633 in the app.
4. **Trust state of the folder.** An untrusted folder ignores allow rules and keeps deny rules, hooks and
   environment.
5. **Account kind.** Two accounts on one release can receive different prompts.
6. **Model.** In one folder, one model's start carried about 6,000 more characters than another's.

Every number we hold today was measured on one Windows machine. We say so, and we do not call a number reproduced
until a second machine on a second system has run it.

One thing does travel: blocks. Totals differ between machines. The size of a given tool or file is far more stable.
Compare two setups block by block.

## Why the instrument matters

The desktop app has a usage panel. The harness also writes a log of every call, with the usage the API returned. They
are two records of the same calls, kept by the same product.

We compared them on two sessions.

| | Panel | Log |
|---|---|---|
| Session 1 (Opus 5.5), cache read | 152.8M | 152,565,400 |
| Session 1, output | 28.4k | 461,837 |
| Session 2 (Fable 5.1), cache read | 5.3M | 5,253,398 |
| Session 2, output | 3.6k | 84,821 |

Input agrees. Output does not, by a factor of 16 in one session and 23 in the other.

We checked our side first.

1. Double counting. Our sum takes one value per message. In the second session, taking the first, last, largest or
   smallest record per message gives the same 84,821.
2. Plausibility. In both sessions the panel's output is below what was visibly written, counting text and tool input
   together. The second session wrote about 150,000 characters; the panel shows 3.6k tokens.
3. Speed. The panel's own clock gives about 70 tokens a second from the log's figure in both sessions. From the
   panel's figure it would be 3 or 4.

We do not know the cause. Two guesses failed: the panel is not counting only the final call of each turn, and it is
not counting text alone.

## What it does not mean

It does not mean anyone was overcharged. Billing is Anthropic's.
[The docs](https://code.claude.com/docs/en/costs) say Claude Code charges by API token consumption, and that the
session figure is a local estimate for API users, with the Console as the authority. For a subscriber, the panel is
not a bill at all.

It means this: if you tune an agent by watching that output figure, you may be tuning against a number that is many
times too small.

## What to do with it

1. Write the six conditions beside every number you publish.
2. Name the instrument and the counting rule: which record, one value per what.
3. When two instruments disagree, check your own side first, then say that the cause is unknown rather than guess.
4. Take output from the log.

## How we know

1. Two sessions, two models, one day in late September 2026, one machine.
2. The gap has not been re-checked on the current release. Do that before relying on it.
3. Other holes can exist in general: a resumed or forked session can copy history into a new file, and side threads
   keep their own records.
4. The list of six is ours. We tested Windows only; Linux and macOS are untested.
