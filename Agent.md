# AGENT.md — TechReel AI

This file gives any AI agent (Antigravity, Claude Code, etc.) working in this repo the
context it needs to act consistently with the project's architecture and philosophy.
Read this before writing code, building n8n workflows, or editing docs.

## What This Project Is

**TechReel AI** takes a tech **topic** and produces a **ready-to-publish Instagram Reel**
through an automated n8n pipeline with human review checkpoints.

It serves three purposes, in priority order:
1. **AI Workflow Engineering learning project** (primary goal)
2. **Portfolio project** — the repo should teach others how the system works
3. **Reusable technical content generation pipeline** (secondary goal)

> "The topic will change. The pipeline should not."

Because this is a learning project, an agent should never silently do the "expert"
version of a task. See **Learning Philosophy** below — it changes how you should behave,
not just what you should build.

## Orchestrator: n8n

n8n (self-hosted) is the workflow orchestrator (see `ADR-001-n8n-as-orchestrator.md`).
Key facts an agent must respect:

- Data flows between nodes as **items**: `[{ "json": {...}, "binary": {...} }]`.
- Expressions: `{{ $json.field }}`, `{{ $json.research.summary }}`, `{{ $('Node Name').item.json.field }}`.
- Every workflow starts with a **Trigger** node (Manual, Webhook, Cron, Error Trigger).
- Pipeline stages are **separate sub-workflows** called via **Execute Workflow** nodes.
- Never hardcode API keys — use n8n **Credentials**.
- Full concept reference: `n8n-fundamentals.md`.

## The 7 Architecture Rules (non-negotiable)

From `architecture-principles.md`:

1. **Everything is modular** — each pipeline stage is its own sub-workflow.
2. **Everything is replaceable** — swap a provider without touching other modules.
3. **No provider lock-in** — prefer HTTP Request + adapter pattern over provider-specific nodes (exception: Phases 0–2, learning).
4. **Human approval required** — nothing publishes without a human checkpoint.
5. **Prefer simple workflows first** — don't reach for complexity you don't yet understand.
6. **Build the smallest working version first** — Phase 1 is one API call, not the pipeline.
7. **Every module has explicit input/output contracts** — define the JSON shape before building.

## Data Model

Every module reads/writes a single **standard reel object** (see `data-model-strategy.md`).
Rules:
- The object only grows — a module adds its own section, never edits another module's section.
- Each module section has its own `status`: `not_started → in_progress → completed → pending_review → approved → failed`.
- Top-level `status` tracks overall progress: `draft → researched → scripted → storyboarded → voiced → assembled → reviewed → published`.
- Sections: `research`, `script`, `storyboard`, `voiceover`, `video`, `review`.
- **Storage**: Google Sheets for Phases 1–3 (learning), migrate to Supabase (Postgres) at Phase 5+.

When generating or editing n8n JSON, code, or prompts, always conform new fields to this
shape rather than inventing a parallel structure.

## Pipeline Stages (see `pipeline-definition.md`)

1. Topic Input → 2. Research → 3. Script Writing → 4. Human Review (script) →
5. Storyboard → 6. Voiceover → 7. Video Assembly → 8. Human Review (video) → 9. Publish (future)

Human review checkpoints (`human-review-checkpoints.md`):
- **Checkpoint 1**: Research → Learning Notes → Human Review
- **Checkpoint 2**: Script → Human Approval
- **Checkpoint 3**: Video → Human Approval → Publish

## Provider Strategy (see `provider-abstraction.md`, `tech-stack.md`)

Never couple a stage to one vendor. Current picks:

| Stage | Primary | Fallback/Upgrade |
|---|---|---|
| Research | Tavily | Serper |
| Script (LLM) | Google Gemini (free) | OpenAI GPT-4o-mini |
| Voiceover | Google Cloud TTS | ElevenLabs |
| Video Assembly | Creatomate | Shotstack |
| Stock media | Pexels | — |
| Storage | Google Sheets (now) | Supabase (Phase 5+) |

Phases 1–3: provider-specific n8n nodes are fine. Phases 4–5: wrap providers in
sub-workflows behind a standard contract. Phase 6+: add fallback provider + circuit breaker.

## Reliability Rules (see `reliability-strategy.md`)

Apply in this order as the project matures:
1. Retry on transient failures (max 3 tries, backoff) — never retry 401/403/400.
2. Fallback path via IF node when a provider fails.
3. Validate every AI/API response (non-empty, valid JSON, required fields present) before passing downstream.
4. Notify a human (Error Trigger workflow → Slack/Email/Sheets log) when automated recovery fails.
5. Circuit breaker pattern — Phase 8+ only.

## Observability (see `observability-strategy.md`)

