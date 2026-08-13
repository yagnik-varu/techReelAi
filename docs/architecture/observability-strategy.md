# Observability Strategy

## Why Observability Matters

When your pipeline fails at 2 AM (or mid-demo), you need to answer:
- **What** failed?
- **When** did it fail?
- **Why** did it fail?
- **What data** was it processing?

Without observability, debugging means re-running everything and hoping to catch the error.

## What Every Workflow Run Must Log

| Field | Example | Why |
|-------|---------|-----|
| Run ID | `exec_abc123` | Unique identifier for this execution |
| Workflow Name | `research-tavily-v1` | Which workflow ran |
| Start Time | `2026-08-10T19:00:00Z` | When it started |
| End Time | `2026-08-10T19:00:05Z` | When it finished |
| Duration | `5000ms` | How long it took |
| Status | `success` / `failed` / `warning` | Did it work? |
| Input Summary | `topic: "What is RAG?"` | What data went in |
| Output Summary | `sources: 5, word_count: 150` | What came out |
| Error | `API returned 429` | What went wrong (if anything) |
| Module | `research` | Which pipeline stage |

## n8n Built-in Observability

### Execution History
- n8n automatically logs every execution
- Access: Left sidebar → Executions
- Shows: start time, duration, status, and full node-by-node data
- **Retention**: Default keeps last 250 executions (configurable)

### Execution Data
- Click any past execution to see input/output at every node
- You can "retry" a failed execution from the last successful node

### Workflow Tags
- Tag workflows by stage: `research`, `script`, `voice`, `video`, `orchestrator`
- Tag by status: `production`, `testing`, `deprecated`

## Custom Logging (Add in Phase 3+)

### Method 1: Google Sheets Log

Add a logging step at the end of every sub-workflow:

```
Processing Nodes
  ↓
Google Sheets Node (Append Row)
  → Sheet: "Execution Log"
  → Data: run_id, workflow, status, duration, timestamp, notes
```

### Method 2: Error Workflow

Create a dedicated error-handler workflow:

```
Error Trigger Node
  ↓
Set Node (format error details):
  - execution_id: {{ $execution.id }}
  - workflow_name: {{ $workflow.name }}
  - error_message: {{ $json.error.message }}
  - timestamp: {{ $now.toISO() }}
  ↓
Google Sheets (log error)
  ↓
Email/Slack Notification
```

### Method 3: Webhook Notifications (Phase 5+)

For real-time monitoring:
- On workflow completion → POST to monitoring webhook
- Dashboard collects and displays execution stats
- Can use services like Grafana, or a simple custom dashboard

## Key Metrics to Track Over Time

| Metric | What It Tells You |
|--------|------------------|
| Success rate per module | Which modules are flaky? |
| Average execution time | Is anything getting slower? |
| API cost per reel | How much does each reel cost to generate? |
| Error frequency by type | What's the most common failure? |
| Retry count | Are retries actually helping? |

## Implementation Priority

1. **Phase 0.5**: Rely on n8n's built-in execution history (free, automatic)
2. **Phase 3**: Add Google Sheets logging
3. **Phase 5**: Add Error Trigger workflow
4. **Phase 8**: Add metrics dashboard