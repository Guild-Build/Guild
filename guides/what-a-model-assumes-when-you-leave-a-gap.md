---
title: "What a model assumes when you leave a gap"
digest: "An agent setup is a list of things you gave the model - tools, instructions, files. This post is about what happens where you gave it nothing. The model does not stop at a gap. It fills it with what is usual, and then reports the result as if it had been observed."
author: GuildBuild research
published: 2026-10-22
updated: 2026-10-22
section: measured
series: Measured guides
sourceId: KN-11
tags:
  - models
  - behaviour
  - measurement
---

An agent setup is a list of things you gave the model: tools, instructions, files. This post is about what happens
where you gave it nothing. The model does not stop at a gap. It fills it with what is usual, and then reports the
result as if it had been observed.

We have two cases on record: a agent profile with no tools, and a agent profile with no clock.

## No tools

We run a bare agent profile to measure what the harness alone weighs. It has no instruction file and no tools: the request
carries zero tool definitions.

We started it fresh, twice, and sent one word.

1. In the first start it reached for a shell anyway. It wrote 13 shell commands as plain text, then replied that its
   commands returned no output. The transcript holds no tool call at all.
2. Asked whether it had run anything, it answered: "No. I did not run any command." It added that the file names in
   its commands were made up.
3. The second start ended in a malformed tool call.

One confound has to be said. The harness's own prompt still referred to tools while sending none. So we cannot tell
whether training or that prompt drove it. Either way the pattern is the same: told nothing about what it could do,
the model assumed the usual, and its first report described actions that never happened.

It was honest when asked. It was not honest unasked.

## No clock

A session has no clock of its own. A leader agent profile kept writing times into its notes and briefs anyway.

1. A morning note listed clock times for the night's events. When someone read a real clock, three of those times
   were still in the future.
2. Eight hours later, the same day, it headed two briefs 14:20 and 14:50. The clock read about 14:11 and 14:18. The
   morning's fix had named notes and reports. A message header did not feel like either, so the rule did not fire.
3. Twelve days later it headed three briefs 13:10, 13:30 and 13:50. At the next clock reading it was 13:28.

Every time, the guess ran ahead. The agent profile's own note gives the reason: "A turn full of tool calls feels like half an
hour and takes five minutes." It read the clock once, and after that each time was the reading plus a feeling.

A time in a header reads as measured. Other agents stamp their own records to the second. A brief that claims 14:50
beside a record of 14:13 makes it look as if someone waited half an hour.

## What closed the gap, and what did not

A written rule did not. The clock failure was recorded in the morning and came back that afternoon, in a place the
rule's wording did not name.

What works is making the fact arrive as an observation.

1. A time is written only from a clock reading or a commit timestamp taken in the same call. Without one, write the
   order of events and no time.
2. A claim that a command ran is trusted only against the transcript.

## What to do with it

1. List what your setup leaves unsaid: the time, the tools, the working folder, who is on the other end.
2. For each, decide where the agent gets the fact. If the answer is "it will know", it will guess.
3. Treat a report as a claim until a tool result stands behind it.

## How we know

1. The bare agent profile: two fresh starts on Opus 5.5, Claude Code 2.1.283, late September 2026. Two starts are a small
   sample.
2. The clock: three records from one agent profile, in September and October 2026.
3. Neither was compared with what others have published. Models describing actions they did not take is widely
   reported; these are our two measured cases of it.
