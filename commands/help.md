---
description: "Show xFlow commands and skill reference"
---

Show the user the xFlow skill structure and available commands:

## Skills (auto-loaded when plugin is installed)

| Skill | What it does |
|-------|-------------|
| `skills/orchestration/SKILL.md` | Control center protocol — routing, delegation, report format, self-verify loop |
| `skills/agents/copilot-cli/SKILL.md` | Copilot CLI behavioral reference — when to use, flags, invocation patterns |
| `skills/agents/claude-cli/SKILL.md` | Claude CLI behavioral reference — when to use, flags, invocation patterns |

## Commands

| Command | What it does |
|---------|-------------|
| `/xflow:setup` | One-time setup — detect agents, authenticate, register xFlow in `~/.claude/CLAUDE.md` |
| `/xflow:auto` | Explicitly load all skills and activate orchestration mode for this session |
| `/xflow:help` | Show this reference |

## Adding a new agent

Drop a `skills/agents/<name>/SKILL.md` file following the existing agent format. No other changes needed.
