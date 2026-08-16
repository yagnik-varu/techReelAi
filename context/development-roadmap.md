# Development Roadmap

## How to Read This

Each phase has:
- **Goal**: What you're learning
- **Deliverables**: What you'll produce
- **Done When**: How you know you're finished
- **Key Concepts**: n8n/technical concepts you'll learn

---

## Phase 0 — Architecture ✅
**Goal**: Define the system before building it.

**Deliverables**:
- All context documents (architecture, workflow rules, data model, etc.)
- Project structure defined
- Learning philosophy established

**Done When**: You can explain your system design to someone without showing them code.

---

## Phase 0.5 — n8n Fundamentals (Current)
**Goal**: Learn how n8n works before building anything real.

**Deliverables**:
- n8n installed and running locally (Docker or npm)
- `n8n-fundamentals.md` completed with your own notes
- 3 practice workflows built and tested:
  1. Manual Trigger → Set Node → Output (learn data flow)
  2. Manual Trigger → HTTP Request → IF Node → Output (learn API calls + conditions)
  3. Cron Trigger → HTTP Request → Google Sheets (learn scheduling + storage)

**Key Concepts**:
- Nodes (triggers, actions, logic)
- Expressions (`{{ $json.fieldName }}`)
- Credentials management
- Execution history and debugging
- Error handling basics

**Done When**: You can build a workflow from scratch without looking at tutorials, and you can explain what `{{ $json.items[0].title }}` means.

---

## Phase 1 — First Real Workflow ✅
**Goal**: Build the simplest version of ONE pipeline stage.

**Deliverables**:
- Research sub-workflow: Manual Trigger → Search API → Format Results → Output
- Input: topic string
- Output: structured research object matching data model

**Key Concepts**:
- HTTP Request node configuration
- JSON parsing and transformation
- Set node for data shaping
- Error handling with IF node

**Done When**: You type a topic, click execute, and get back a clean JSON object with sources and summary.

---

## Phase 2 — Topic Input System ✅
**Goal**: Accept topics from outside n8n.

**Deliverables**:
- Webhook trigger that accepts POST requests with a topic
- Google Sheets integration to log incoming topics
- Simple validation (reject empty topics)

**Key Concepts**:
- Webhook nodes and URLs
- Request validation
- Google Sheets node (read/write)
- Webhook response node

**Done When**: You can send a POST request from Postman/curl and see the topic appear in Google Sheets.

---

## Phase 2.5 — Workflow Testing
**Goal**: Learn to test and debug workflows systematically.

**Deliverables**:
- Test data file with 5 sample topics (easy, medium, hard, edge cases)
- Error workflow that catches failures and logs them
- Documentation of 3 bugs you found and fixed

**Key Concepts**:
- n8n execution log analysis
- Error Trigger node
- Pin data for testing
- Workflow tags and organization

**Done When**: You have a repeatable testing process and can debug a broken workflow in under 10 minutes.

---

## Phase 3 — Data Model Implementation ✅
**Goal**: Implement the full reel object and storage.

**Deliverables**:
- Google Sheets structured as reel database
- Create, Read, Update operations via n8n
- Status tracking across pipeline stages

**Key Concepts**:
- Google Sheets as database
- CRUD operations in n8n
- Data transformation with Code node (JavaScript)
- Merge node for combining data

**Done When**: You can create a reel record, update it through stages, and query reels by status.

---

## Phase 3.5 — Data Engineering
**Goal**: Handle real-world data problems.

**Deliverables**:
- Input sanitization workflow
- Data validation at each pipeline stage
- Graceful handling of missing/malformed data

**Key Concepts**:
- Code node for custom validation
- IF/Switch nodes for branching
- Function node vs Code node
- SplitInBatches for bulk processing

**Done When**: Your workflow handles bad input gracefully instead of crashing.

---

## Phase 4 — API Integration Fundamentals
**Goal**: Connect to AI APIs (LLM for script writing).

**Deliverables**:
- Script generation sub-workflow
- Prompt template system (prompts stored as variables, not hardcoded)
- Input: research object → Output: script object

**Key Concepts**:
- OpenAI/Claude API via HTTP Request
- API key management with credentials
- Prompt template construction
- Response parsing (extracting content from API responses)
- Token usage tracking

**Done When**: Given research output, the workflow generates a structured script matching your data model.

---

## Phase 4.5 — Prompt Engineering
**Goal**: Optimize prompts for consistent, high-quality output.

**Deliverables**:
- Versioned prompt templates in `prompts/` folder
- A/B test results for at least 2 prompt variations
- Prompt that consistently produces scripts with hook + body + CTA structure

**Key Concepts**:
- System vs user prompts
- Few-shot examples in prompts
- Output format enforcement (JSON mode)
- Temperature and parameter tuning

**Done When**: Your prompt produces usable scripts 8 out of 10 times without manual editing.

---

## Phase 5 — Voice Generation
**Goal**: Convert scripts to voiceover audio.

**Deliverables**:
- Voice generation sub-workflow
- Input: script text → Output: audio file URL
- Voice selection and configuration

**Key Concepts**:
- TTS API integration (ElevenLabs or OpenAI TTS)
- Binary data handling in n8n
- File storage (local or cloud)
- Audio duration calculation

**Done When**: You can generate a voiceover from a script and play it back.

---

## Phase 6 — Video Assembly
**Goal**: Combine visuals + audio into a reel.

**Deliverables**:
- Video assembly sub-workflow
- Input: storyboard + audio → Output: video file
- Caption/subtitle generation

**Key Concepts**:
- Video API integration (Creatomate, Shotstack, or FFmpeg)
- Template-based video generation
- Binary file handling
- Async job polling (video rendering takes time)

**Done When**: You get a downloadable MP4 that looks like an Instagram reel.

---

## Phase 7 — Orchestration
**Goal**: Connect all sub-workflows into the full pipeline.

**Deliverables**:
- Main orchestrator workflow
- End-to-end: Topic → Research → Script → Review → Voice → Video → Review
- Human review pause points

**Key Concepts**:
- Execute Workflow node chaining
- Wait node for human review
- Workflow error handling at orchestrator level
- Execution timeout management

**Done When**: One click produces a complete reel (with human review pauses).

---

## Phase 8 — Polish & Deploy
**Goal**: Make it production-ready.

**Deliverables**:
- Deployed n8n instance (cloud or self-hosted)
- Monitoring and alerting
- Documentation for others to understand the system

**Key Concepts**:
- n8n deployment options
- Environment variables
- Backup and restore
- Performance optimization

**Done When**: The system runs reliably without you watching it.