# Tech Stack

## Overview

This document tracks every technology decision — what we're using, what we considered, and why.

## Core Platform

| Component | Choice | Why | Status |
|-----------|--------|-----|--------|
| Workflow Orchestrator | **n8n** | Visual, open-source, self-hostable, strong community | Decided ✅ |
| n8n Hosting | TBD | Options: local Docker, Railway, Render, n8n Cloud | Pending |

See [ADR-001](file:///d:/yagnik-deploy/TechReelAi/decisions/ADR-001-n8n-as-orchestrator.md) for the full decision rationale.

## APIs by Pipeline Stage

### Research
| Option | Status | Notes |
|--------|--------|-------|
| Tavily | **Primary** (planned) | Best free tier for AI search (1000 req/mo) |
| Serper | Fallback | 2500 free req/mo, Google results |

### Script Writing (LLM)
| Option | Status | Notes |
|--------|--------|-------|
| Google Gemini | **Primary** (planned) | Good free tier for learning |
| OpenAI GPT-4o-mini | Upgrade path | Best quality/cost balance |

### Voiceover (TTS)
| Option | Status | Notes |
|--------|--------|-------|
| Google Cloud TTS | **Primary** (planned) | Most generous free tier |
| ElevenLabs | Upgrade path | Best quality |

### Video Assembly
| Option | Status | Notes |
|--------|--------|-------|
| Creatomate | **Primary** (planned) | Easiest API for reel format |
| Shotstack | Alternative | More flexible, steeper learning |

### Stock Media
| Option | Status | Notes |
|--------|--------|-------|
| Pexels | **Primary** (planned) | Free, good API, video + photos |

### Storage
| Option | Status | Notes |
|--------|--------|-------|
| Google Sheets | **Phase 1-3** | Learning phase, visual, free |
| Supabase | **Phase 5+** | Production, PostgreSQL |

## Development Tools

| Tool | Purpose | Status |
|------|---------|--------|
| VS Code / Cursor | Code editing, documentation | Active ✅ |
| Docker | Running n8n locally | To set up |
| Postman / curl | API testing | To set up |
| Git | Version control | To set up |

## Budget Estimate (Monthly)

### Learning Phase (Phase 0-3): ~$0
- n8n: Self-hosted (free)
- Tavily: Free tier
- Gemini: Free tier
- Google Sheets: Free

### Building Phase (Phase 4-6): ~$10-20/mo
- OpenAI: ~$5 (GPT-4o-mini is very cheap)
- ElevenLabs: Free tier or ~$5
- Creatomate: Free tier (5 renders/mo) or ~$12
- Pexels: Free

### Production Phase (Phase 7-8): ~$30-50/mo
- n8n hosting: ~$10-20 (Railway/Render)
- APIs: ~$20-30 combined
- Supabase: Free tier

## Decisions Still Needed

- [ ] n8n hosting approach (local Docker vs cloud)
- [ ] Instagram Business account setup
- [ ] Final voice provider choice
- [ ] Video style/template choice
- [ ] Content niche specifics
