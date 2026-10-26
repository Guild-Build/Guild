---
title: "What is an agent build, and why rank builds instead of models"
digest: "An agent build is the complete configuration around a model: the skills, subagents, hooks, MCP servers, playbooks and settings a coding agent runs with. The same model scores very differently under different builds, so the build, not the model, is what a working team actually chooses."
author: GuildBuild
published: 2026-10-22
updated: 2026-10-22
section: guild
tags:
  - builds
  - harness
  - basics
---

An agent build is everything you give a model before it starts working: the instruction files it reads, the skills it can load, the subagents it can delegate to, the hooks that run around its tool calls, the MCP servers that give it tools, and the settings that allow or deny them. Game players call the equivalent a build or a loadout. Agent engineers have started calling the discipline of designing it harness engineering.

## Why the build matters as much as the model

Public benchmark re-runs in 2026 showed the same model scoring between the high fifties and the mid seventies on the same terminal task set depending on which harness ran it. That spread is about the size of the gap between model generations. A team that picks a model and ignores the build is leaving most of the result on the table.

Model leaderboards rank models under a lab's own harness. Skill directories list parts, usually by star count. Neither tells you which complete configuration does your kind of work well, cheaply and within your rules. That is what the guild ranks.

## What a build is made of

| Part | What it is | Example in a guild build |
|---|---|---|
| Playbook | Instruction text the agent reads at start: CLAUDE.md or AGENTS.md fragments | Lean Start playbook |
| Skill | A folder with a SKILL.md the agent loads when the task matches its description | test-first, source-ledger |
| Agent | A subagent definition with its own tools and limits | scout (read-only), independent-reviewer |
| Hook | A command that runs on an event such as before a tool call or when the agent stops | merge-guard, uncommitted-work-check |
| MCP server | A tool server the agent can call | GitHub MCP, Playwright MCP |
| Settings | Allow and deny lists, model and effort choices | deny Write for a read-only profile |
| Agent profiles | The roles inside a build, each with a model, effort, tools and limits | author and reviewer in Evidence-First Review |

A guild build is packaged as a Claude Code plugin (and, from Season 1, a Codex plugin) with one extra file, `buildguild.json`, that names its branches, modules, agent profiles and trial.

## How a build earns a tier

Tiers move only on evidence from a trial run on a named machine. D is a valid, declared build. C means the author ran the season trial and published the evidence from their own GitHub account. B means a second account reproduced it within tolerance. A adds a hidden slice run by the guild. S is the season leader among A builds. The full rules are on the tiers page, and the trial design is published with every season.

## Cite this

GuildBuild, "What is an agent build, and why rank builds instead of models", buildguild.gg, 22 October 2026, https://buildguild.gg/guides/what-is-an-agent-build/
