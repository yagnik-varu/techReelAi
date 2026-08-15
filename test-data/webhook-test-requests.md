# Webhook Test Requests

*Note: This is a reference for Phase 2. The webhook is not yet functional.*

## Testing the Webhook

**Important n8n Concept: Test vs. Production URLs**
n8n has two different URLs for every webhook. The error `The requested webhook "..." is not registered` happens when you call a URL that isn't actively listening.

### 1. Test URL (For Building & Debugging)
Use this when you are actively building the workflow. It will only work if you click **"Listen for Test Event"** or **"Execute Workflow"** in the n8n UI first.

```bash
curl -X POST http://localhost:5678/webhook-test/topic-input \
  -H "Content-Type: application/json" \
  -d '{"topic": "What is RAG in AI?"}'
```
*(Notice the `/webhook-test/` in the URL)*

### 2. Production URL (For Live Use)
Use this when your workflow is finished. It will **only** work if you toggle the workflow to **"Active"** (top right corner of the n8n editor). If the workflow is inactive, you will get a 404 error.

```bash
curl -X POST http://localhost:5678/webhook/topic-input \
  -H "Content-Type: application/json" \
  -d '{"topic": "What is RAG in AI?"}'
```
*(Notice the `/webhook/` in the URL)*
