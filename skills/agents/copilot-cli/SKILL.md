---
name: Copilot CLI Agent
description: Behavioral reference for GitHub Copilot CLI. Use when delegating GitHub-specific or GitHub-adjacent tasks via `copilot -p`.
when_to_use: when delegating to Copilot CLI — see skills/orchestration/SKILL.md for routing decision
version: 3.0.0
---

# Copilot CLI Agent

## When to Use

**Use Copilot CLI for:**
- GitHub-specific tasks — PRs, issues, repos, Actions, branches, code review
- Git workflow tasks where GitHub context helps
- Tasks where Copilot's GitHub-native tooling is an advantage

**Do NOT use Copilot CLI for:**
- General code explanations or refactors (handle natively or use Claude CLI)
- Tasks that need your current session context
- When Copilot is not installed or not authenticated

## Core Flags

| Flag | Purpose |
|------|---------|
| `-p "PROMPT"` | Non-interactive mode — exits when done |
| `-s` | Silent — suppress decoration, output only the response |
| `--no-ask-user` | Prevent agent from pausing to ask questions |
| `--no-auto-update` | Suppress update checks |
| `--no-color` | Plain text output |

Always combine `-s --no-ask-user --no-auto-update --no-color` for clean programmatic output.

## Tool Permissions (`--allow-tool`)

| Use case | Flag |
|----------|------|
| Read-only (questions, analysis) | `--allow-tool='read'` |
| Read + write files | `--allow-tool='write, read'` |
| Read + write + git | `--allow-tool='write, shell(git:*), read'` |
| Read + write + git + npm | `--allow-tool='write, shell(git:*), shell(npm run:*), read'` |

Use the narrowest permission set that gets the job done.

## Model Selection

| Task | Model |
|------|-------|
| Quick question, analysis | `--model=claude-haiku-4.5` |
| Complex fix, multi-step | omit (defaults to Opus) |

## Invocation Patterns

**Read-only delegation (question, analysis, review):**
```bash
copilot -p "[delegation prompt]" -s --no-ask-user --no-auto-update --no-color \
  --allow-tool='read' --model=claude-haiku-4.5
```

**Write delegation (fix, implement):**
```bash
copilot -p "[delegation prompt]" -s --no-ask-user --no-auto-update --no-color \
  --allow-tool='write, shell(git:*), read'
```

**Code review (built-in /review agent):**
```bash
copilot -p "/review [scope]" -s --no-ask-user --no-auto-update --no-color \
  --allow-tool='shell(git:*), read'
```

Redirect stderr if needed: add `2>/dev/null`

## Delegation Prompt

Follow the template from `skills/orchestration/SKILL.md`. Include the structured report format instructions at the end of every prompt.

## Handling the Report

The agent's stdout is its report. Capture it directly:
```bash
REPORT=$(copilot -p "[prompt]" -s --no-ask-user --no-auto-update --no-color --allow-tool='read' 2>/dev/null)
```

Read STATUS first. If ⚠️ or ❌, read ISSUES before deciding next action.

## Error Handling

| Failure | Action |
|---------|--------|
| Command not found | Tell user: `brew install copilot-cli` |
| Auth failure / no output | Tell user: `copilot login` then re-run `/xflow:setup` |
| Unexpected output format | Retry once. If still malformed, handle the task natively. |

## Chaining

Pass the SUMMARY + STEPS excerpt from this agent's report as [Context] in the next delegation prompt. Do not pass raw output.
