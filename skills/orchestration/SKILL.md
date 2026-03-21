---
name: xFlow Orchestration
description: Cross-agent orchestration protocol. Teaches the host CLI to act as a persistent control center — decomposing tasks, delegating to CLI agents, receiving verified reports, and managing workflow state.
when_to_use: always — loaded at session start when xFlow is installed
version: 3.0.0
---

# xFlow Orchestration

## Overview

xFlow turns your active CLI into a **control center** that delegates subtasks to other CLI agents. Each agent executes in isolation, self-verifies, and returns a structured report. You stay in context — the result comes back, the noise stays out.

**Core principle:** Delegate with a clear reason. Review every report before proceeding.

**Violating the letter of this protocol is violating its spirit.**

## The Iron Law

```
NO CROSS-CLI DELEGATION WITHOUT A CLEAR REASON.
NO PROCEEDING WITHOUT REVIEWING THE REPORT.
```

## The Three-Tier Model

```
CONTROL CENTER (your active CLI)
  └── MANAGER (optional — receives complex subtask, orchestrates internally)
        └── ENGINEER (leaf — execute → self-verify → report)
```

The control center holds the plan. Managers decompose internally. Engineers execute and report. Every tier returns the same structured report — the control center never needs to know how deep the work went.

## When to Delegate Cross-CLI

**Delegate when:**
1. **Platform-specific** — GitHub ops (PRs, repos, Actions) → Copilot; Anthropic reasoning or Claude-specific model → Claude CLI
2. **Context isolation** — offload a long subtask so its intermediate work never enters your context (the final result DOES return via stdout)
3. **Different model needed** — the target CLI runs a model the host cannot

**Do NOT delegate when:**
1. Task needs your current session context, open files, or in-memory state
2. The host has a native subagent that can handle it
3. The target CLI is not installed or not authenticated
4. You have no clear reason — "big task" is not a reason

**Decision tree:**
```
New task →
  Platform-specific? YES → cross-CLI
  Context isolation / different model? YES → cross-CLI
  Host native subagent available? YES → use it (faster, no auth needed)
  Default → handle in current context
```

## Control Center Protocol

1. **Decompose** — Break into scoped, independently executable subtasks. Each must be verifiable by the agent itself.
2. **Route** — Apply decision tree. Check agent availability (see below).
3. **Dispatch** — Use delegation prompt template. Always include scope, success criteria, report format.
4. **Review** — Read STATUS first. Spot-check if needed (`git diff`, tests). Decide: proceed, re-assign, or adjust.
5. **Track** — Update state (done / pending / failed). Never skip to the next subtask without reviewing the current report.

## Delegation Prompt Template

```
[Task]: <one sentence — what needs to be done>
[Context]: <only what the agent strictly needs>
[Success criteria]: <how the agent knows it's done>
[Report format]: Return ONLY this when finished:
  STATUS: ✅ Verified / ⚠️ Partial / ❌ Failed
  SUMMARY: <1-2 sentences>
  STEPS: <bullets>
  FILES: <changed files or "none">
  ISSUES: <notes for the control center or "none">
  Max 150 words.
```

Keep prompts lean. No project dumps.

## Structured Report Format

Every agent, every tier, every time:

```
STATUS: ✅ Verified / ⚠️ Partial / ❌ Failed
SUMMARY: <1-2 sentences>
STEPS:
  - <step 1>
  - <step 2>
FILES: <list or "none">
ISSUES: <notes or "none">
```

**Never return raw output or tool call dumps to the control center.**

## Self-Verify Before Reporting (when YOU are the agent)

Execute → Verify → (fix if needed) → Report.

1. Run the task
2. Check your own work — tests, file contents, expected state
3. If verification fails → fix, then verify again
4. Only then → write the structured report

| Status | Meaning |
|--------|---------|
| ✅ Verified | You actively checked. It works. |
| ⚠️ Partial | Some parts verified, some could not be checked. Explain in ISSUES. |
| ❌ Failed | Describe what was attempted and what failed. |

**Never mark ✅ without actually checking.**

## Agent Detection

```bash
# Copilot CLI
which copilot 2>/dev/null && copilot -p "ping" -s --no-ask-user --no-auto-update --no-color --allow-tool='read' --model=claude-haiku-4.5 2>/dev/null | head -1

# Claude CLI
which claude 2>/dev/null && claude -p "ping" --output-format text --allowedTools "Read" --max-turns 1 --no-session-persistence 2>/dev/null | head -1
```

If unavailable: fall back to another agent, handle natively, or tell the user to run `/xflow:setup`.

## Error Handling

| STATUS | Control center action |
|--------|-----------------------|
| ✅ | Accept, proceed |
| ⚠️ | Read ISSUES, accept partial or re-assign unverified parts |
| ❌ | Read ISSUES. Re-assign, adjust scope, or handle natively |
| No output | Retry once. If still failing, handle natively. |
| Auth failure | Tell user: `/xflow:setup` |

**After 2 consecutive ❌ on the same subtask — stop and discuss with the user before retrying.**

## Cross-Agent Chaining

Pass excerpts of a report as context into the next delegation prompt. Max 2 levels of cross-CLI delegation below the control center. Managers use their platform-native tools — no further cross-CLI chaining at the leaf level.

## Red Flags — STOP

- Delegating without a clear reason ("it's a big task")
- Moving to the next subtask without reading the report
- Returning raw output instead of a structured report
- Marking ✅ without running verification
- Retrying a ❌ task without reading ISSUES first
- Delegating a task that needs current session context
- Chaining more than 2 levels deep

## Quick Reference

| I want to… | Do this |
|------------|---------|
| GitHub task | Copilot — see `skills/agents/copilot-cli/SKILL.md` |
| General code task | Claude CLI — see `skills/agents/claude-cli/SKILL.md` |
| Add a new agent | Drop `skills/agents/<name>/SKILL.md` — follow existing format |
| Handle auth failure | Tell user: `/xflow:setup` |
| Handle ❌ report | Read ISSUES. Re-assign or handle natively. Never retry blindly. |
