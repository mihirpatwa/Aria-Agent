# Universal Setup Guide for Aria Agent

This guide will help you configure Aria Agent to work with **any** Azure DevOps project.

## 🎯 What You'll Need

Before starting, ensure you have:

- ✅ Access to an Azure DevOps organization and project
- ✅ A Personal Access Token (PAT) for Azure DevOps
- ✅ Your Azure DevOps display name (exact match)
- ✅ An AI IDE or CLI (Claude Code, Cursor, Windsurf, etc.)
- ✅ Git installed on your machine

## 📋 Step-by-Step Setup

### Step 1: Clone or Add Aria Agent to Your Project

**Option A: As a Git Submodule (Recommended)**

```bash
# Navigate to your project root
cd /path/to/your/project

# Add Aria Agent as a submodule
git submodule add https://github.com/your-org/Aria-Agent.git Aria-Agent

# Initialize the submodule
git submodule update --init --recursive
```

**Option B: Direct Copy**

```bash
# Navigate to your project root
cd /path/to/your/project

# Copy the Aria-Agent directory
cp -r /path/to/Aria-Agent .
```

### Step 2: Find Your Azure DevOps Configuration

#### 2.1 Find Your Azure Organization

1. Log in to [Azure DevOps](https://dev.azure.com/)
2. Look at the URL in your browser address bar
3. Your organization name is the first part after `dev.azure.com/`

**Example:**
- URL: `https://dev.azure.com/mycompany/_projects`
- Your organization: `mycompany`

#### 2.2 Find Your Azure Project

1. In Azure DevOps, click on **Projects** in the top navigation
2. Select your project from the list
3. Look at the URL: `https://dev.azure.com/{org}/{project}/_workitems/`
4. Your project name is after your organization name

**Example:**
- URL: `https://dev.azure.com/mycompany/E-Commerce/_workitems/`
- Your project: `E-Commerce`

#### 2.3 Find Your Display Name (Assignee)

1. In Azure DevOps, click on your profile picture in the top-right corner
2. Your display name appears in the dropdown
3. **Important**: Use the EXACT display name as it appears in Azure DevOps

**Examples:**
- `John Smith`
- `Jane Doe`
- `Development Team`

#### 2.4 Generate a Personal Access Token (PAT)

1. In Azure DevOps, click on your profile picture
2. Select **Personal access tokens** (or go to `https://dev.azure.com/{your-org}/_usersSettings/tokens`)
3. Click **+ New Token**
4. Configure the token:
   - **Name**: `Aria Agent` (or any descriptive name)
   - **Organization**: Your organization (select from dropdown)
   - **Expiration**: Choose an expiration date (90 days recommended)
   - **Scopes**: Click **Show all scopes**
     - ✅ **Work Items** → **Read** (required)
     - ✅ **Work Items** → **Write** (required for updating tickets)
5. Click **Create**
6. **Important**: Copy the token immediately - you won't see it again!

### Step 3: Configure Environment Variables

1. Copy the example environment file:
   ```bash
   cp Aria-Agent/.env.example Aria-Agent/.env
   ```

2. Edit `Aria-Agent/.env` with your configuration:
   ```bash
   # Azure DevOps Configuration
   AZURE_ORG="mycompany"                    # Your Azure org (from Step 2.1)
   AZURE_PROJECT="E-Commerce"               # Your project name (from Step 2.2)
   AZURE_PAT="your-pat-token-here"          # Your PAT token (from Step 2.4)
   AZURE_ASSIGNEE="John Smith"              # Your display name (from Step 2.3)
   ```

3. **Optional**: Configure QA testing (if you want automated browser testing):
   ```bash
   # QA Automation Configuration
   QA_ENABLE_CHROME_MCP="true"
   QA_DEV_SERVER_URL="http://localhost:3000"
   QA_TEST_EMAIL="your-test@example.com"
   QA_TEST_PASSWORD="your-test-password"
   ```

### Step 4: Configure AI IDE Integration

Choose your AI platform and follow the appropriate setup:

#### Claude Code

Create `.claude/claude_desktop_config.json` in your project root:

```json
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": ["-y", "chrome-devtools-mcp@latest"]
    }
  }
}
```

#### Cursor

Create `.cursor/mcp.json` in your project root:

```json
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": ["-y", "chrome-devtools-mcp@latest"],
      "disabled": false
    }
  }
}
```

#### Windsurf

Create `.windsurf/mcp.json` in your project root:

```json
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": ["-y", "chrome-devtools-mcp@latest"]
    }
  }
}
```

#### Other AI IDEs

See [SETUP-PROMPT.md](SETUP-PROMPT.md) for configurations for other platforms.

### Step 5: Verify Your Setup

1. **Test Azure DevOps Connection**:
   ```bash
   curl -u :$AZURE_PAT \
     "https://dev.azure.com/$AZURE_ORG/_apis/projects?api-version=7.1"
   ```

   You should see a JSON response with your project details.

2. **Verify Configuration Files**:
   ```bash
   # Check that .env exists
   ls -la Aria-Agent/.env

   # Verify .env is not tracked by git
   grep "Aria-Agent/.env" .gitignore
   ```

### Step 6: Activate Aria Agent

**In your AI IDE**, use one of these wake phrases:

- `"Hey Aria, good morning"`
- `"Hey Aria, wake up"`
- `"Aria, triage my tickets"`
- `"Aria, show me all open tickets"`

Aria will automatically:
1. Read `Aria-Agent/ARIA-AUTONOMOUS.md`
2. Fetch your Azure DevOps tickets
3. Begin the autonomous workflow

## 🔧 Troubleshooting

### Problem: "No tickets found"

**Solution**: Verify these settings in `Aria-Agent/.env`:

1. **Organization**: Check `AZURE_ORG` matches your Azure DevOps URL
2. **Project**: Check `AZURE_PROJECT` matches exactly
3. **Assignee**: Check `AZURE_ASSIGNEE` matches your display name EXACTLY
4. **Permissions**: Verify your PAT token has **Work Items → Read** permission

**Debug Steps**:
```bash
# Test your credentials manually
curl -u :$AZURE_PAT \
  "https://dev.azure.com/$AZURE_ORG/$AZURE_PROJECT/_apis/wit/workitems?api-version=7.1"
```

### Problem: "Authentication error"

**Solution**:

1. Verify your PAT token is valid:
   ```bash
   curl -u :$AZURE_PAT \
     "https://dev.azure.com/$AZURE_ORG/_apis/projects?api-version=7.1"
   ```

2. Check PAT token expiration:
   - Go to `https://dev.azure.com/{your-org}/_usersSettings/tokens`
   - Verify the token hasn't expired
   - Regenerate if necessary

3. Ensure PAT token has required permissions:
   - ✅ Work Items → Read
   - ✅ Work Items → Write

### Problem: "Wrong organization or project"

**Solution**:

1. List your available organizations:
   ```bash
   curl -u :$AZURE_PAT \
     "https://dev.azure.com/_apis/projects?api-version=7.1"
   ```

2. Find your organization name in the response
3. Update `AZURE_ORG` in `Aria-Agent/.env`

### Problem: "Chrome MCP not available"

**Solution**:

1. **Check if Chrome MCP is installed**:
   ```bash
   npx -y chrome-devtools-mcp@latest --version
   ```

2. **Install Chrome MCP**:
   ```bash
   npx -y chrome-devtools-mcp@latest
   ```

3. **Restart your AI IDE** after creating MCP configuration files

4. **Disable QA automation** if not needed:
   ```bash
   # In Aria-Agent/.env
   QA_ENABLE_CHROME_MCP="false"
   ```

### Problem: "Aria doesn't wake up"

**Solution**:

1. **Verify ARIA-AUTONOMOUS.md exists**:
   ```bash
   ls -la Aria-Agent/ARIA-AUTONOMOUS.md
   ```

2. **Check AI IDE configuration**:
   - For Cursor: Verify `.cursorrules` exists
   - For Windsurf: Verify `.windsurfrules` exists
   - For Claude Code: Verify `.claude/SYSTEM.md` exists

3. **Manual activation**:
   ```
   Read Aria-Agent/ARIA-AUTONOMOUS.md and initialize: Hey Aria, wake up
   ```

## 🎯 Best Practices

### Security

- ✅ Never commit `Aria-Agent/.env` to version control
- ✅ Rotate PAT tokens every 90 days
- ✅ Use minimal required permissions for PAT tokens
- ✅ Keep test credentials separate from production

### Configuration

- ✅ Use exact display names for `AZURE_ASSIGNEE`
- ✅ Test PAT tokens before saving to `.env`
- ✅ Verify all required permissions are granted
- ✅ Keep `.env.example` updated with any new variables

### Performance

- ✅ Use Git submodules for easier updates
- ✅ Regularly pull latest Aria Agent updates
- ✅ Monitor Azure DevOps API rate limits
- ✅ Cache frequently accessed data in memory files

## 📚 Additional Resources

- [Azure DevOps REST API Documentation](https://docs.microsoft.com/en-us/rest/api/azure/devops/)
- [Personal Access Token Authentication](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate)
- [WIQL Query Syntax](https://docs.microsoft.com/en-us/azure/devops/boards/queries/wiql-syntax)
- [README.md](README.md) - Main documentation
- [QUICKSTART.md](QUICKSTART.md) - Quick reference guide
- [AZURE-SETUP.md](AZURE-SETUP.md) - Detailed Azure configuration
- [ENVIRONMENT-SAFETY.md](ENVIRONMENT-SAFETY.md) - Safety guarantees

## 🎉 You're Ready!

Once configured, Aria will:

1. ✅ Fetch your Azure DevOps tickets automatically
2. ✅ Analyze your codebase for root causes
3. ✅ Generate production-ready fixes
4. ✅ Run tests with automatic retry logic
5. ✅ Perform automated browser testing (optional)
6. ✅ Update pattern memory for continuous learning
7. ✅ Commit and push changes (with approval)

**Start using Aria:**
```bash
# In your AI IDE
"Hey Aria, wake up"
"Aria, triage my tickets"
"Aria, fix the login bug"
```

---

**Need Help?** Check the [README.md](README.md) or open an issue on GitHub.

**Aria Agent** · Universal AI Engineering Assistant · Ready to help!
