# Data Model Strategy

## Core Principle

Every module reads and writes a **standard reel object**. This object grows richer as it flows through the pipeline. Each module adds its section but never removes or modifies another module's data.

## The Reel Object

```json
{
  "id": "reel_20260810_001",
  "topic": "What is RAG in AI?",
  "status": "draft",
  "created_at": "2026-08-10T19:00:00Z",
  "updated_at": "2026-08-10T19:05:00Z",

  "research": {
    "status": "completed",
    "sources": [
      {
        "url": "https://example.com/rag-explained",
        "title": "RAG Explained",
        "snippet": "Retrieval Augmented Generation is..."
      }
    ],
    "summary": "RAG combines retrieval with generation...",
    "key_facts": [
      "RAG reduces hallucination by grounding responses in real data",
      "RAG has two phases: retrieval and generation"
    ],
    "completed_at": "2026-08-10T19:01:00Z"
  },

  "script": {
    "status": "pending_review",
    "hook": "Did you know AI can now Google things before answering?",
    "body": "That's called RAG — Retrieval Augmented Generation...",
    "cta": "Follow for more AI concepts explained simply.",
    "word_count": 150,
    "estimated_duration_seconds": 45,
    "version": 1,
    "completed_at": null
  },

  "storyboard": {
    "status": "not_started",
    "scenes": [],
    "completed_at": null
  },

  "voiceover": {
    "status": "not_started",
    "audio_url": null,
    "duration_seconds": null,
    "provider": null,
    "completed_at": null
  },

  "video": {
    "status": "not_started",
    "video_url": null,
    "thumbnail_url": null,
    "completed_at": null
  },

  "review": {
    "status": "not_started",
    "approved_by": null,
    "feedback": null,
    "approved_at": null
  }
}
```

## Status Values (Enum)

Each module's `status` field follows this progression:

```
not_started → in_progress → completed → pending_review → approved → failed
```

The top-level `status` field summarizes overall progress:

```
draft → researched → scripted → storyboarded → voiced → assembled → reviewed → published
```

## How This Maps to n8n

In n8n, data flows between nodes as **items**. Each item has a `.json` property.

```
n8n item structure:
{
  "json": {
    "id": "reel_20260810_001",
    "topic": "What is RAG?",
    "status": "draft",
    "research": { ... },
    "script": { ... }
  }
}
```

Key n8n rules:
- Access fields with expressions: `{{ $json.topic }}` or `{{ $json.research.summary }}`
- Each node receives items from the previous node
- Use **Set** node to reshape data between modules
- Use **Merge** node to combine outputs from parallel paths

## Storage Strategy (TBD — Decide in Phase 3)

Options to evaluate:

| Option | Pros | Cons | Best For |
|--------|------|------|----------|
| Google Sheets | Free, visual, easy to share | Slow with large data, no relations | Learning phase |
| Airtable | Visual, good API, relations | Row limits on free tier | Small scale |
| Supabase | PostgreSQL, real-time, free tier | More complex setup | Production |
| JSON files | Simplest, no external dependency | No querying, manual management | Prototyping |

**Decision**: Start with Google Sheets for learning (Phase 1-3). Migrate to Supabase when building the real pipeline (Phase 5+).