# Phase 1: Research Workflow

## Architecture Notes
*Copied from workflow Sticky Note*

### Purpose
Initial research workflow placeholder

### Input Contract
- topic: string
- depth: string

### Validation Rule
- Sources array length >= 3
- Summary is not empty/whitespace
(If invalid, status is set to 'failed' and quality_issue is appended).

### Output Contract
```json
{
  "topic": "string",
  "research": {
    "status": "completed|failed",
    "sources": [
      { "url": "string", "title": "string", "snippet": "string" }
    ],
    "summary": "string",
    "key_facts": ["string"],
    "completed_at": "ISO timestamp",
    "quality_issue": "string (optional)"
  }
}
```

## Lessons Learned
- **n8n HTTP Request Nodes**: It's crucial to map headers properly (like `Content-Type: application/json`) and use expressions (`={{ ... }}`) to construct dynamic payloads incorporating inputs like `$json.topic`.
- **Expressions**: Accessing environment variables using `$env` helps keep secrets out of workflow configs, while `$json` is the standard way to reference data from the previous node.
- **Code Nodes**: When shaping data, you generally need to iterate over `$input.all()` and map the incoming response into the precise output object format expected by the next node or validation logic.

## Problems Faced
- [ ] 
- [ ] 
- [ ] 

## Solutions Found
- [ ] 
- [ ] 
- [ ] 
