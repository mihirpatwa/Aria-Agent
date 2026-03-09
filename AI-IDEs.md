# Using Aria with AI IDEs and AI CLIs

Aria is intentionally **AI-agnostic**. The runtime now supports two integration styles:

- **Direct CLI dispatch** for tools like Claude, Gemini, Codex, Auggie, or any compatible custom CLI
- **Agnostic IDE handoff** for Cursor, Windsurf, Antigravity, and other assistants that can read files and edit code

## IDE mode (`agnostic`)

Use this when your AI assistant lives inside the editor instead of a terminal CLI.

```bash
ARIA_AGENT_MODE=agnostic AI_IDE_NAME="Cursor" ./run.sh "Analyze the avatar sync issue and fix it"
```

Aria will create:

- `output/agnostic-instructions.md`
- `output/azure-report.md`
- `output/post-action-report.md`

The IDE assistant should:

1. read `agnostic-instructions.md`
2. produce or update the requested agent reports
3. make code changes if needed
4. verify the result
5. stop before commit / push / Azure update until explicit approval is given

## Direct CLI mode

You can route agents through any CLI that supports either:

- a **single prompt argument**, or
- **stdin prompt input**

### Common examples

```bash
# Claude CLI
AI_ENGINE="claude"

# Gemini-style prompt flag
AI_ENGINE="gemini"
AI_BIN="gemini"
AI_CLI_STYLE="single_prompt"
AI_PROMPT_FLAG="-p"

# GitHub Copilot / Codex-style wrapper
AI_ENGINE="codex"
AI_BIN="gh"
AI_EXTRA_ARGS="copilot"
AI_CLI_STYLE="single_prompt"
AI_PROMPT_FLAG="-p"

# Custom or Auggie-like CLI
AI_ENGINE="auggie"
AI_BIN="auggie"
AI_CLI_STYLE="stdin"
```

## Approval-gated delivery flow

After a fix is verified, Aria prepares a delivery plan in `output/post-action-report.md`.

Sensitive actions are **not executed by default**:

- git branch creation / commit
- git push
- Azure ticket comment / state transition

Approve them explicitly on the next run:

```bash
ARIA_APPROVE_SENSITIVE_ACTIONS=commit,push,azure ./run.sh "same request"
```

Optional Azure transition:

```bash
AZURE_TRANSITION_STATE="Resolved"
```

## Why this pattern works

Aria separates:

- **orchestration** → `run.sh`
- **agent behavior** → `prompts/*.md`
- **memory** → `memory/*.json`
- **delivery approval** → `output/post-action-report.md`

That makes it portable across CLIs, AI IDEs, and future assistant runtimes without rewriting the workflow logic.
