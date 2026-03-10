# Azure DevOps Setup for Aria

## Your Configuration

**Assignee Name**: your-name (must match Azure DevOps display name exactly)
**Ticket Statuses**: To Do, In Progress, Reopen

## WIQL Query Used

Aria uses this exact WIQL query to fetch your tickets:

```sql
SELECT [System.Id],[System.Title],[System.State],[System.Priority],
       [System.Description],[System.WorkItemType],[System.CreatedDate],
       [System.Tags],[System.ChangedDate]
FROM WorkItems
WHERE [System.AssignedTo] CONTAINS '{assignee}'
  AND [System.State] IN ('To Do', 'In Progress', 'Reopen')
  AND [System.TeamProject] = '{project}'
ORDER BY [Microsoft.VSTS.Common.Priority] ASC
```

## Setup Instructions

### 1. Generate Azure DevOps PAT Token

1. Go to: https://dev.azure.com/{your-org}/_usersSettings/tokens
2. Click "Create New Token"
3. Name it: "ARIA Agents"
4. **Required Permissions**:
   - ✅ Work Items → Read
   - ✅ Work Items → Write (to update ticket states)
5. Copy the token

### 2. Configure Environment Variables

Create `.env` file in `Aria-Agent/`:

```bash
cd Aria-Agent
cp .env.example .env
```

Edit `.env` with your credentials:

```bash
# Azure org from your board URL (e.g., "mycompany" from https://dev.azure.com/mycompany/)
AZURE_ORG="your-azure-org"

# Azure project from your board URL (e.g., "E-Commerce", "InternalTools")
AZURE_PROJECT="your-project-name"

# Assignee filter for ticket triage (must match Azure DevOps display name exactly)
AZURE_ASSIGNEE="your-name"

# Personal Access Token (PAT) from step 1
AZURE_PAT="your-pat-token-here"
```

### 3. Test Connection

From your terminal (not from Claude Code):

```bash
cd Aria-Agent
./run.sh "Show me all open tickets"
```

This should now fetch your **actual** Azure DevOps tickets!
Primary board URL:
`https://dev.azure.com/{your-org}/{your-project}/_workitems/recentlyupdated/`

## What Gets Fetched

Aria will fetch tickets that match:

- ✅ Assigned to: **Your configured assignee name** (must match Azure DevOps display name exactly)
- ✅ Status: **To Do** OR **In Progress** OR **Reopen**
- ✅ Project: Your configured project

## Ticket States Tracked

| State       | Fetched? | Description                    |
| ----------- | -------- | ------------------------------ |
| To Do       | ✅ Yes   | New tickets not started        |
| In Progress | ✅ Yes   | Active tickets being worked on |
| Reopen      | ✅ Yes   | Tickets that were reopened     |
| New         | ❌ No    | Not included in query          |
| Closed      | ❌ No    | Completed tickets              |
| Removed     | ❌ No    | Cancelled tickets              |
| Resolved    | ❌ No    | Fixed but not closed           |

## Troubleshooting

### No tickets returned?

1. **Check assignee name** - Make sure tickets are assigned to your configured name (exact match with Azure DevOps display name)
2. **Check ticket states** - Ensure tickets are in "To Do", "In Progress", or "Reopen" status
3. **Check project** - Verify AZURE_PROJECT matches your actual project name
4. **Check permissions** - Ensure PAT token has Work Items Read permission

### Authentication error?

```bash
# Test your credentials manually
curl -u :$AZURE_PAT \
  "https://dev.azure.com/$AZURE_ORG/_apis/projects?api-version=7.1"
```

### Wrong organization?

```bash
# List your organizations
curl -u :$AZURE_PAT \
  "https://dev.azure.com/_apis/projects?api-version=7.1"
```

## Azure DevOps API Endpoints Used

| Purpose            | Method | Endpoint                             |
| ------------------ | ------ | ------------------------------------ |
| Fetch tickets      | POST   | `/_apis/wit/wiql`                    |
| Get ticket details | GET    | `/_apis/wit/workitems`               |
| Post comment       | POST   | `/_apis/wit/workitems/{id}/comments` |
| Update state       | PATCH  | `/_apis/wit/workitems/{id}`          |

## Security Notes

- ✅ PAT token is stored in `.env` (not committed to git)
- ✅ `.env` is in `.gitignore` pattern
- ✅ Only required permissions: Work Items (Read & Write)
- ✅ Token rotation recommended every 90 days

---

**Need Help?** Check `README.md` for full documentation or `QUICKSTART.md` for quick setup.
