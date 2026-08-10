# Reliability Strategy

## What Can Go Wrong

In an AI workflow pipeline, failures are not exceptions — they're expected. Every external API call can fail.

| Failure Type | Example | How Often |
|---|---|---|
| API Failure | OpenAI returns 500 | Occasional |
| Timeout | Search API takes 30+ seconds | Occasional |
| Empty Response | LLM returns blank content | Rare |
| Invalid JSON | LLM returns malformed output | Common |
| Rate Limit | Too many API calls per minute | Common at scale |
| Auth Failure | Expired API key or token | Occasional |
| Quota Exceeded | Monthly API credits exhausted | Monthly risk |

## Strategy 1: Retry

When: Temporary failures (500 errors, timeouts, rate limits)

n8n Implementation:
- **Node-level retry**: Every HTTP Request node has Settings → "On Error" → "Retry on Fail"
  - Set retry count: 3
  - Set wait between retries: 1000ms (increases automatically)
- **When NOT to retry**: Auth failures (401/403), validation errors (400)

```
Node Settings:
  On Error → Continue (using error output)
  Retry On Fail → true
  Max Tries → 3
  Wait Between Tries → 1000ms
```

## Strategy 2: Fallback

When: Primary service is down or consistently failing

n8n Implementation:
- Use **IF** node after API call to check for errors
- If error → route to fallback path
- Fallback can be: different API, cached response, or default value

```
HTTP Request (OpenAI)
  ↓
IF (success?)
  ├── YES → Continue pipeline
  └── NO → HTTP Request (Claude as fallback)
           ↓
           IF (success?)
             ├── YES → Continue pipeline
             └── NO → Set node (error message) → Notify Human
```

## Strategy 3: Validate Output

When: API succeeds but returns bad data

n8n Implementation:
- Use **IF** node or **Code** node after every AI response
- Check: Is the response empty? Is it valid JSON? Does it have required fields?

```javascript
// Code node: Validate LLM Output
const response = $json.choices[0].message.content;

// Check 1: Not empty
if (!response || response.trim() === '') {
  throw new Error('LLM returned empty response');
}

// Check 2: Has required sections
if (!response.includes('Hook:') || !response.includes('Body:')) {
  throw new Error('Script missing required sections');
}

return [{ json: { script: response, valid: true } }];
```

## Strategy 4: Notify Human

When: All automated recovery fails

n8n Implementation:
- **Error Trigger** workflow: Create a separate workflow that triggers on any workflow failure
- Send notification via: Email, Slack, Discord, or Webhook
- Include: What failed, when, input data, error message

```
Error Trigger Node
  ↓
Set Node (format error details)
  ↓
Send Email / Slack Message
  ↓
Google Sheets (log the error)
```

## Strategy 5: Circuit Breaker (Advanced — Phase 8+)

When: An API is consistently failing and you want to stop wasting calls

Logic:
- Track failure count in Google Sheets
- If failures > 5 in last hour → skip that provider, use fallback
- Reset counter after cooldown period

## Implementation Priority

1. **Phase 1**: Add retry to all HTTP Request nodes
2. **Phase 2**: Add output validation with IF nodes
3. **Phase 3**: Add Error Trigger workflow for notifications
4. **Phase 5+**: Add fallback paths
5. **Phase 8**: Add circuit breaker pattern