---
description: "Activate autonomous multi-agent mode — Claude will know when and how to invoke copilot -p or claude -p for the rest of this session"
---

**Step 1 — Check if setup has been run:**

Read `~/.claude/CLAUDE.md` and `~/.copilot/copilot-instructions.md`. Check whether each contains a `## xFlow` section.

- If **neither** file has the section — stop and tell the user:
  > ⚠️ xFlow is not set up yet. Run `/xflow:setup` first to detect your CLI agents and register xFlow, then run `/xflow:auto` again.

- If **only one** file has the section — warn the user setup may be incomplete and suggest re-running `/xflow:setup`, then proceed to Step 2.

- If **both** files have the section — proceed to Step 2.

**Step 2 — Load skills and activate:**

Read and load the following skill files into your context for this session:

- `skills/orchestration/SKILL.md` — the control center orchestration protocol
- `skills/agents/copilot-cli/SKILL.md` — Copilot CLI behavioral reference
- `skills/agents/claude-cli/SKILL.md` — Claude CLI behavioral reference

From now on, operate as the xFlow control center for this session. Apply the orchestration protocol automatically whenever a task would benefit from cross-CLI delegation — without the user needing to ask explicitly.

Confirm to the user that xFlow multi-agent mode is active. Let them know:
- You will delegate subtasks to the right CLI agent based on the decision tree
- Every delegated agent will self-verify before reporting back
- You will review each report before proceeding to the next task
- They can run `/xflow:setup` if any agent needs authentication
