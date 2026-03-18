---
description: "Verify GitHub Copilot CLI is installed and authenticated. Run this before using any other cofluent commands."
---

Run the following checks and report the results clearly to the user:

**Step 1 — Check if Copilot CLI is installed:**
```bash
which copilot && copilot --version
```

**Step 2 — Check if the user is authenticated:**
```bash
copilot -p "say hello" -s --no-ask-user --no-auto-update --no-color --allow-tool='read' --model=claude-haiku-4.5
```

Based on the results:

- ✅ If both steps succeed — tell the user CoFluent is ready and list the available commands: `/cofluent:ask`, `/cofluent:suggest`, `/cofluent:explain`, `/cofluent:fix`, `/cofluent:review`
- ❌ If `copilot` is not found — show install instructions:
  ```bash
  brew install copilot-cli   # macOS
  # or
  npm install -g @github/copilot
  ```
- ❌ If authentication fails — tell the user to run:
  ```bash
  copilot login
  ```
  then re-run `/cofluent:init` to verify.
