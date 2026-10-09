# dbellizzi plugins

My personal plugin marketplace for Claude Code, Codex and Muse. The repo holds only the two marketplace files, and each plugin lives in its own repo:
- `.claude-plugin/marketplace.json` for Claude Code
- `.agents/plugins/marketplace.json` for Codex, which Muse reads too

Antigravity has no compatible marketplace, so its plugins install straight from their repo (see each plugin's README).

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
```bash
muse plugins marketplace add dbellizzi dbellizzi/plugins
```

## Install a plugin

```bash
claude plugin install agentlog@dbellizzi
```
```bash
codex plugin add agentlog@dbellizzi
```
```bash
muse plugins install agentlog@dbellizzi && muse plugins approve agentlog
```

Some plugins ship hooks, as agentlog does. Codex and Muse won't run plugin hooks until you approve them: in Codex, start `codex` once afterwards and approve them in `/hooks`; in Muse, `muse plugins approve <plugin>` does it. Check each plugin's README for any other setup.

## Update

```bash
claude plugin marketplace update dbellizzi && claude plugin update agentlog@dbellizzi
```
```bash
codex plugin marketplace upgrade dbellizzi && codex plugin add agentlog@dbellizzi
```
```bash
muse plugins marketplace update dbellizzi && muse plugins install agentlog@dbellizzi && muse plugins approve agentlog
```

Installed plugins only update when the plugin's own `version` changes, so releasing means bumping the version in the plugin's repo.

## Adding a plugin

Add an entry to **both** marketplace files with the same `name` and `source`. CI checks that they agree.
- **Plugin in its own repo:** put the plugin at that repo's root (its `.claude-plugin/`, `.codex-plugin/`, … next to the README) and use `{"source": "url", "url": "https://github.com/dbellizzi/<repo>.git"}`. Avoid `git-subdir` sources: Claude Code and Codex accept them, but Muse skips them.
- **Small plugin that lives here:** put it in `plugins/<name>/` and use the string `"./plugins/<name>"` as the source in both files.

Plugin repos must not ship their own marketplace files. Installing the same plugin from two marketplaces would run its hooks twice.

Entries follow each plugin repo's default branch. Add `"ref"` or `"sha"` to an entry to pin a release.
