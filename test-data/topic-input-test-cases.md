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

---

## Error Cases (Test URL)

These cases test the specific error types handled by the `sanitize-topic-v1` workflow.

### 1. Topic Too Long (`too_long`)
Use this command to send a topic that exceeds the 200 character limit:

```bash
curl -X POST http://localhost:5678/webhook-test/topic-input \
  -H "Content-Type: application/json" \
  -d '{"topic": "This is a very long topic that is intended to exceed the two hundred character limit set by the sanitize-topic-v1 sub-workflow in order to trigger the too_long error type validation failure and verify that it correctly rejects the input and returns the appropriate error message to the client."}'
```
**Expected Error Message:** The response should return a validation failure with an `error_type` of `too_long` and an `error_message` indicating the topic exceeds the 200 character limit.

### 2. Disallowed Content (`disallowed_content`)
Use this command to send a topic containing potentially malicious content:

```bash
curl -X POST http://localhost:5678/webhook-test/topic-input \
  -H "Content-Type: application/json" \
  -d '{"topic": "<script>alert(1)</script>"}'
```
**Expected Error Message:** The response should return a validation failure with an `error_type` of `disallowed_content` and an `error_message` indicating the topic contains disallowed content.

### 3. Invalid Type (`invalid_type`)
Use this command to send a topic as a number instead of a string:

```bash
curl -X POST http://localhost:5678/webhook-test/topic-input \
  -H "Content-Type: application/json" \
  -d '{"topic": 12345}'
```
**Expected Error Message:** The response should return a validation failure with an `error_type` of `invalid_type` and an `error_message` indicating the topic must be a string.
