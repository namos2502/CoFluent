# CoFluent

> Two AIs walk into a terminal. One knows the flags. The other learns them.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![GitHub](https://img.shields.io/badge/GitHub-namos2502%2FCoFluent-181717?logo=github)](https://github.com/namos2502/CoFluent)

CoFluent is a Claude Code plugin that teaches Claude how to speak Copilot CLI — so when you need `copilot -p` invoked correctly, Claude already knows the flags, the permissions, and the patterns. No doc-diving, no flag guessing.

## Get it

```
/plugin marketplace add namos2502/agent-plugins
/plugin install cofluent@agent-plugins
/reload-plugins
```

> Requires Claude Code v1.0.33+ and the [GitHub Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli).
> Install it with `brew install copilot-cli` then `copilot login`.
> Note: `copilot` is not the same as `gh copilot` — different tool entirely.

## Commands

| Command | What it does |
|---------|-------------|
| `/cofluent:auto` | Activates copilot-aware mode — Claude handles `copilot -p` calls on its own |
| `/cofluent:verify` | Checks everything is installed and you are logged in |
| `/cofluent:ask` | Asks Copilot a question |
| `/cofluent:suggest` | Gets a shell command for a task |
| `/cofluent:explain` | Explains a command, error, or snippet |
| `/cofluent:fix` | Fixes a bug or error (reads and writes files) |
| `/cofluent:review` | Reviews staged diff or a specific file |
| `/cofluent:help` | Shows this reference |

### Pick your style

**Hands-off** — run `/cofluent:auto` once and let Claude decide when to reach for Copilot. Good for longer sessions where you just want things to work.

**Hands-on** — use explicit commands when you want to point Claude at a specific task. You stay in control of every delegation.

## How it works

There is a single skill file — `skills/copilot-cli/SKILL.md` — that Claude loads as its Copilot CLI reference manual. It covers the right flags for non-interactive use, how to scope tool permissions, which model fits which task, and ready-to-run patterns.

The short version of what Claude learns:

```bash
copilot -p "..." -s --no-ask-user --no-auto-update --no-color --allow-tool='read'
```

Full reference lives in the [official docs](https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-programmatic-reference).

## License

MIT
