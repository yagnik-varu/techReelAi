# Topic Input Test Cases

## Valid Topic Case
Use this command to send a valid topic to the webhook (using the test URL):

```bash
curl -X POST http://localhost:5678/webhook-test/topic-input \
  -H "Content-Type: application/json" \
  -d '{"topic": "What is RAG in AI?"}'
```

**Verification:**
After running this curl command, check the "TechReel AI - Reels" Google Sheet! You should see a new row added matching the topic sent, along with an auto-generated ID, a "draft" status, and the current timestamp.
