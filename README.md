# github-agentic-workflow

A repository for experimenting with [GitHub Agentic Workflows](https://github.github.io/gh-aw/) (`gh aw`) — Markdown-defined, AI-agent-driven GitHub Actions workflows.

## What is `gh aw`?

`gh aw` compiles Markdown files with YAML frontmatter (in [.github/workflows](.github/workflows)) into standard GitHub Actions workflows (`*.lock.yml`). Each `.md` file describes, in natural language, what an AI agent should do; the frontmatter controls triggers, permissions, and which tools (bash commands, GitHub API, MCP servers, etc.) the agent is allowed to use.

## Workflows

### `daily-repo-status`

Source: [githubnext/agentics – repo-status](https://github.com/githubnext/agentics/blob/main/workflows/repo-status.md)

This workflow was added via:

```powershell
gh aw add-wizard githubnext/agentics/daily-repo-status
```

It runs on a daily schedule (and can be triggered manually via `workflow_dispatch`), then:

1. Gathers recent repository activity — issues, pull requests, discussions, releases, and code changes.
2. Studies that activity to assess project progress and highlight community contributions.
3. Opens a new GitHub issue (labeled `report`, `daily-status`) containing an upbeat status report with recommendations and next steps for maintainers, closing out older status issues as it goes.

**What adding it achieved:**
- Automated recurring project visibility — a fresh status report lands as an issue every day without manual effort.
- Read-only, tightly scoped permissions (`contents: read`, `issues: read`, `pull-requests: read`) plus an explicit, minimal `bash` allowlist (`cat`, `ls`, `find`, `grep`, `head`, `tail`, `wc`), so the agent can inspect the repo without shell access beyond those commands.
- A compiled, auditable GitHub Actions workflow (`daily-repo-status.lock.yml`) generated from the human-readable `daily-repo-status.md` source, keeping intent and implementation in sync.

## Usage

- Compile workflows after editing a `.md` source: `gh aw compile`
- Run a workflow on demand: `gh aw run <workflow-name>`
- Update workflows to the latest upstream version: `gh aw update`