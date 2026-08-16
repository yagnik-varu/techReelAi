# Topics Sheet Schema

## Failure Behavior
If the Google Sheets write fails, the webhook still responds (with an HTTP 500 status and a retry message) instead of hanging or leaking a raw n8n stack trace to the caller.
