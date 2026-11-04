# Hearth — Claude Reference

## What this is

Discord tooling for TK ForgeWorks: bots and other tools for the company's
servers and servers its people take part in, plus ideas for what to build
next. Greenfield as of 2026-11-03: repo set up to org standards, no tools or
code yet.

## Stack & commands

No toolchain chosen yet. Decide it with the first tool's ticket, then add the
matching org CI workflow (`ci-typescript.yml` or `ci-python.yml`), the
Dependabot ecosystem entry, and fill in this table.

| Task | Command |
|---|---|
| Install | TBD |
| Lint | TBD |
| Test | TBD |

## Repo & process conventions (org standard)

- **Jira: project TKFW ("TKForgeWorks"), shared with other company work.
  Every hearth ticket MUST carry the `discord` label** (keep any other
  labels). Filter: `project = TKFW AND labels = discord`. Move a ticket to
  *In Progress* when its branch opens. **Never close a ticket unless asked** —
  comment "Actions taken" + commit hash and leave it for the human to verify.
- Branching (**deviation** from `tkforgeworks/.github/docs/branching-and-release.md`,
  recorded in `.github/org-standards.yml`): hearth holds several tools, each
  versioned and released on its own, so there is **no repo-wide
  `vX.Y.Z/main` release branch**. Topic branches are
  `feature/TKFW-N-short-topic`, cut from `main` and PR'd into `main`.
- `main` is protected by the org repository ruleset (id 26504718): PR-only,
  no force-push or deletion, **no bypass actors** (not even admins).
  Required check: **none yet** — add it with `PUT` once the first tool's CI
  has reported on a PR (standard:
  `tkforgeworks/.github/docs/branch-protection-ruleset.md`). Topic branches
  are deleted automatically when their PR merges (repo setting).
- CI consumes the shared reusable workflows
  `tkforgeworks/.github/.github/workflows/<ci-typescript|ci-python>.yml@main`.
  Don't hand-roll steps that belong in the shared workflow — change it there.
- **Commit subjects are the changelog.** `TKFW-N: Imperative summary`.
  Bug fixes: `TKFW-N: Fix ...`. `ticket-prefix: TKFW` for `release-notes.yml`.
- Releases: per tool, not per repo. No release pipeline exists yet; the org
  release workflows assume one version per repo, so a per-tool release
  workflow is an org-level addition to make before the first tool ships.
  Never hand-edit versions or push tags by hand once one exists.
- License: **Apache-2.0** (`LICENSE` is the verbatim Apache text — never
  edit it). Image assets (`.svg`/`.png`/etc.) are **all rights reserved** via
  `NOTICE`. Manifest `license` fields must say `Apache-2.0`. Standard:
  `tkforgeworks/.github/docs/licensing.md`.
- Update this file as the last step of closing any ticket that changed
  conventions, architecture, or status.

## Architecture decisions (locked)

- Multi-tool repo with per-tool versioning and releases; no repo-wide
  release branch (2026-11-03, TKFW-95).

## Current status

TKFW-95: repo bootstrapped to org standards (licensing, templates, `main`
ruleset without a required check). No tools yet.
