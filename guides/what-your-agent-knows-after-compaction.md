---
title: "What your agent still knows after a compaction"
digest: "Two moments decide what an agent knows - the start of a session, and the moment its context is compacted. Most confusion about 'it forgot my instructions' comes from not knowing what each harness reloads at those two moments. We read the docs, then tested where the docs stop."
author: GuildBuild research
published: 2026-10-22
updated: 2026-10-22
section: measured
series: Measured guides
sourceId: KN-09
tags:
  - compaction
  - context
  - claude-code
  - codex
---

Two moments decide what an agent knows: the start of a session, and the moment its context is compacted. Most
confusion about "it forgot my instructions" comes from not knowing what each harness reloads at those two moments. We
read the docs, then tested where the docs stop.

## Claude Code

This part is the maker's documentation, and we add nothing to it but order.

At start it loads:

1. Every CLAUDE.md on the path, with its imports.
2. The first 200 lines or 25 KB of the memory index.
3. Each skill's name and description.

After a compaction it reloads from disk:

1. The project's root CLAUDE.md and its unscoped rules.
2. The memory index.
3. The bodies of skills already invoked, capped.
4. Up to five recently read or edited files.

A top search result says compaction summarizes your CLAUDE.md away. The docs say it reloads from disk. The docs are
right.

Everything else, every note and every reference file, reaches the agent only through a line in an index and the
model's own judgment. That is where we tested.

## Does an index line fire?

1. Asked a question, yes. A running agent profile was sent six questions. It opened the routed file each time, including one
   whose index line was written after it started. It reads the index from disk.
2. At start, no. A fresh agent profile whose index carried an at-every-start section was sent only its name. It opened
   nothing.
3. At the first real question, yes again. Given a remark with no keyword from any file, the fresh agent profile opened three
   notes whose triggers matched, about 9.5 KB, and placed the remark correctly. A agent profile started without the index
   answered the same remark from general knowledge.

So an index works at question time and not at start. If something must be read before the first task, an index line
will not do it. A line in the instruction file or a start hook will.

One side effect is worth knowing. After reading a routed note, the agent profile said "I" about a cost that the note's author
had paid, not the agent profile. A routed read can produce a false first person.

## Codex

We read nine compactions in three chats.

1. The human's own messages are kept, verbatim and in order.
2. The history that replaces the old one holds no tool output and no assistant message. Beside it the record keeps
   the last few messages in clear text; whether the model is shown those cannot be told from a log. One encrypted
   item carries the rest.
3. That item is rich. A long hash, a word and a number came through two compactions exact, with nothing visible
   holding them.
4. A message from another agent, sent through the app's tool, lands as tool output. It was dropped at the first
   compaction. A stub naming the sender survives, never the body.
5. A message put in through the queue lands as a user message, and stays.

So in a Codex team, what a peer told you is the first thing you lose.

Its generated memory has a separate limit. The summary injected at start arrived cut: about 2,500 tokens of a real
store's 6,100, with the middle removed. In our case the part lost held the routing entries for the current work. This
limit is public: see [openai/codex issue 48679](https://github.com/openai/codex/issues/48679).

A wider public write-up counts over a thousand Codex compactions and goes deeper on the mechanism. It also reports
that user messages are kept only up to a token budget, which we have not checked. Ours is narrower: what a handoff
between agents needs to survive.

## What to do with it

1. Anything that must survive a compaction belongs where the harness reloads it: the root instruction file, or a file
   on disk the agent is told to read.
2. Anything that must be read at start needs a hook or a line in the instruction file.
3. Do not hand work to a Codex peer in a message and expect it to outlive a compaction. Put it in a file and send the
   path.
4. Keep must-always-apply rules out of generated memory.

## How we know

1. The Claude Code load list is from the docs, read in late September 2026. The index tests are three agent profiles on
   Sonnet 5. Recall depends on model and effort, so a larger model may differ.
2. The Codex findings are from CLI 0.159.2, early October 2026. The value that survived two compactions was tested on
   one model.
3. Both harnesses ship often. Every line here belongs to its release.
