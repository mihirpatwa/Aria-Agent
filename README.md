# Aria Autonomous Multi-Agent System

An intelligent, fully autonomous engineering assistant. Aria orchestrates specialized roles for ticket triage, code analysis, fix generation, and verified delivery using a markdown-driven instruction set.

## 🎯 True Autonomous Workflow

Aria operates FULLY AUTONOMOUSLY with automatic retry logic:

- ✅ **Fix automatically** - Generates patches without intervention
- ✅ **Test automatically** - Runs tests with 3x retry and exponential backoff
- ✅ **QA automatically** - Chrome MCP browser testing with auto-retry
- ✅ **Retry automatically** - Exponential backoff (2s → 4s → 8s) on failures
- ⚠️ **Ask only for commit/push approval** - ONLY stage requiring human approval

## 🧠 Autonomous Architecture

Aria operates directly through your AI IDE by following `ARIA-AUTONOMOUS.md`.

### 7-Stage Autonomous Pipeline:

1. **Triage** - Fetches live Azure DevOps tickets (auto-retry 3x)
2. **Analysis** - Scans codebase for root causes (auto-retry 3x)
3. **Fix** - Generates production-ready patches (auto-retry 3x)
4. **Verify** - Runs `test`, `lint`, or `build` (auto-retry 3x, multiple commands)
5. **QA** - Automated browser testing via Chrome MCP (auto-retry 3x)
6. **Memory** - Updates persistent JSON patterns and history (auto-retry 3x)
7. **Delivery** - Git commit, push, and Azure comments (**ONLY STAGE NEEDING APPROVAL**)

### 🔁 Automatic Retry Behavior

Every stage (except delivery) has built-in retry logic:

- **Maximum 3 attempts** per stage
- **Exponential backoff**: 2 seconds → 4 seconds → 8 seconds
- **On failure**: Log error, wait, retry automatically
- **After 3 failures**: Skip stage, continue to next, report in summary
- **Never asks for help** except at delivery stage

## 🚀 Usage

No script required. Just wake Aria with a natural language request in your AI-enabled editor.

### Example Requests:

- "Hey Aria, triage my tickets"
- "Aria, fix the Deepgram WebSocket issue"
- "Aria, what are our most common bug patterns this week?"
- "Aria, run full autonomous analysis on ticket #123456"
- "Hey Aria, wake up"

### 🛠️ Activating in any CLI (Claude, Gemini, etc.)

If you are using a standalone CLI that doesn't automatically read `.cursorrules` or `.windsurf` rules, you can activate Aria by manually pointing the agent to her master instructions in your first message:

> "Read `Aria-Agent/ARIA-AUTONOMOUS.md` and initialize: Hey Aria, wake up"

This ensures the agent adopts the Aria persona and follows the autonomous multi-stage pipeline.

### How It Works:

1. You give Aria a request
2. Aria reads `ARIA-AUTONOMOUS.md` and follows it step-by-step
3. Each stage runs automatically with retry logic
4. Reports are generated in `output/` directory
5. Only at delivery stage does Aria ask for approval (unless auto-approved)

## 📂 Directory Structure

```
Aria-Agent/
├── ARIA-AUTONOMOUS.md      # Master Autonomous Instruction Set
├── prompts/                # Role-specific instructions
│   ├── main-agent.md       # Core Persona (with autonomous behavior)
│   ├── agent-azure.md      # Triage Instructions
│   ├── agent-code.md       # Analysis Instructions (with auto-retry)
│   ├── agent-fix.md        # Generation Instructions (with auto-retry)
│   ├── agent-verify.md     # Verification Instructions (with auto-retry)
│   ├── agent-qa.md         # QA Instructions (Chrome MCP + auto-retry)
│   └── agent-memory.md     # Memory Instructions (with auto-retry)
├── memory/                 # Persistent JSON knowledge base
│   ├── patterns.json       # Bug pattern frequencies
│   ├── ticket-history.json # Ticket resolution history
│   └── agent-checklist.json # Run status tracking
├── output/                 # Agent execution reports
│   ├── azure-report.md     # Triage results
│   ├── code-report.md      # Root cause analysis
│   ├── fix-report.md       # Fix specification
│   ├── verify-report.md    # Test results
│   ├── qa-report.md        # QA automation results
│   ├── memory-report.md    # Pattern updates
│   └── final-report.md     # Comprehensive summary
└── README.md               # This file
```

