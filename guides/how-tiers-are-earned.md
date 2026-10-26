---
title: "How tiers are earned: the rules behind D, C, B, A and S"
digest: "A tier on buildguild.gg is computed from trial evidence, never typed in and never bought. Declared needs a valid manifest. Tested needs the author's own run, published from their GitHub account. Reproduced needs a second account. Proven adds a hidden slice run by the guild. Season leader is the top Proven build per category on the season's ranked trials. Trials have a ladder of their own: Declared, Calibrated, Adopted."
author: GuildBuild
published: 2026-10-22
updated: 2026-10-22
section: guild
tags:
  - tiers
  - evidence
  - trials
---

Every tier on this site is computed at build time from evidence files in the repository. There is no field a maintainer can set to raise a build. Sponsors get a label; they never get a tier.

## The ladder

| Tier | Needs | Who can produce it |
|---|---|---|
| D · Declared | A valid plugin manifest, a licence, one named branch, an author | Any accepted submission |
| C · Tested | The author ran the season trial on a named environment and published the evidence as a public Gist from their GitHub account | The author |
| B · Reproduced | A second GitHub account reproduced the trial on its own machine within tolerance and published its own Gist | Any member who is not the author; we call them Assayers |
| A · Proven | B, plus a pass on the season's hidden slice run by a guild maintainer, plus a published cost profile | Guild maintainers |
| S · Season leader | The top A build per branch and category when the season closes | Computed |

## What counts as evidence

An evidence file is the JSON report that `claude plugin eval` writes, wrapped with the build version and archive hash, the trial version, and an environment fingerprint: operating system, Claude Code version, model, effort, date and the GitHub login of the runner. Failed and incomplete runs stay in the denominator. Two records with different fingerprints are different evidence and are never averaged.

## Tolerance

A reproduction counts when the overall score is within 0.10 of the author's and no dimension differs by more than 0.25. Below that, both results are published side by side and the build stays at C until a third run settles it.

## Ranked trials and custom games

A build has a tier on every trial it has evidence for. Its season tier is its best tier on any trial in the season's ranked set; S is awarded on the ranked set only. Every other trial is a custom game whose leaderboard feeds the build's profile but not its season rank. The evidence grid on the tier page shows all of it: builds down, trials across.

## Trials earn tiers too

| Tier | Needs |
|---|---|
| Declared | Valid cases, a licence, a README, accepted by a maintainer |
| Calibrated | A build and the bare runtime differ by at least 15 points on it |
| Adopted | Calibrated, and used in accepted evidence by three or more GitHub accounts |

A trial written by a build's author never carries that build above Tested. See [write a trial](/guides/write-a-trial/).

## The baseline

Every season lists the bare runtime, run on the same trial with no modules. The runner does this for you: each case runs with and without the plugin. Every build page shows its delta over the baseline, so a tier answers "did this beat nothing", not only "did this pass".

## What tiers do not claim

A tier does not say a build is safe. The runner's own documentation says a passing suite says nothing about safety, and we repeat it. A tier says the build did what the trial's cases describe, on that machine, on that day, and that the record is public.

## Cite this

GuildBuild, "How tiers are earned", buildguild.gg, 22 October 2026, https://buildguild.gg/guides/how-tiers-are-earned/
