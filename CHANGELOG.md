# Changelog

All notable changes to CoFluent will be documented here.

## [1.0.0-beta] — 2026-03-18

Initial beta release. Core plugin structure is stable; commands and skill reference are functional but may evolve based on feedback.

### Added
- `skills/copilot-cli/SKILL.md` — core programmatic reference for the GitHub Copilot CLI; auto-loaded by Claude when invoking `copilot -p`
- `/cofluent:verify` — checks that Copilot CLI is installed and authenticated; guides the user through fixing any issues
- `/cofluent:ask` — delegates a question to Copilot (read-only, fast, uses `claude-haiku-4.5`)
- `/cofluent:suggest` — asks Copilot to suggest a shell command for a task
- `/cofluent:explain` — asks Copilot to explain a command, error, or code snippet
- `/cofluent:fix` — asks Copilot to fix a bug or error (read + write file permissions)
- `/cofluent:review` — runs the Copilot `/review` agent on staged diff or a specific file
- `/cofluent:help` — shows the full command reference
- `.claude-plugin/plugin.json` — plugin manifest for installation via Claude Code plugin system
- Marketplace listing in [namos2502/agent-plugins](https://github.com/namos2502/agent-plugins)

### Known limitations
- Requires GitHub Copilot CLI (`brew install copilot-cli`) — separate from `gh copilot`
- Requires active Copilot subscription and `copilot login` before use
- Commands are namespaced as `/cofluent:*` when installed as a plugin
