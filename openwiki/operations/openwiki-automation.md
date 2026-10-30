---
type: Automation
title: OpenWiki documentation automation
description: How this repository's generated wiki is refreshed — the scheduled GitHub Actions workflow that runs OpenWiki against the repo and opens a pull request, plus the agent instruction files it maintains.
tags: [openwiki, github-actions, automation, documentation, ci]
---

# OpenWiki documentation automation

This repository's wiki under `openwiki/` is **generated**, not hand-written. A scheduled GitHub Actions workflow regenerates it and opens a pull request. The pages you are reading can therefore be overwritten; the durable source of truth is the repository code and assets.

## The scheduled workflow

`/.github/workflows/openwiki-update.yml` (untracked at the time of writing — it exists in the working tree but is not in the last commit) defines the `OpenWiki Update` workflow.

**Triggers**

- `workflow_dispatch` — manual run from the Actions tab.
- `schedule` — `cron: "0 8 * * *"`, i.e. daily at 08:00 UTC.

**Permissions** — `contents: write` and `pull-requests: write`, which is what lets the workflow push a branch and open a PR.

**Steps**

1. `actions/checkout@34e1148` (v4) — checks out the repository.
2. `actions/setup-node@49933ea` (v4) with `node-version: "22"`.
3. `npm install --global openwiki@0.2.3 mermaid@11.16.0 jsdom@29.1.1` — installs OpenWiki plus the optional Mermaid validation stack. The inline comment notes that `mermaid` and `jsdom` are optional and exist to add high-fidelity validation of Mermaid diagrams; they can be removed if the wiki contains none.
4. `openwiki code --update --print` — runs the update in code mode against the current repository and prints the final output.
5. `peter-evans/create-pull-request@22a9089` (v7) — opens a PR from branch `openwiki/update` with commit message `docs: update OpenWiki`, restricted to these paths:
   - `openwiki`
   - `AGENTS.md`
   - `CLAUDE.md`
   - `.github/workflows/openwiki-update.yml`

**Environment**

| Variable | Source | Purpose |
|---|---|---|
| `OPENWIKI_PROVIDER` | literal `openrouter` | Selects the model provider. |
| `OPENROUTER_API_KEY` | GitHub secret | Provider credential. Never print or copy this value. |
| `OPENWIKI_MODEL_ID` | literal `z-ai/glm-5.2` | Model used for generation. |
| `LANGSMITH_API_KEY` | GitHub secret | Tracing credential. Never print this value. |
| `LANGCHAIN_PROJECT` | literal `openwiki` | LangSmith project name. |
| `LANGCHAIN_TRACING_V2` | literal `"true"` | Enables LangChain tracing. |

All actions are pinned to full commit SHAs, so the workflow is reproducible and resistant to upstream tag mutation.

## Agent instruction files

`/AGENTS.md` and `/CLAUDE.md` are byte-identical (502 bytes each) and contain only an OpenWiki block delimited by `<!-- OPENWIKI:START -->` / `<!-- OPENWIKI:END -->` markers. The block tells agents to start at `openwiki/quickstart.md`, follow its links, and prefer regenerating over hand-editing generated pages.

These files are scaffolding, not product documentation. Do not add product notes to them, and do not edit them during ordinary wiki runs unless explicitly asked — the workflow regenerates them anyway.

## What this means for contributors

- **Do not hand-edit pages under `openwiki/`.** Change the source (`index.html`, `assets/`) and let the workflow regenerate, or run the update locally with `openwiki code --update --print`.
- **The PR is the review surface.** Changes to `openwiki/`, `AGENTS.md`, `CLAUDE.md`, and the workflow file itself all land in the same PR, so a reviewer can see documentation drift alongside the source change that caused it.
- **Daily cadence, not on-commit.** The wiki reflects whatever was on `main` at 08:00 UTC; a source change can take up to a day to appear in the docs.
- **Locally**, `openwiki --init` regenerates documentation for the current repository, and `openwiki --update [message]` refreshes it — the workflow uses the latter via `openwiki code --update --print`.

## Repo state at the time of the initial documentation run

- The last commit is `5014cef` — "CLARIS Optics 中文版 demo" — and contains only `index.html` and the eight files in `assets/`.
- `.github/`, `AGENTS.md`, and `CLAUDE.md` were untracked in the working tree, meaning the OpenWiki scaffolding was added locally but not yet committed.

This matters when reading git history: the visible history says nothing about the wiki or the workflow, because neither has been committed.