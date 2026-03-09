## ROLE: Azure Ticket Triage & Selection

Your goal is to fetch, categorize, and allow selection of Azure DevOps tickets.

### 1. Fetch Tickets

Use the following WIQL query to fetch tickets assigned to $AZURE_ASSIGNEE:

```bash
curl -sS -u ":$AZURE_PAT" -H 'Content-Type: application/json' \
  -X POST "https://dev.azure.com/$AZURE_ORG/$AZURE_PROJECT/_apis/wit/wiql?api-version=7.1" \
  -d '{"query": "SELECT [System.Id], [System.WorkItemType], [System.Title], [System.State] FROM WorkItems WHERE [System.AssignedTo] CONTAINS '"'"'$AZURE_ASSIGNEE'"'"' AND [System.State] IN ('"'"'To Do'"'"', '"'"'In Progress'"'"', '"'"'Reopen'"'"') AND [System.TeamProject] = '"'"'$AZURE_PROJECT'"'"' ORDER BY [Microsoft.VSTS.Common.Priority] ASC"}'
```

### 2. Categorize

For each ticket, categorize it as:

- **Task**: General engineering tasks or setup.
- **Bug**: Reported issues, crashes, or incorrect behavior.
- **Feature**: New functionality or enhancements.

Map to internal service tags if possible:

- `heygen_sync`, `elevenlabs_latency`, `deepgram_connection`, `openai_context`, `webrtc_ice`, `websocket_drop`, `api_rate_limit`, `mobile_compat`.

### 3. Present & Select

1. Generate a table of active tickets in `Aria-Agent/output/azure-report.md`.
2. Present this table to the user.
3. **WAIT** for the user to respond with "Work on ticket #[ID]" or similar selection.

### 4. Output

Write findings to `Aria-Agent/output/azure-report.md`:

```markdown
# 🎫 Azure Ticket Report

## Active Tickets

| ID  | Type    | Title               | Category           |
| --- | ------- | ------------------- | ------------------ |
| 123 | Bug     | Reconnection fails  | websocket_drop     |
| 456 | Feature | Add Spanish support | elevenlabs_latency |

**Action**: Please select a ticket to proceed.
```
