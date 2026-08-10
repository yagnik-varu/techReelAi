# ADR-001: n8n as Workflow Orchestrator

## Status
Accepted

## Date
2026-08-10

## Context

TechReel AI needs a workflow orchestrator to connect multiple AI APIs (search, LLM, TTS, video) into an automated content pipeline. The orchestrator must:

- Connect 5+ APIs in sequence
- Handle errors and retries
- Pause for human review
- Be modular (swap individual components)
- Support learning (visual, debuggable)

## Options Considered

### Option 1: n8n (Selected ✅)
- **What**: Open-source workflow automation with visual editor
- **Pros**:
  - Visual — see the entire pipeline as a flowchart
  - Self-hostable — no vendor lock-in, full control
  - 400+ built-in integrations
  - Strong community and documentation
  - Sub-workflows for modularity
  - Error handling built-in
  - Free (self-hosted)
  - JavaScript-based (familiar)
- **Cons**:
  - Learning curve for expressions and data model
  - UI can get slow with very large workflows
  - Self-hosting requires maintenance

### Option 2: Make.com (Rejected)
- **What**: Cloud-based visual automation platform
- **Pros**: Very visual, easy to start, many integrations
- **Cons**:
  - Paid beyond free tier (1000 operations/mo)
  - Closed source — can't self-host
  - Vendor lock-in
  - Limited customization for complex logic
  - Less portfolio value (proprietary tool)

### Option 3: LangFlow / Flowise (Rejected)
- **What**: Visual LLM orchestration tools
- **Pros**: Built specifically for AI workflows, LangChain integration
- **Cons**:
  - Focused on LLM chains, not full pipelines
  - Less mature for non-LLM tasks (video, storage)
  - Smaller community
  - Rapidly changing APIs

### Option 4: Custom Python (Rejected)
- **What**: Build the entire pipeline in Python code
- **Pros**: Maximum flexibility, full control
- **Cons**:
  - No visual debugging
  - Much more code to write and maintain
  - Harder to show in portfolio (code vs visual flows)
  - No built-in UI for monitoring
  - Have to build retry/error handling from scratch

## Decision

**n8n** because:
1. Visual approach makes learning faster and debugging easier
2. Open-source + self-hosted = no cost ceiling, no vendor lock-in
3. Sub-workflows directly support our modular architecture
4. Portfolio value: visual workflows are more impressive in demos
5. Community resources for learning

## Consequences

- Must learn n8n's data model (items, json, binary)
- Must learn n8n's expression syntax
- Must set up Docker or npm for local development
- Workflow files (.json) can be version-controlled in Git
- Complex logic may require Code nodes (JavaScript)
