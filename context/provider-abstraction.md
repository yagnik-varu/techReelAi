# Provider Abstraction

## Core Rule

Never directly couple a pipeline stage to a specific provider. Always design so the provider can be swapped.

## Why This Matters

- APIs change pricing → you need to switch
- APIs go down → you need a fallback
- Better tools appear → you want to upgrade easily
- Free tiers run out → you move to the next free option

## The Adapter Pattern

```
Bad (tight coupling):
  Research Module → Tavily API (hardcoded)

Good (abstraction):
  Research Module → Research Interface → Tavily Adapter
                                       → Serper Adapter
                                       → Perplexity Adapter
```

In n8n, this means: Each sub-workflow defines a **standard input/output contract**. Inside the sub-workflow, you can use whatever provider you want. The orchestrator doesn't care.

## Provider Options by Pipeline Stage

### Research (Topic → Facts + Sources)

| Provider | Tier | API Type | Notes |
|----------|------|----------|-------|
| Tavily | Free tier: 1000 req/mo | REST API | Built for AI search, structured results |
| Serper | Free tier: 2500 req/mo | REST API | Google search results as JSON |
| SerpAPI | Free tier: 100 req/mo | REST API | Multiple search engines |
| Perplexity | Paid only | REST API | AI-powered search with citations |
| Brave Search | Free tier: 2000 req/mo | REST API | Privacy-focused, good API |

**Recommendation for learning**: Start with Tavily (best free tier for AI projects)

### Script Writing (Research → Script)

| Provider | Tier | API Type | Notes |
|----------|------|----------|-------|
| OpenAI (GPT-4o-mini) | Pay per use (~$0.15/1M tokens) | REST API | Best balance of quality/cost |
| OpenAI (GPT-4o) | Pay per use (~$2.50/1M tokens) | REST API | Higher quality, higher cost |
| Google Gemini | Free tier available | REST API | Good free option |
| Anthropic Claude | Pay per use | REST API | Excellent for structured output |
| Groq (Llama) | Free tier available | REST API | Very fast, good free tier |

**Recommendation for learning**: Start with Gemini (free) or GPT-4o-mini (cheapest paid)

### Voiceover (Script → Audio)

| Provider | Tier | API Type | Notes |
|----------|------|----------|-------|
| ElevenLabs | Free tier: 10,000 chars/mo | REST API | Best quality, most natural |
| OpenAI TTS | Pay per use (~$15/1M chars) | REST API | Good quality, simple API |
| Google Cloud TTS | Free tier: 1M chars/mo | REST API | Good quality, generous free tier |
| Azure TTS | Free tier: 0.5M chars/mo | REST API | Good quality |

**Recommendation for learning**: Start with Google Cloud TTS (most generous free tier)

### Video Assembly (Storyboard + Audio → Video)

| Provider | Tier | API Type | Notes |
|----------|------|----------|-------|
| Creatomate | Free tier: 5 renders/mo | REST API | Template-based, good for reels |
| Shotstack | Free tier available | REST API | Powerful, JSON-based video editing |
| Remotion | Open source | JavaScript | Programmatic video, React-based |
| FFmpeg | Free | Command line | Most flexible, steepest learning curve |
| JSON2Video | Free trial | REST API | Simple JSON to video |

**Recommendation for learning**: Start with Creatomate (easiest API for reel format)

### Stock Media (Visuals for B-roll)

| Provider | Tier | Notes |
|----------|------|-------|
| Pexels | Free | Free stock video/photos, good API |
| Pixabay | Free | Free stock media |
| Unsplash | Free | Free photos (no video) |

### Storage (Reel Data Persistence)

| Provider | Tier | Notes |
|----------|------|-------|
| Google Sheets | Free | Great for learning, built-in n8n node |
| Airtable | Free tier: 1000 records | Better structure than Sheets |
| Supabase | Free tier: 500MB | PostgreSQL, best for production |
| Notion | Free tier | Good for human review workflow |

## Implementation Strategy

### Phase 1-3 (Learning): Use provider-specific n8n nodes
It's okay to use the built-in "OpenAI" node or "Google Sheets" node directly. Focus on learning n8n, not architecture perfection.

### Phase 4-5 (Building): Abstract into sub-workflows
Wrap each provider call in its own sub-workflow. The orchestrator calls "research" sub-workflow, not "tavily" directly.

### Phase 6+ (Production): Add fallback providers
Each sub-workflow has a primary provider and a fallback. IF primary fails → try fallback → notify human.