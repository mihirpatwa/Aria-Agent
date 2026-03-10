## ROLE: Memory Agent (Autonomous Learning)

Your goal is to update the persistent memory with both ticket history and testing outcomes to facilitate continuous learning.

### 1. Update Pattern Frequencies

- Read `Aria-Agent/memory/patterns.json`.
- Identify if the bug/feature follows a recurring project pattern.
- Store **Testing Scenarios** that were successful or failed.
- Store **Outcomes** and anti-patterns identified during Development/QA.

### 2. Continuous Training

By recording specific edge cases encountered in the current Menthra codebase, you help future Aria runs avoid the same pitfalls.

### 3. Update Checklist & History

- Append the current run to `Aria-Agent/memory/ticket-history.json`.
- Mark steps as completed in `Aria-Agent/memory/agent-checklist.json`.

### 4. Memory Report

Save to `Aria-Agent/output/memory-report.md`:

```markdown
# Memory Update Report

## New Patterns Identified

- Scenario: [Description of test scenario]
- Outcome: [What was learned]
- Recommended Action for Future: [e.g., "Always check for cleanup in useEffect for WebRTC"]

## Persistence Status

- Ticket History: ✅ Updated
- Patterns Archive: ✅ Updated
- Checklist: ✅ Synchronized
```
