---
description: "Remove xFlow configuration added by /xflow:setup"
---

Remove the xFlow sections that were added during setup. Run the following steps in order.

**Step 1 — Remove from ~/.claude/CLAUDE.md:**

Read `~/.claude/CLAUDE.md`. Find the `## xFlow` section — it starts at the `## xFlow` heading and ends before the next `##` heading (or end of file).

- If the section **exists** — remove it (including any blank lines immediately before it) and save the file.
- If it **does not exist** — skip and note it was already absent.

**Step 2 — Remove from ~/.copilot/copilot-instructions.md:**

Read `~/.copilot/copilot-instructions.md`. Find the `## xFlow` section using the same rule above.

- If the section **exists** — remove it (including any blank lines immediately before it) and save the file.
- If it **does not exist** — skip and note it was already absent.

**Step 3 — Confirm:**

Report which files were updated and which were already clean. Remind the user to run `/plugin uninstall xflow` in Claude Code to fully remove the plugin.
