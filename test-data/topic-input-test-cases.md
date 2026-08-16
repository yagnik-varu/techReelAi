# Topic Input Test Cases

## Testing Topic Input

n8n webhooks have two endpoints:
- **Test URL (`/webhook-test/...`)**: Use this when you are actively building or testing a workflow within the n8n UI (e.g., clicking "Execute Workflow" or "Listen for Test Event").
- **Production URL (`/webhook/...`)**: Use this for live integration. **Important:** The production URL will only work if the workflow toggle is set to **Active** in the n8n UI.

---

## Valid Topic Case (Test URL)
Use this command to send a valid topic to the webhook (using the test URL):

```bash
curl -X POST http://localhost:5678/webhook-test/topic-input \
  -H "Content-Type: application/json" \
  -d '{"topic": "What is RAG in AI?"}'
```

## Valid Topic Case (Production URL)
Use this command to send a valid topic to the active webhook (using the production URL). *Ensure the workflow is Active!*

```bash
curl -X POST http://localhost:5678/webhook/topic-input \
  -H "Content-Type: application/json" \
  -d '{"topic": "What is RAG in AI?"}'
```

**Verification:**
After running either curl command successfully, check the "TechReel AI - Reels" Google Sheet! You should see a new row added matching the topic sent, along with an auto-generated ID, a "draft" status, and the current timestamp.
