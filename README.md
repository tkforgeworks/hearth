# Hearth

A TK ForgeWorks Product

## Purpose

This repo is meant to hold and serve as needed all discord tooling for the TK ForgeWorks LLC.  It will contain bots and discord tooling, potentially other items for use by the company in personal and other servers the users/employees may participate in.

## Available tooling

None yet. See [Proposed tooling](#proposed-tooling).

## Tooling descriptions

Each tool gets a section here once it ships.

## Proposed tooling

Ideas only: nothing here is built yet. Everything planned is tracked under
the Jira epic [TKFW-97](https://tkforgeworks.atlassian.net/browse/TKFW-97)
(label `discord`). Each ticket starts as a skeleton labelled `needs-spec`, and
gets a full spec before any work starts. "Type" refers to the
[tool types](#proposed-stack) below.

### Dev workflow

| Tool | What it does | Type | Ticket |
|---|---|---|---|
| Release announcer | Posts a product's generated release notes to a `#releases` channel when its release workflow publishes. Likely an org reusable workflow in `tkforgeworks/.github` that any repo's release can call. | Webhook | [TKFW-101](https://tkforgeworks.atlassian.net/browse/TKFW-101) |
| Jira bridge | `/ticket TKFW-95` shows a status card. A right-click "Create TKFW ticket" on a message files it with the `discord` label. Optional feed of status changes. | Slash-command bot | [TKFW-102](https://tkforgeworks.atlassian.net/browse/TKFW-102) |
| Digest poster | Posts the weekly and midweek forge-digest summaries to a channel as well as email. | Webhook | [TKFW-105](https://tkforgeworks.atlassian.net/browse/TKFW-105) |

### For users of TK ForgeWorks apps

| Tool | What it does | Type | Ticket |
|---|---|---|---|
| Bug and feedback intake | A `/bug` form, or a forum channel, that files a Jira bug with the app's label and replies with the ticket key. | Slash-command bot | [TKFW-103](https://tkforgeworks.atlassian.net/browse/TKFW-103) |
| Beta tester hub | A tester role people opt into, and an announcement when a release candidate reaches the Play internal track. | Slash-command bot + webhook | [TKFW-104](https://tkforgeworks.atlassian.net/browse/TKFW-104) |
| Docs and FAQ lookup | `/docs <query>` searches the product READMEs and docs. Could later answer questions with Claude. | Slash-command bot | [TKFW-106](https://tkforgeworks.atlassian.net/browse/TKFW-106) |

### Groundwork

Needed before the first code tool ships:

| Work | Ticket |
|---|---|
| TypeScript `tools/` workspace, org CI, and the required check on `main` | [TKFW-98](https://tkforgeworks.atlassian.net/browse/TKFW-98) |
| Dev Discord application, private test server and Cloudflare Workers hosting | [TKFW-99](https://tkforgeworks.atlassian.net/browse/TKFW-99) |
| Per-tool release workflow and an org branching standard for multi-tool repos | [TKFW-100](https://tkforgeworks.atlassian.net/browse/TKFW-100) |

### Use Discord's built-in features instead

Don't build these; Discord or the services we already use cover them. The
ones that need setting up have a configuration ticket:

- **GitHub CI and PR feed:** add `/github` to the end of a Discord webhook URL
  and add it as a GitHub repo or org webhook. Build a bot only if it needs
  filtering.
  ([TKFW-107](https://tkforgeworks.atlassian.net/browse/TKFW-107))
- **Member onboarding and role picking:** Server Settings → Onboarding.
  ([TKFW-108](https://tkforgeworks.atlassian.net/browse/TKFW-108))
- **Moderation:** AutoMod.
  ([TKFW-109](https://tkforgeworks.atlassian.net/browse/TKFW-109))
- **Homelab and Grafana alerts:** Grafana has a Discord contact point. Not
  planned yet.
- **Polls and event scheduling:** built-in polls and scheduled events. Nothing
  to set up.

## Proposed stack

TypeScript throughout, so every tool shares one toolchain and the org
`ci-typescript.yml` workflow. Which way a tool runs depends on what it does:

| Type | Use it for | Runs on |
|---|---|---|
| Webhook | One-way notifications | A GitHub Actions step or the sending service. No bot account. |
| Slash-command bot | Slash commands, buttons and forms. Discord sends each one to the bot as an HTTP request. | Cloudflare Workers, following Discord's [Cloudflare sample app](https://github.com/discord/cloudflare-sample-app). No server to run. |
| Always-connected bot | Reacting to messages and server events as they happen | A long-running Node process using [discord.js](https://discord.js.org/), such as a homelab container |

Start every tool as a webhook or slash-command bot and move it to an
always-connected bot only if it needs live events.

**Layout:** one folder per tool, `tools/<name>/`, each with its own
`package.json` and version, so tools are released separately.

**Testing:**

- A separate dev Discord application (its own token) and a private test
  server. Register commands to the test server only, so they update
  immediately.
- Keep logic in plain functions with unit tests (Vitest); command handlers
  only pass Discord data in and out.
- Workers: run locally with `wrangler dev` and test with Cloudflare's Vitest
  integration, including that requests with a bad signature are rejected.
- Discord has no official emulator, so each change ends with a manual check
  in the test server.

## Versioning and releases

Hearth holds several independent tools, and each one is versioned and
released on its own. Because of that, this repo does not follow the org's
single `vX.Y.Z/main` release-branch model: topic branches are cut from `main`
and merged back into `main` by pull request. This is a recorded deviation from
the org standard (see [`.github/org-standards.yml`](.github/org-standards.yml)).

Work is tracked in the TKFW Jira project; every hearth ticket carries the
`discord` label.

## License

Apache-2.0 — see [`LICENSE`](LICENSE). Image assets (logos, icons,
illustrations, screenshots) are **not** covered and are all rights reserved;
see [`NOTICE`](NOTICE).
