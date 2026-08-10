# Pipeline Definition

## What TechReel AI Does

Takes a **topic** and produces a **ready-to-publish Instagram Reel** through an automated pipeline with human review checkpoints.

## The Complete Pipeline

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│  TOPIC   │───▶│ RESEARCH │───▶│  SCRIPT  │───▶│  REVIEW  │
│  INPUT   │    │          │    │ WRITING  │    │  (Human) │
└──────────┘    └──────────┘    └──────────┘    └────┬─────┘
                                                     │
                                              ┌──────┴──────┐
                                              │  Approved?  │
                                              └──────┬──────┘
                                        No ◄──┤      ├──► Yes
                                        │             │
                                   ┌────▼────┐  ┌─────▼──────┐
                                   │ REVISE  │  │ STORYBOARD │
                                   │ SCRIPT  │  │ GENERATION │
                                   └─────────┘  └─────┬──────┘
                                                      │
                                                ┌─────▼──────┐    ┌──────────┐    ┌──────────┐
                                                │ VOICEOVER  │───▶│  VIDEO   │───▶│  REVIEW  │
                                                │ GENERATION │    │ ASSEMBLY │    │  (Human) │
                                                └────────────┘    └──────────┘    └────┬─────┘
                                                                                      │
                                                                               ┌──────┴──────┐
                                                                               │  Approved?  │
                                                                               └──────┬──────┘
                                                                         No ◄──┤      ├──► Yes
                                                                         │             │
                                                                    ┌────▼────┐  ┌─────▼────┐
                                                                    │ REVISE  │  │ PUBLISH  │
                                                                    └─────────┘  └──────────┘
```

## Stage Details

### Stage 1: Topic Input

**What happens**: A topic enters the system.

| | Detail |
|---|---|
| **Input** | Topic string (e.g., "What is RAG in AI?") |
| **Processing** | Validate topic, check for duplicates, create reel record |
| **Output** | Reel object with status "draft" |
| **Entry methods** | Webhook POST, manual input, Google Sheets row, scheduled queue |
| **n8n nodes** | Webhook / Manual Trigger → IF (validate) → Google Sheets (create record) |

---

### Stage 2: Research

**What happens**: Gather facts, sources, and key information about the topic.

| | Detail |
|---|---|
| **Input** | `{ topic: "What is RAG?", depth: "beginner" }` |
| **Processing** | Search web, extract key facts, compile sources |
| **Output** | `{ summary, key_facts[], sources[], completed_at }` |
| **APIs** | Tavily / Serper / Brave Search |
| **Quality check** | Minimum 3 sources, no empty summary |
| **n8n nodes** | HTTP Request (search API) → Code (parse results) → Set (format output) |

---

### Stage 3: Script Writing

**What happens**: AI generates a reel script from research.

| | Detail |
|---|---|
| **Input** | Research object (summary + key facts) |
| **Processing** | Send research + prompt template to LLM, parse response |
| **Output** | `{ hook, body, cta, word_count, estimated_duration }` |
| **APIs** | OpenAI / Gemini / Claude |
| **Quality check** | Has hook + body + CTA, word count 100-200, duration 30-60s |
| **n8n nodes** | Set (build prompt) → HTTP Request (LLM API) → Code (parse + validate) |

Script structure for a 30-60 second reel:
```
Hook (first 3 seconds): Attention-grabbing question or statement
Body (20-50 seconds): Explanation with 2-3 key points
CTA (last 5 seconds): Follow, comment, or share prompt
```

---

### Stage 4: Human Review (Script)

**What happens**: Human reads the script and approves, rejects, or requests changes.

| | Detail |
|---|---|
| **Input** | Script object |
| **Processing** | Send to review channel, wait for response |
| **Output** | `{ approved: true/false, feedback: "..." }` |
| **Channels** | Email, Slack, Discord, or web dashboard |
| **n8n nodes** | Send Email / Slack → Wait (for webhook) → IF (approved?) |

---

### Stage 5: Storyboard Generation

**What happens**: AI creates scene-by-scene visual descriptions for the reel.

| | Detail |
|---|---|
| **Input** | Approved script |
| **Processing** | Break script into scenes, describe visuals for each |
| **Output** | `{ scenes: [{ text, visual_description, duration, media_query }] }` |
| **APIs** | OpenAI / Gemini (same LLM as script, different prompt) |
| **n8n nodes** | Set (build storyboard prompt) → HTTP Request → Code (parse scenes) |

---

### Stage 6: Voiceover Generation

**What happens**: Convert script text to spoken audio.

| | Detail |
|---|---|
| **Input** | Script text |
| **Processing** | Send to TTS API, receive audio file |
| **Output** | `{ audio_url, duration_seconds, provider }` |
| **APIs** | ElevenLabs / OpenAI TTS / Google Cloud TTS |
| **n8n nodes** | HTTP Request (TTS API) → Move Binary Data → Write Binary File |

---

### Stage 7: Video Assembly

**What happens**: Combine storyboard visuals + voiceover into final video.

| | Detail |
|---|---|
| **Input** | Storyboard + audio URL |
| **Processing** | Find/generate visuals, overlay captions, merge with audio |
| **Output** | `{ video_url, thumbnail_url, duration }` |
| **APIs** | Creatomate / Shotstack + Pexels (stock video) |
| **Note** | Video rendering is async — submit job, poll for completion |
| **n8n nodes** | HTTP Request (submit render) → Wait (30-60s) → HTTP Request (check status) → Loop until done |

---

### Stage 8: Human Review (Video)

**What happens**: Human watches the final reel and approves for publishing.

| | Detail |
|---|---|
| **Input** | Video URL + script + metadata |
| **Processing** | Send to review channel with video link |
| **Output** | `{ approved: true/false, feedback: "..." }` |
| **n8n nodes** | Same pattern as Stage 4 |

---

### Stage 9: Publish (Future)

**What happens**: Upload approved reel to Instagram.

| | Detail |
|---|---|
| **Input** | Approved video file + caption |
| **Processing** | Upload via Instagram Graph API |
| **Output** | `{ post_id, url, published_at }` |
| **Note** | Requires Instagram Business account + Facebook App setup |
| **n8n nodes** | HTTP Request (Instagram API) → Google Sheets (update status) |

## What You're Building Phase by Phase

| Phase | Stages Built |
|-------|-------------|
| 0-0.5 | None (learning) |
| 1 | Stage 2 (Research only) |
| 2 | Stage 1 (Topic Input) |
| 3 | Storage layer for all stages |
| 4 | Stage 3 (Script Writing) |
| 5 | Stage 6 (Voiceover) |
| 6 | Stage 5 + 7 (Storyboard + Video) |
| 7 | All stages connected (Orchestrator) |
| 8 | Stage 9 (Publish) + Monitoring |
