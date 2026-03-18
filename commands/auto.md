---
description: "Activate autonomous Copilot CLI mode — Claude will know when and how to invoke copilot -p for the rest of this session"
---

Load the copilot-cli skill reference into your context for this session.

From now on, whenever a task would benefit from delegating to the GitHub Copilot CLI, use `copilot -p` with the correct programmatic flags from the skill reference — without the user needing to ask explicitly.

Confirm to the user that copilot-aware mode is active and that you will autonomously use `copilot -p` when appropriate. Remind them that explicit commands are still available if they prefer direct control:

- `/cofluent:ask` — ask Copilot a question
- `/cofluent:suggest` — get a shell command suggestion
- `/cofluent:explain` — explain a command or snippet
- `/cofluent:fix` — fix a bug or error
- `/cofluent:review` — review code
- `/cofluent:verify` — check Copilot CLI is installed and authenticated
