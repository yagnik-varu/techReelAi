# Architecture Principles

## Rule 1: Everything is Modular

Every pipeline stage (research, script, storyboard, voice, video) is a **separate n8n sub-workflow**.

Why: If your script-writing breaks, you don't lose the research. Each piece runs and fails independently.

n8n Implementation:
- Use the **Execute Workflow** node to call sub-workflows
- Each sub-workflow has its own trigger, logic, and output
- Main workflow = orchestrator that calls sub-workflows in sequence

## Rule 2: Everything is Replaceable

Any module can be swapped without changing the rest of the pipeline.

Example: Replace OpenAI with Claude for script writing — only the script sub-workflow changes. The research module before it and the storyboard module after it don't know or care.

n8n Implementation:
- Sub-workflows communicate through a **standard data contract** (JSON shape)
- The orchestrator doesn't know what's inside each sub-workflow

## Rule 3: No Provider Lock-in

Never write workflows that only work with one specific API.

Bad: Research node that only works with Tavily
Good: Research sub-workflow with an adapter pattern — swap Tavily for Serper by changing one node

n8n Implementation:
- Use **HTTP Request** nodes with configurable URLs instead of provider-specific nodes when possible
- Store API endpoints and keys in **n8n credentials** (not hardcoded)

## Rule 4: Human Approval Required

No content is published without human review. AI generates, human approves.

n8n Implementation:
- Use **Wait** node to pause workflow until human responds
- Use **Webhook** node to receive human approval
- Alternative: Send output to a review channel (Slack/Discord/Email), human clicks approve link

## Rule 5: Prefer Simple Workflows First

Start with the simplest version that works. Add complexity only when you understand why.

Example progression:
1. Manual Trigger → HTTP Request → Output (learn basic flow)
2. Add error handling (learn IF nodes)
3. Add sub-workflows (learn modularity)
4. Add webhooks (learn triggers)

## Rule 6: Build Smallest Working Version First

Phase 1 is NOT the full pipeline. Phase 1 is:
- Manual Trigger → Call one API → See the output

That's it. Everything else comes after you understand that.

## Rule 7: Every Module Has Input and Output Contracts

Before building a module, define:
- What JSON shape it receives
- What JSON shape it returns
- What happens if input is invalid

This is critical in n8n because data flows between nodes as `items[]` — each item has a `.json` property containing your data.

```
Input Contract Example (Research Module):
{
  "topic": "What is RAG in AI?",
  "depth": "beginner"
}

Output Contract Example:
{
  "topic": "What is RAG in AI?",
  "sources": [...],
  "summary": "...",
  "key_facts": [...]
}
```