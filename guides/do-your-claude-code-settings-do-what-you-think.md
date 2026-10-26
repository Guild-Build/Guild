---
title: "Do your Claude Code settings do what you think?"
digest: "A settings file feels like a contract - you write a key, the tool obeys. For Claude Code that holds less often than it looks. A key can be valid and unknown to your editor. A file can be right and never read by the session you are in. And some things a package simply cannot carry."
author: GuildBuild research
published: 2026-10-22
updated: 2026-10-22
section: measured
series: Measured guides
sourceId: KN-10
tags:
  - settings
  - claude-code
  - configuration
---

A settings file feels like a contract: you write a key, the tool obeys. For Claude Code that holds less often than it
looks. A key can be valid and unknown to your editor. A file can be right and never read by the session you are in.
And some things a package simply cannot carry.

We counted one of these gaps and read the docs for the other two.

## The schema knows fewer keys than the docs

Claude Code publishes a JSON schema for its settings file. Editors use it to complete and check keys.

1. The schema lists 142 top-level keys.
2. The docs' settings reference carries 231 key headings. These are not the same kind of count: the 231 may include
   nested keys.
3. So we did a like-for-like check. We took one working, lean settings file with 17 top-level keys. Nine of the 17
   are missing from the schema.

The schema also allows unknown keys. Put those two facts together. Your editor will not offer nine keys that work,
and schema validation will not flag a key you misspelled. Whether the harness itself warns about an unknown key is a
separate check, and we did not run it.

The docs say the schema can lag the newest releases. We put a number on the lag.

## Which session reads which file

This part is from the docs.

1. The terminal and the desktop app read the same files.
2. A cloud session reads only what is committed to the repository: the project's instruction file, its shared
   settings, rules, skills and agents. It does not read your user settings, your local settings file or your
   personal instruction file.
3. Some keys never apply from a shared project file at all.
4. Allow rules wait until you trust the folder. Deny and ask rules apply at once.
5. The settings screen shows a short set of options, not every key.

The consequence: a setup that must behave the same everywhere has one landing place, the committed project files.
Anything you put in your user settings is gone in the cloud.

## What a plugin cannot carry

Also from the docs. A plugin can ship skills, commands, agents, hooks and servers. It cannot ship:

1. An instruction file. One at the plugin's root is not loaded as project context.
2. Permissions. A plugin's settings file takes effect for two keys only, and neither is a deny rule.

So a packaged agent splits in two. Its tools can ride a plugin. Its fence, the deny rules and the instruction file,
has to be written into the project as files.

## What to do with it

1. Do not trust editor validation for settings keys. Check a key against the docs, then check that it took effect.
2. Decide which sessions must obey a rule, then put it in a file those sessions read.
3. Ship fences as project files and tools as plugins.
4. Count again at each release. The schema and the docs both move.

## How we know

1. The schema count is from the published schema on one day in early October 2026, by a script. It was counted once.
2. The 231 is from a copy of the docs dated mid-September 2026.
3. The surface and plugin sections are a reading of the docs, not a measurement. Anyone can check them there.
4. Since then the harness has added code-level extensions that can refuse a tool call. That is a second kind of deny
   and is not covered here.