## 🛠️ Configuration

Configure `Aria-Agent/.env` with:

### Azure DevOps Configuration:

```bash
AZURE_ORG="your-azure-org"              # Your Azure DevOps organization (e.g., "mycompany")
AZURE_PROJECT="your-project-name"       # Your Azure DevOps project (e.g., "E-Commerce")
AZURE_PAT="your-pat-token"              # Generate at: https://dev.azure.com/{your-org}/_usersSettings/tokens
AZURE_ASSIGNEE="your-name"              # Your display name in Azure DevOps
```

### Delivery Automation:

```bash
# Auto-approval options:
ARIA_APPROVE_SENSITIVE_ACTIONS="false"  # false | true | commit,push,azure
ARIA_ENABLE_POST_ACTIONS="true"         # Enable delivery actions
ARIA_GIT_REMOTE="origin"                # Git remote name
ARIA_PUSH_AFTER_COMMIT="true"           # Auto-push after commit
ARIA_AZURE_COMMENT_ON_COMPLETE="true"   # Comment on Azure tickets
```

### QA Automation:

```bash
QA_ENABLE_CHROME_MCP="true"                  # Enable Chrome MCP testing
QA_DEV_SERVER_URL="http://localhost:3000"    # Local dev server URL
QA_TEST_EMAIL="your-test-email@example.com"  # Test account email
QA_TEST_PASSWORD="your-test-password"        # Test account password
```

## 🚀 Quick Setup

1. **Configure Environment:**
   ```bash
   cp .env.example .env
   # Edit .env with your Azure DevOps configuration
   ```

2. **Find Your Azure Configuration:**
   - Organization: `https://dev.azure.com/{your-org}/`
   - Project: Navigate to your project in Azure DevOps
   - PAT Token: Generate at `https://dev.azure.com/{your-org}/_usersSettings/tokens`
   - Assignee: Your exact display name in Azure DevOps

3. **Wake Aria:**
   - In your AI IDE: "Hey Aria, wake up"
   - Or: "Read `Aria-Agent/ARIA-AUTONOMOUS.md` and initialize"

For detailed setup instructions, see [UNIVERSAL-SETUP.md](UNIVERSAL-SETUP.md).

## 🎯 Approval Workflow

By default, Aria will ask for approval ONLY at the delivery stage:

1. Aria completes all autonomous stages (triage → memory)
2. Aria generates a delivery approval request in `output/delivery-approval.md`
3. You review the changes in the output directory
4. You approve with "APPROVE" or "LGTM"
5. Aria proceeds with commit, push, and Azure comments

### Enable Full Automation:

Set in `.env`:

```bash
ARIA_APPROVE_SENSITIVE_ACTIONS="commit,push,azure"
```

Now Aria will run COMPLETELY autonomously from start to finish without any human intervention.

## 📊 Output Reports

Every autonomous run generates detailed reports:

- **azure-report.md** - Triage results and ticket categorization
- **code-report.md** - Root cause analysis with file details
- **fix-report.md** - Patch specification with before/after code
- **verify-report.md** - Test results with pass/fail status
- **qa-report.md** - Browser testing results with screenshots
- **memory-report.md** - Pattern memory updates
- **final-report.md** - Comprehensive summary of entire run

## 🔒 Safety Features

- **Automatic retry** prevents transient failures from stopping workflow
- **Exponential backoff** reduces system load during retries
- **Error logging** captures all failures for review
- **Approval gate** prevents unwanted commits (unless explicitly enabled)
- **Stage independence** - failure in one stage doesn't block others

---

**Aria Autonomous System** · Universal AI Agent · v4.0 (Fully Autonomous with Auto-Retry)