- Phase 0.5: rely on n8n's built-in Execution History.
- Phase 3+: add a Google Sheets execution log (run_id, workflow, status, duration, timestamp, notes).
- Phase 5+: dedicated Error Trigger workflow.
- Phase 8: metrics dashboard.
- Tag workflows by stage (`research`, `script`, `voice`, `video`, `orchestrator`) and by status (`production`, `testing`, `deprecated`).
- Naming convention: `[stage]-[function]-v[version]`, e.g. `research-tavily-v1`.

## Prompts (see `prompt-engineering-guide.md`, `researcher.md`)

- Prompts are **versioned assets**, stored under `prompts/`, never hardcoded inside a workflow node.
- Every prompt must define: objective, inputs, outputs, examples.
- Template shape: Role → Context → Task → Output Format → Rules → Examples.
- LLM outputs should be **JSON-only**, validated in a Code node before being trusted downstream.

## Quality Control (see `quality-control-framework.md`)

- Research: multiple sources, source tracking, fact validation (min. 3 sources, no empty summary).
- Script: accurate, beginner-friendly, single topic, hook+body+CTA, 100–200 words, 30–60s duration.
- Video: narration match, visual accuracy, caption accuracy.

## Learning Philosophy — How the Agent Should Behave

From `learning-philosophy.md` and `implementation-rules.md`, this governs the agent's
*tone and process*, not just outputs:

- Cycle: **Learn → Build → Break → Debug → Understand → Improve.**
- Never say "just use this" and move on. Always explain **why it exists, alternatives,
  tradeoffs, and real-world usage** — the user is learning n8n and AI workflow engineering,
  not outsourcing it.
- Never hide implementation details behind abstractions the user hasn't learned yet.
- **UI Bridging:** Whenever you generate raw JSON for a new n8n node, always include a brief explanation of how to build/configure that node visually in the n8n UI so the user can learn the visual equivalent.
- Always build the smallest working version before optimization, scaling, multi-agent
  systems, or complex automation.
- Never skip testing.
- Every phase must produce: Architecture Notes, Lessons Learned, Problems Faced,
  Solutions Found (`documentation-rules.md`). Documentation is a deliverable, not an afterthought.

## Roadmap & Current State

Full roadmap: `development-roadmap.md`. Phase-to-stage mapping: `pipeline-definition.md`.

**Current phase (per `current-session.md`): Phase 0 → 0.5 transition.**
- Phase 0 (Architecture) is complete — all context docs exist.
- Phase 0.5 (n8n Fundamentals) is in progress:
  - [ ] Install n8n locally
  - [ ] Practice workflow 1: Manual Trigger → Set → Output
  - [ ] Practice workflow 2: Manual Trigger → HTTP Request → IF → Output
  - [ ] Practice workflow 3: Cron → HTTP Request → Google Sheets

**Do not jump ahead to Phase 1+ work (real Research sub-workflow, storage schema, etc.)
unless the user explicitly asks for it** — the roadmap is sequential by design, and skipping
phases undermines the learning goal.

## Quick Reference: File Map

| File | Purpose |
|---|---|
| `project-vision.md` | Why this project exists |
| `architecture-principles.md` | The 7 non-negotiable rules |
| `data-model-strategy.md` | Reel object schema |
| `pipeline-definition.md` | Full 9-stage pipeline + n8n node mapping |
| `workflow-design-rules.md` | How to structure any individual n8n sub-workflow |
| `provider-abstraction.md` | Provider options + swap strategy per stage |
| `reliability-strategy.md` | Retry/fallback/validate/notify/circuit-breaker |
| `observability-strategy.md` | Logging & monitoring per phase |
| `prompt-engineering-guide.md` / `researcher.md` | Prompt format + example (Research prompt) |
| `quality-control-framework.md` | Per-stage quality bar |
| `human-review-checkpoints.md` | Where humans must approve |
| `development-roadmap.md` | Phase-by-phase deliverables and "done when" criteria |
| `n8n-fundamentals.md` | n8n concepts reference |
| `tech-stack.md` | Concrete tool/API choices + budget |
| `ADR-001-n8n-as-orchestrator.md` | Why n8n was chosen over Make.com/LangFlow/custom Python |
| `learning-philosophy.md` / `implementation-rules.md` | How the agent should teach, not just do |
| `documentation-rules.md` | What every phase must document |
| `portfolio-goals.md` | Skills this repo must demonstrate |
| `current-session.md` | Live status — check this first every session |

## Agent Checklist Before Any Change

1. Which phase are we in? (check `current-session.md`)
2. Does this change respect the reel object contract? (`data-model-strategy.md`)
3. Is this the smallest working version, or am I over-building for the current phase?
4. Am I locking into a provider I shouldn't be? (`provider-abstraction.md`)
5. Have I explained the *why*, not just delivered the *what*?
6. Does this need a human review checkpoint before proceeding?
7. Did I update `current-session.md` / propose a documentation update?