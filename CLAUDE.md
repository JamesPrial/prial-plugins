# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A Claude Code **marketplace manifest** — not plugin source. The only meaningful file is `.claude-plugin/marketplace.json`, which registers 10 plugins (todo-log, version-control, security-hooks, golang-workflow, go-plugin-release, deep-research, bumper, openclaw-assist, unraid-assistant, claudebot). Each plugin's actual code lives in its own GitHub repo under `JamesPrial/<name>`. There is no source, no build, no tests, no lint in this repo.

Almost every task here is a small edit to `marketplace.json`. If you find yourself looking for plugin implementation files in this directory, stop — they aren't here.

## Plugin entry schema

Each item in the `plugins` array uses this shape:

```json
{
  "name": "<plugin-name>",
  "source": {
    "source": "github",
    "repo": "JamesPrial/<repo-name>",
    "ref": "releases",
    "sha": "e324eff9b95..."
  },
  "description": "..."
}
```

`ref`, `sha`, and `description` are optional. Two pinning styles coexist intentionally:

- **Pinned** (`ref` + `sha`): used for `todo-log` and `security-hooks`. Updating these is a two-step convention seen in history: first pin the branch with a sha placeholder, then a follow-up commit updates the sha. The plugin repo name often differs from the marketplace `name` (e.g. `golang-workflow` → `JamesPrial/golang-plugin`).
- **Floating** (no `ref`/`sha`): tracks the default branch. Most entries use this.

Don't normalize one style into the other without being asked — the difference is deliberate.

## Conventions

- Commit messages follow `feat: add <plugin> plugin to marketplace` for additions, `fix:` for repo/URL corrections, `chore: Pin <plugin> to ...` for sha bumps. Match the existing style when adding entries.
- `firebase-debug.log` at the repo root is a local artifact and should stay untracked. Don't commit it.
