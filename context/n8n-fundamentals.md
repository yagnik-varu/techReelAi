# n8n Fundamentals

## What is n8n?

n8n (pronounced "nodemation") is a **workflow automation tool**. Think of it as a visual programming environment where you connect boxes (nodes) together to create automated pipelines.

For TechReel AI, n8n is the **orchestrator** — it connects all the APIs (search, LLM, voice, video) into one automated pipeline.

## Core Concepts

### 1. Nodes

Nodes are the building blocks. Each node does one thing.

**Types of nodes:**

| Type | Purpose | Examples |
|------|---------|----------|
| **Trigger** | Starts a workflow | Manual Trigger, Webhook, Cron, Error Trigger |
| **Action** | Does something | HTTP Request, Google Sheets, Send Email |
| **Logic** | Controls flow | IF, Switch, Merge, SplitInBatches |
| **Transform** | Changes data | Set, Code, Function, Date & Time |
| **Flow** | Manages execution | Wait, Execute Workflow, Stop and Error |

Every workflow **must** start with a Trigger node.

### 2. Data Structure (Critical to Understand)

n8n passes data between nodes as an **array of items**. Each item has a `json` property and optionally a `binary` property.

```json
// What data looks like inside n8n:
[
  {
    "json": {
      "name": "RAG",
      "description": "Retrieval Augmented Generation"
    },
    "binary": {}
  },
  {
    "json": {
      "name": "Fine-tuning",
      "description": "Training a model on custom data"
    },
    "binary": {}
  }
]
```

Key rules:
- Every node receives items from the previous node
- Every node outputs items to the next node
- If a node receives 3 items, it processes all 3 (unless configured otherwise)
- You access data using **expressions**: `{{ $json.name }}` → `"RAG"`

### 3. Expressions

Expressions let you reference data dynamically. They use double curly braces.

```
{{ $json.fieldName }}          → Access current item's field
{{ $json.research.summary }}   → Access nested field
{{ $json.sources[0].url }}     → Access array element
{{ $('Node Name').item.json.field }}  → Access data from a specific node
{{ $now.toISO() }}             → Current timestamp
{{ $execution.id }}            → Current execution ID
{{ $workflow.name }}           → Current workflow name
```

### 4. Credentials

Credentials store API keys, passwords, and auth tokens securely.

- Created in: Settings → Credentials
- Referenced by nodes (not hardcoded in workflows)
- Encrypted at rest
- Can be shared across workflows

**Never hardcode API keys in node configurations** — always use credentials.

### 5. Execution Modes

| Mode | When | How |
|------|------|-----|
| **Manual** | You click "Execute Workflow" | Runs once, you see results immediately |
| **Production** | Triggered automatically | Webhook receives request, Cron fires, etc. |
| **Test** | You click "Execute Node" | Runs just that one node |

For learning: Always use Manual mode first. Switch to Production when you're confident.

### 6. Error Handling

Three levels:

**Level 1: Node-level**
- Each node → Settings → "On Error"
- Options: Stop (default), Continue, Continue (using error output)
- "Continue (using error output)" is most useful — routes errors to a separate path

**Level 2: Retry**
- Each node → Settings → "Retry On Fail"
- Set max retries and wait time
- Good for temporary API failures

**Level 3: Error Workflow**
- Workflow Settings → "Error Workflow"
- Points to a separate workflow that triggers on failure
- Use Error Trigger node in the error workflow

### 7. Sub-Workflows

A workflow can call another workflow using the **Execute Workflow** node.

```
Main Workflow:
  Manual Trigger
    ↓
  Execute Workflow (ID: research-workflow)
    ↓
  Execute Workflow (ID: script-workflow)
    ↓
  Output
```

Why: Modularity. Each sub-workflow can be tested, debugged, and replaced independently.

Data passing:
- Parent sends data to child via the Execute Workflow node
- Child receives data in its Trigger node
- Child's output returns to the parent

## Essential Nodes You'll Use

### Manual Trigger
- Starts workflow when you click "Execute"
- No configuration needed
- Use for: testing, development

### HTTP Request
- Makes API calls (GET, POST, PUT, DELETE)
- Configure: URL, method, headers, body, authentication
- Use for: calling any REST API (OpenAI, Tavily, ElevenLabs, etc.)

### Set
- Creates or modifies data fields
- Use for: reshaping data between modules, setting default values

### IF
- Branches workflow based on a condition
- Two outputs: true and false
- Use for: validation, error checking, conditional logic

### Code
- Runs custom JavaScript
- Full access to item data
- Use for: complex transformations, validation, data manipulation
- Has access to `items` array and can return modified items

### Google Sheets
- Read, write, append, update rows in Google Sheets
- Use for: data storage, logging, topic management

### Wait
- Pauses workflow execution
- Can wait for: time duration, webhook, or date/time
- Use for: human review pauses, rate limiting

## Your First 3 Practice Workflows

### Practice 1: Hello n8n
```
Manual Trigger → Set Node (add field: message = "Hello n8n!") → No-op (view output)
```
**Learn**: How data flows, how Set node works, how to view output

### Practice 2: API Call
```
Manual Trigger → HTTP Request (GET https://api.quotable.io/random) → IF (quote length > 50?) → Set Node (format output)
```
**Learn**: HTTP requests, IF conditions, data transformation

### Practice 3: Scheduled Storage
```
Cron Trigger (every hour) → HTTP Request (GET news API) → Google Sheets (append row)
```
**Learn**: Scheduled triggers, writing to external storage

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| Tab | Open node search |
| Ctrl+Enter | Execute workflow |
| Ctrl+S | Save workflow |
| Ctrl+Z | Undo |
| D | Disable/enable node |
| F2 | Rename node |

## Resources

- n8n Documentation: https://docs.n8n.io
- n8n Community: https://community.n8n.io
- n8n Templates: https://n8n.io/workflows (browse existing workflows for inspiration)
