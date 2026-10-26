---
title: "Write a trial: tasks are community content too"
digest: "A trial is a versioned set of tasks with fixtures and deterministic graders that builds are tested on. Members write them, submit them like builds, and they earn their own tier: Declared when valid, Calibrated when a build and the bare runtime differ by fifteen points, Adopted when three accounts have used it. A trial never carries its author's own build above Tested."
author: GuildBuild
published: 2026-10-22
updated: 2026-10-22
section: guild
tags:
  - trials
  - evals
  - authoring
---

In a game the ground truth is the game. Here the community writes it. A trial is first-class content: it has an author, a version, a licence, a page, a leaderboard and a tier of its own.

## What a trial contains

One folder per case, in the format the official plugin eval runner reads:

```
my-trial/
├── trial.json                 # name, version, categories, dimensions, tolerance, hidden-slice count
├── README.md                  # what it measures and for whom, forty words or more
├── LICENSE
└── cases/<case>/
    ├── prompt.md              # frontmatter: tags (one dimension), runs, max_turns, timeout_seconds, allowed_tools
    ├── case.yaml              # context.scaffold_script: fixture.sh
    ├── fixture.sh             # builds the workspace; no network
    └── graders/*.md           # regex, tool_used, tool_order, file_exists
```

## The rules

1. One dimension per case in its tags: scope, quality, safety, handoff, tools, reliability or cost.
2. Deterministic graders first. An `llm` grader needs a pinned judge model and is not used in Season 0.
3. Fixtures build everything they need and never reach the network.
4. Check that the bare runtime can fail it: run one case with `--runs 1 --ablation with-without`. A case every bare runtime passes measures nothing.
5. Declare your rights and opt in to be named.

## How a trial earns its tier

| Tier | Needs |
|---|---|
| Declared | Valid cases, a licence, a README, accepted by a maintainer |
| Calibrated | A build and the bare runtime differ by at least 15 points on it |
| Adopted | Calibrated, and used in accepted evidence by three or more GitHub accounts |

"Ranked" is separate: each season names a small ranked set that decides season tiers and the S award. Every other trial is a custom game with its own leaderboard.

## Conflicts

Your trial never carries your own build above Tested; the site enforces it. Hidden slices rotate each season and are run by guild maintainers. Assayers reproduce trials as well as builds.

## Submit

Use the [submit page](/submit/) with kind "trial" and a public repository. A maintainer validates the cases, runs one with the bare runtime, and lists the trial as Declared. The [Trial Runner](/builds/trial-runner/) build's write-eval-case skill writes a case in the right shape.

## Cite this

GuildBuild, "Write a trial", buildguild.gg, 22 October 2026, https://buildguild.gg/guides/write-a-trial/
