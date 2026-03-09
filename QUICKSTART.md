# Aria Quick Start Guide (Fully Autonomous Mode)

Aria is now a **fully autonomous** engineering assistant with automatic retry logic. She executes the complete workflow without intervention, asking for approval ONLY at commit/push stage.

## 🚀 Get Started in 2 Steps

### Step 1: Initialize Environment

Ensure `Aria-Agent/.env` is configured with your Azure credentials:

```bash
# Azure DevOps Configuration
AZURE_ORG="aauti"
AZURE_PROJECT="HealthCare"
AZURE_PAT="your-pat-token"
AZURE_ASSIGNEE="Mihir Patwa"

# Delivery Automation (optional - for full automation)
ARIA_APPROVE_SENSITIVE_ACTIONS="false"  # Set to "true" or "commit,push,azure" for full autonomy

# QA Automation (optional)
QA_ENABLE_CHROME_MCP="true"
QA_DEV_SERVER_URL="http://localhost:8081"
QA_TEST_EMAIL="erik@mailinator.com"
QA_TEST_PASSWORD="Robert@123"
```

### Step 2: Wake Aria

Simply talk to Aria in your AI-enabled editor (Antigravity, Cursor, Windsurf, Claude Code, etc.).

**Examples:**

- "Hey Aria, show me all open tickets assigned to Mihir"
- "Aria, analyze the HeyGen lip-sync bug and provide a fix"
- "Aria, triage my board for a morning update"
- "Aria, run full autonomous workflow on ticket #123456"

## 🧠 How It Works

Aria follows the master instruction set in `Aria-Agent/ARIA-AUTONOMOUS.md`. She will:

1.  **Triage**: Fetch tickets from Azure DevOps (with 3x auto-retry)
2.  **Analyze**: Scan the codebase for root causes (with 3x auto-retry)
3.  **Fix**: Generate production-ready patches (with 3x auto-retry)
4.  **Verify**: Run project tests and linting (with 3x auto-retry, multiple commands)
5.  **QA**: Automated browser testing via Chrome MCP (with 3x auto-retry)
6.  **Memory**: Update pattern and history tracking (with 3x auto-retry)
7.  **Deliver**: Commit and push (ONLY stage asking for approval)

### 🔁 Automatic Retry Behavior

Every stage has built-in resilience:

- **3 attempts maximum** per stage
- **Exponential backoff**: 2s → 4s → 8s
- **Continue on error**: If a stage fails after 3 retries, Aria logs it and continues to the next stage
- **Never stops** except at delivery approval gate (unless auto-approved)

### ⚠️ Approval Workflow

**Default behavior**: Aria will ask for approval ONLY before committing/pushing:

1. Aria completes all 6 autonomous stages automatically
2. Aria generates `output/delivery-approval.md` with summary
3. You review the changes in `output/` directory
4. You respond with "APPROVE" or "LGTM"
5. Aria commits, pushes, and comments on Azure ticket

**Full automation**: Set `ARIA_APPROVE_SENSITIVE_ACTIONS="commit,push,azure"` in `.env` to skip approval entirely.

## 📂 Core Files

- `ARIA-AUTONOMOUS.md`: The master instruction set with retry logic
- `memory/`: Persistent storage for patterns and history
- `output/`: Audit logs for every autonomous action
- `prompts/`: Agent-specific instructions with auto-retry behavior

## 📊 Output Reports

After each run, check `Aria-Agent/output/`:

- `azure-report.md` - Triage results
- `code-report.md` - Root cause analysis
- `fix-report.md` - Fix specification
- `verify-report.md` - Test results
- `qa-report.md` - Browser testing results
- `memory-report.md` - Pattern updates
- `final-report.md` - Comprehensive summary

## 🎯 What Makes Aria Different

**Other agents**: Ask for help at every step, fail on first error, require constant supervision

**Aria**:

- ✅ Executes entire workflow autonomously
- ✅ Retries failed operations automatically (3x with exponential backoff)
- ✅ Continues on error (reports but doesn't stop)
- ✅ Only asks for approval at commit/push stage
- ✅ Generates detailed reports for review
- ✅ Learns from patterns over time

---

**Aria Autonomous System** · Fully Autonomous with Auto-Retry · Ready to help.
