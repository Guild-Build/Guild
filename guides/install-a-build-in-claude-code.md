---
title: "Install a build in Claude Code in two commands"
digest: "Add the guild marketplace once with claude plugin marketplace add and the site's marketplace URL, then install any guild build with claude plugin install. Builds are zip archives pinned by SHA-256, served from buildguild.gg, and need Claude Code 2.1.224 or later."
author: GuildBuild
published: 2026-10-22
updated: 2026-10-25
section: guild
tags:
  - install
  - claude-code
  - marketplace
---

The guild site is itself a Claude Code plugin marketplace. You add it once; after that every guild build is one command away.

## Add the marketplace

```
claude plugin marketplace add https://buildguild.gg/.claude-plugin/marketplace.json
```

Claude Code fetches only that file. Each build entry points at a zip archive on this site with a SHA-256 digest, and Claude Code refuses a download that does not match. Archive sources need Claude Code 2.1.224 or later; running a trial with `claude plugin eval` needs 2.1.269 or later. Check with `claude --version`.

## Install a build

```
claude plugin install measured-delivery@buildguild
```

Add `--scope project` to install for one repository (it writes the committed `.claude/settings.json`), or `--scope local` for a git-ignored personal setting. The default scope is user. Inside a session, `/plugin install measured-delivery@buildguild` opens the details first and installs nothing until you confirm a scope.

## Update, list, remove

```
claude plugin marketplace update buildguild
claude plugin update measured-delivery@buildguild
claude plugin list
claude plugin uninstall measured-delivery@buildguild
```

Auto-update is off by default for third-party marketplaces, so a build changes on your machine only when you ask.

## What a build can do on your machine

Hooks, MCP servers and anything under a plugin's `bin/` run with your privileges, outside the permission sandbox. Read the build page and the bundle before installing, exactly as you would read a shell script before running it. Guild builds list every hook and script on their page.

## Without the marketplace: the buildguild CLI

Run from a checkout of the Build repository; the `buildguild` npm package is not published yet, so do not run it with `npx`.

```
node cli/buildguild.mjs add measured-delivery --scope project
```

The CLI downloads the same archive, verifies the digest, unpacks it into `.claude/` (and `.agents/skills/` for Codex users) and records what it installed in `.buildguild/lock.json`, so `node cli/buildguild.mjs remove measured-delivery` reverses it exactly.

## Cite this

GuildBuild, "Install a build in Claude Code in two commands", buildguild.gg, 22 October 2026, https://buildguild.gg/guides/install-a-build-in-claude-code/
