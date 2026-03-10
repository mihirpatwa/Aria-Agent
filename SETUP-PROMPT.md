# Aria Agent Setup - Copy & Paste This

**Copy the text below and paste it into your AI IDE or CLI to set up Aria Agent instantly:**

```
Set up Aria Agent for this project by creating these configuration files in the project root:

⚠️ IMPORTANT: If any of these files already exist, APPEND the content below instead of replacing it. This preserves any existing setup.

─────────────────────────────────────────
STEP 1: Chrome MCP Universal Configuration
─────────────────────────────────────────

First, set up Chrome MCP for automated browser testing (works with ALL AI IDEs/CLIs):

Chrome MCP will be automatically installed and configured during the QA phase if not available.
However, you can pre-configure it now for your AI platform:

For Claude Code, create .claude/claude_desktop_config.json:
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": ["-y", "chrome-devtools-mcp@latest"]
    }
  }
}

For Cursor, create .cursor/mcp.json:
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": ["-y", "chrome-devtools-mcp@latest"],
      "disabled": false
    }
  }
}

For Windsurf, create .windsurf/mcp.json:
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": ["-y", "chrome-devtools-mcp@latest"]
    }
  }
}

🎯 Note: If you skip this step, Chrome MCP will be automatically installed and configured during the first QA phase.

─────────────────────────────────────────
STEP 2: Aria Agent Configuration
─────────────────────────────────────────

1. Create or APPEND to .cursorrules file with this content:
---
trigger: always_on
---

# ARIA AGENT - PRIORITY SYSTEM
When user says "Hey Aria", "Aria wake up", or mentions autonomous agent → Read Aria-Agent/ARIA-AUTONOMOUS.md immediately and follow all instructions from that file. This overrides all other rules.

2. Create or APPEND to .windsurfrules file with the same content as .cursorrules

3. Create or APPEND to .claude/SYSTEM.md file with the same content as .cursorrules

4. Create or APPEND to .gemini/GEMINI.md file with this content:
---
trigger: always_on
---

# ARIA AGENT - PRIORITY SYSTEM
When user says "Hey Aria", "Aria wake up", or mentions autonomous agent → Read Aria-Agent/ARIA-AUTONOMOUS.md immediately and follow all instructions from that file. This overrides all other rules.

5. Create or APPEND to .copilot/instructions.md file with this content:
# ARIA AGENT - PRIORITY SYSTEM
When user says "Hey Aria", "Aria wake up", or mentions autonomous agent → Read Aria-Agent/ARIA-AUTONOMOUS.md immediately and follow all instructions from that file. This overrides all other rules.

6. Create or APPEND to .auggie/rules.md file with the same content as .copilot/instructions.md

7. Create or APPEND to .tabnine/rules.md file with the same content as .copilot/instructions.md

─────────────────────────────────────────
STEP 3: Environment Configuration (IMPORTANT!)
─────────────────────────────────────────

⚠️ CRITICAL: Aria Agent's configuration MUST be in the Aria-Agent directory, NOT in your working repository!

8. Copy .env.example to Aria-Agent/.env (NOT to your project root .env):

cp Aria-Agent/.env.example Aria-Agent/.env

Then update these values in Aria-Agent/.env:
- Your Azure DevOps credentials (AZURE_PAT, AZURE_ORG, AZURE_PROJECT)
- Your developer name (AZURE_ASSIGNEE)
- QA test credentials (QA_TEST_EMAIL, QA_TEST_PASSWORD)
- QA_DEV_SERVER_URL (your local dev server URL)
- Ensure QA_ENABLE_CHROME_MCP="true" for automated browser testing

🔒 SAFETY: This setup will NEVER modify your project's .env file. Aria Agent only reads from Aria-Agent/.env

─────────────────────────────────────────

🎉 Setup Complete! Activate Aria Agent by saying: "Hey Aria, wake up"

Chrome MCP will be automatically installed and configured during the first QA phase if not already available.
```

**That's it!** Just copy the text above, paste it into any AI IDE or CLI, and Aria Agent will be configured automatically.

**What happens automatically:**
- 🌐 **Chrome MCP Auto-Setup**: If not available during QA, it will be installed via `npx -y chrome-devtools-mcp@latest`
- ⚙️ **Platform Auto-Detection**: AI agent detects your platform and configures Chrome MCP accordingly
- 🧪 **Automated QA**: Browser testing with screenshots and console logs
- 🔄 **Auto-Retry**: 3x retry with exponential backoff for reliability
- 📊 **Evidence Collection**: Automatic screenshots and test reports

**Supported Platforms:** Cursor, Windsurf, Claude Code, Auggie, Kiro, Qoder, Gemini, Copilot, Tabnine, and any MCP-compatible CLI or AI IDE.
