# Workflow Design Rules

## Core Pattern: Input → Processing → Output

Every n8n sub-workflow follows this structure:

```
Trigger Node (receives input)
  ↓
Validation (check input is correct)
  ↓
Processing (do the actual work — API calls, transformations)
  ↓
Output Formatting (shape data to match output contract)
  ↓
Return / Output Node
```

## Rule 1: Every Module Must Be Testable

How to test in n8n:
- **Pin data**: Right-click any node → "Pin Data" → paste test input
- **Execute node**: Click a single node to test just that step
- **Test workflow**: Use Manual Trigger for testing, switch to Webhook/Cron for production

Good practice:
- Keep 2-3 pinned test cases per sub-workflow
- Include one normal case, one edge case, one failure case

## Rule 2: Every Module Must Be Replaceable

n8n implementation:
- Each pipeline stage is a **separate sub-workflow**
- The orchestrator calls sub-workflows using the **Execute Workflow** node
- To replace a module: create new sub-workflow, update the Execute Workflow node to point to it
- Old sub-workflow stays intact (rollback is just changing the pointer)

```
Orchestrator Workflow:
  Execute Workflow (Research)    ← Change this ID to swap research module
    ↓
  Execute Workflow (Script)     ← Change this ID to swap script module
    ↓
  Execute Workflow (Voiceover)  ← Change this ID to swap voice module
```

## Rule 3: Every Module Must Be Independently Debuggable

n8n features that help:
- **Execution History**: See every past run, its input/output at each node
- **Sticky Notes**: Add documentation notes directly on the workflow canvas
- **Node descriptions**: Every node should have a description explaining what it does
- **Console.log equivalent**: Use a Set node or Code node to output debug info

Best practice:
- Name your nodes descriptively: "Validate Research Input" not "IF1"
- Add Sticky Notes explaining complex logic
- Use workflow tags to categorize: "research", "script", "voice", "video"

## Avoid: Giant Workflows

Bad: One workflow with 50 nodes doing everything
Good: 6-8 focused sub-workflows, each with 5-10 nodes

Why: Giant workflows are impossible to debug, test, or modify.

n8n limit to know: Workflow canvas gets slow beyond ~50 nodes.

## Avoid: Hidden Logic

Bad: Complex JavaScript in a Code node with no comments
Good: Clear node names + Sticky Notes + comments in Code nodes

Every piece of logic should be explainable by reading the workflow canvas — someone looking at your workflow should understand the flow without clicking into every node.

## Avoid: Provider-Specific Design

Bad: Using the "OpenAI" node directly (locks you into OpenAI)
Good: Using "HTTP Request" node with OpenAI's API URL (can be changed to any provider)

Exception: For learning phases (0-2), using provider-specific nodes is fine. Abstract later.

## Workflow Naming Convention

```
[Stage]-[Function]-v[Version]

Examples:
  research-tavily-v1
  script-openai-v1
  voice-elevenlabs-v1
  orchestrator-main-v1
  error-handler-v1
```

## Sticky Note Standards

Every sub-workflow should have these Sticky Notes on the canvas:

1. **Purpose**: What this workflow does (top-left corner)
2. **Input Contract**: What JSON shape this workflow expects
3. **Output Contract**: What JSON shape this workflow returns
4. **Version**: Current version and what changed