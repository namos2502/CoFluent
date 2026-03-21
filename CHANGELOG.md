# Changelog

All notable changes to xFlow will be documented here.

## [0.4.0] — 2026-03-19

### Added
- `skills/claude-cli/SKILL.md` — programmatic reference for the Claude CLI; covers `claude -p` flags, tool permission syntax (`--allowedTools`, `--disallowedTools`, `--tools`), model selection, budget caps, and common invocation patterns for ask/suggest/explain/fix/review tasks
- `/xflow:auto` now loads both `copilot-cli` and `claude-cli` skill references; includes routing guidance (Copilot for GitHub tasks, Claude for general code tasks)
- `/xflow:setup` now detects and authenticates both Copilot CLI and Claude CLI; CLAUDE.md block updated to describe both agents
- `/xflow:help` now shows commands in a multi-agent context

---

## [0.3.0-beta] — 2026-03-19

### Changed
- Renamed project from **CoFluent** to **xFlow** — reflecting the broader multi-agent vision
- New tagline: "One plugin. Every CLI. Stay in flow."
- All commands renamed from `/cofluent:*` to `/xflow:*`
- Plugin install slug changed from `cofluent@agent-plugins` to `xflow@agent-plugins`
- GitHub repo renamed from `namos2502/CoFluent` to `namos2502/xFlow`

---

## [0.2.0-beta] — 2026-03-18

### Added
- `/xflow:setup` — one-time setup command that verifies Copilot CLI is installed and authenticated, then registers xFlow awareness in `~/.claude/CLAUDE.md` so Claude knows about xFlow in every future session (idempotent — safe to re-run)
- `## Why xFlow` section in README covering two key benefits: staying in flow and saving tokens on Claude by offloading subtasks to a separate `copilot -p` process

### Removed
- `/xflow:verify` — absorbed into `/xflow:setup`; re-run setup anytime to re-check your installation


---
## [0.1.0-beta] — 2026-03-18

Initial beta release. Core plugin structure is stable; commands and skill reference are functional but may evolve based on feedback.

### Added
- `skills/copilot-cli/SKILL.md` — core programmatic reference for the GitHub Copilot CLI; auto-loaded by Claude when invoking `copilot -p`
- `/xflow:auto` — activates autonomous Copilot CLI mode for the session; Claude will invoke `copilot -p` automatically when appropriate
- `/xflow:verify` — checks that Copilot CLI is installed and authenticated; guides the user through fixing any issues
- `/xflow:ask` — delegates a question to Copilot (read-only, fast, uses `claude-haiku-4.5`)
- `/xflow:suggest` — asks Copilot to suggest a shell command for a task
- `/xflow:explain` — asks Copilot to explain a command, error, or code snippet
- `/xflow:fix` — asks Copilot to fix a bug or error (read + write file permissions)
- `/xflow:review` — runs the Copilot `/review` agent on staged diff or a specific file
- `/xflow:help` — shows the full command reference
- `.claude-plugin/plugin.json` — plugin manifest for installation via Claude Code plugin system
- Marketplace listing in [namos2502/agent-plugins](https://github.com/namos2502/agent-plugins)

### Known limitations
- Requires GitHub Copilot CLI (`brew install copilot-cli`) — separate from `gh copilot`
- Requires active Copilot subscription and `copilot login` before use
- Commands are namespaced as `/xflow:*` when installed as a plugin
