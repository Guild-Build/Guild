# Guild

The community side of [BuildGuild](https://buildguild.gg/): the guides and articles the site publishes, the rules the
guild keeps, and, later, its governance.

| Path | What it holds |
|---|---|
| `guides/<slug>.md` | One guide or article per file, published at `buildguild.gg/guides/<slug>/`. |
| `code-of-conduct.md` | Rules of the hall for members, maintainers and Assayers ([buildguild.gg/code-of-conduct](https://buildguild.gg/code-of-conduct/)). |
| `oaths.md` | The rules the guild holds itself to ([buildguild.gg/oaths](https://buildguild.gg/oaths/)). |
| `governance/` | Reserved for community governance material. |

## Discussions

The guild's Discussions live in this repository: questions, proposals for guides, and the maintainers' replies to what
members send from [buildguild.gg/submit](https://buildguild.gg/submit/).

## Propose a guide

Start a Discussion with the question the guide answers and the sources it rests on. To write it, fork this repository
and open a pull request that adds `guides/<slug>.md`, one guide per pull request. Its front matter needs a `title`, a
`digest` (the answer in the first paragraph, at least 60 characters), an `author`, `published` and `updated` dates
(YYYY-MM-DD), at least one entry under `tags`, and a `section` (`guild`, `measured` or `seasons`).
[`CONTRIBUTING.md`](CONTRIBUTING.md) has the rest.

Licensed under Creative Commons Attribution 4.0 International ([`LICENSE`](LICENSE)).
