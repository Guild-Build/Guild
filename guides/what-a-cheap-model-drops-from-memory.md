---
title: "What a cheaper model loses when it summarizes your agent's memory"
digest: "Agent memory piles up. Sooner or later someone has a cheaper model read it and write a digest, because the cheaper model costs well under half as much. We measured what that digest loses, and learned something about measuring it on the way."
author: GuildBuild research
published: 2026-10-22
updated: 2026-10-22
section: measured
series: Measured guides
sourceId: KN-06
tags:
  - memory
  - models
  - measurement
---

Agent memory piles up. Sooner or later someone has a cheaper model read it and write a digest, because the cheaper
model costs well under half as much. We measured what that digest loses, and learned something about measuring it on the way.

## The test

We took two big files from a real agent's memory.

1. A dense ledger, 55 KB: checkpoints, identifiers, sizes.
2. A narrative file, 67 KB: what happened and why.

Sonnet and Opus each wrote a digest of both. A digest cost $0.36 to $0.47 on Sonnet and $1.05 to $1.12 on Opus, and
Opus's digest was about twice as long.

Then we checked what survived. We looked for three kinds of fact a later reader needs: a size or percentage, a
failure event, and a mechanism.

## What survived

| Kept | Sonnet | Opus |
|---|---|---|
| Ledger, of 56 facts | 30 to 32 | 48 and 53 |
| Narrative, of 81 rows | 54 and 63 | 75 |

On the dense ledger Sonnet kept about 55 in 100 and Opus about 90.

Two things stood out.

1. Sonnet loses most in long entries. In entries of 5,000 bytes or more it kept fewer facts than in shorter ones, in
   both files. Both models cut a long entry short. Sonnet cut it to about 3 KB.
2. Its losses are whole facts. That matters more than it sounds. Each digest entry points back to its source lines. A
   figure missing from a kept fact can be fetched. A fact dropped whole cannot, because nothing tells the reader it
   exists.

## The sample that flattered

Our first measurement said Sonnet was fine on the narrative file. It was wrong, and the reason is the useful part.

The first sample was drawn by a reader's judgment: the facts that looked important. The digest was written the same
way. A
sample drawn by judgment and a digest written by judgment agree with each other. That agreement is not coverage of
the source.

We withdrew it. The second sample was drawn by a written rule, a fixed walk through the file. On the ledger it was
hashed before its reader saw any digest. On the narrative it was scored with the digests' makers masked. On those
samples both files told the same story.

The rule we keep: fix the sample from the source before the output exists.

## Is the digest at least true?

Dropping is one failure. Saying something false is another. We had a fresh auditor read Sonnet's digests of big files
entry by entry against the source.

1. The auditor's first grade: 9 of 15 entries stated something false or lost a failure.
2. A second reader then read every serious line by hand, and the disputed ones were ruled on. 5 of 15 stood.
3. The auditor's quotes matched the source 31 of 31 times. Its false alarms were of grade, not of fact.

What the failing entries did: marked a fix as made when the source only ordered it, put a statement in the wrong
mouth, gave a mechanism the source never gives, and left a failure in no field.

Most of the other flagged lines were lost detail. We read a sample of those and they were right. That is a digest
compressing, which is its job.

## What we do now

1. Files of 50,000 bytes or more go to the larger model.
2. A digest of a big file is a map to its source lines, not a substitute for them.
3. When something turns on a digest's claim, we open the lines it points to.

Summary faithfulness is a wide public field. This is its agent-memory case: the reader is another agent, and what it
needs is failures, sizes and causes.

## How we know

1. Two files, one reader per file, late September and early October 2026.
2. Two Sonnet runs and one or two Opus runs per file. The two Sonnet runs on the narrative differ by 9 rows of 81, so
   small gaps are inside the spread.
3. The audit is 15 entries from 9 digests by Sonnet 5.5 at medium effort. With 5 of 15, the true rate could be as
   high as 57 percent.
4. No small file has been measured on a rule-drawn sample. The cheaper model below 50,000 bytes is untested.
5. Why long entries lose more is not shown. Identifier density and entry length are tangled in one of the two files.
