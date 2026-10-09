# dbellizzi plugins

My personal plugin marketplace for Claude Code and Codex. The repo holds only the two marketplace files, and each plugin lives in its own repo:
- `.claude-plugin/marketplace.json` for Claude Code
- `.agents/plugins/marketplace.json` for Codex

| Plugin | What it does | Source |
|---|---|---|
| `agentlog` | Shared message board so agents working in the same repo (and its worktrees) can claim work, coordinate edits and ask each other to pause | [dbellizzi/agentlog](https://github.com/dbellizzi/agentlog) |

## Set up a machine (once)

```bash
claude plugin marketplace add dbellizzi/plugins
```
```bash
codex plugin marketplace add dbellizzi/plugins
```

## Install a plugin

```bash
claude plugin install agentlog@dbellizzi
```
```bash
codex plugin add agentlog@dbellizzi
```

Some plugins ship hooks, as agentlog does. For those, start `codex` once afterwards and approve the hooks in `/hooks`. Codex won't run plugin hooks until you approve them. Check each plugin's README for any other setup.

## Update

```bash
claude plugin marketplace update dbellizzi && claude plugin update agentlog@dbellizzi
```
```bash
codex plugin marketplace upgrade dbellizzi && codex plugin add agentlog@dbellizzi
```

Installed plugins only update when the plugin's own `version` changes, so releasing means bumping the version in the plugin's repo.

## Adding a plugin

Add an entry to **both** marketplace files with the same `name` and `source`. CI checks that they agree.
- **Plugin in its own repo:** use `{"source": "git-subdir", "url": "https://github.com/dbellizzi/<repo>.git", "path": "<dir with .claude-plugin/ and .codex-plugin/>"}`. If the plugin sits at that repo's root, use `{"source": "url", "url": "…"}` instead.
- **Small plugin that lives here:** put it in `plugins/<name>/` and use the string `"./plugins/<name>"` as the source in both files.

Plugin repos must not ship their own marketplace files. Installing the same plugin from two marketplaces would run its hooks twice.

Entries follow each plugin repo's default branch. Add `"ref"` or `"sha"` to an entry to pin a release.
