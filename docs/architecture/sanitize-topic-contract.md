# Sanitize Topic Contract (`sanitize-topic-v1`)

This document defines the input/output contracts for the `sanitize-topic-v1` sub-workflow, which acts as the data validation and sanitization layer for incoming topics.

## Input Contract

The sub-workflow expects a JSON object containing the raw input data:

```json
{
  "topic": "string",
  "depth": "string (optional)"
}
```

## Output Contract

The sub-workflow always returns a structured JSON object indicating whether the input was valid, the sanitized data (if valid), and detailed error information (if invalid):

```json
{
  "valid": "boolean",
  "topic": "string (sanitized, if valid)",
  "depth": "string (sanitized, if valid)",
  "error_type": "string (null if valid)",
  "error_message": "string (null if valid)"
}
```

## Error Types

If `valid` is `false`, the `error_type` field will be populated with one of the following values:

| `error_type` | Trigger Condition |
| :--- | :--- |
| `empty` | Triggered when the topic string is empty, null, or consists solely of whitespace. |
| `too_long` | Triggered when the topic string exceeds the maximum allowed character limit (e.g., > 200 characters). |
| `invalid_type` | Triggered when the provided topic or depth fields are not of the expected string data type. |
| `disallowed_content` | Triggered when the topic contains inappropriate, offensive, or restricted keywords/patterns. |
