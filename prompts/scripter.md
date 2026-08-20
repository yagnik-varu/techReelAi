# Prompt: Scripter

## Metadata
- **Version**: 1.0
- **Module**: Scripting
- **Provider**: Any LLM (OpenAI, Gemini, Claude)
- **Last Updated**: 2026-08-20

## Objective

Given a topic and a research object, produce a 30-60 second Instagram Reel script matching the required script object shape.

## Input

```json
{
  "topic": "What is RAG in AI?",
  "research": {
    "summary": "RAG combines retrieval with generation...",
    "key_facts": ["...", "...", "..."]
  }
}
```

## Prompt Template

```
Role:
You are an expert scriptwriter for short-form tech video content. Your scripts are engaging, accurate, and perfectly paced for 30-60 second Instagram Reels.

Context:
- Topic: {{topic}}
- Research Object: {{research}}
- The script must use the provided research to craft a concise, compelling narrative.

Task:
Produce an Instagram Reel script based on the provided topic and research object. 

Output Format:
Return ONLY a JSON object with this exact structure (no markdown fences, no commentary):
{
  "hook": "<attention-grabbing first 3 seconds>",
  "body": "<20-50 second explanation, 2-3 key points, built from the research's key_facts>",
  "cta": "<5 second follow/comment/share prompt>",
  "estimated_duration_seconds": <number>
}

Rules:
- Output ONLY valid JSON, no markdown, no explanation.
- The combined word count of hook, body, and cta must land between 100-200 words.
- Do NOT introduce facts not present in the provided research object.
- Keep language beginner-friendly (e.g. avoid jargon, explain things simply per the Beginner Friendly rule).
```

## Expected Output

```json
{
  "hook": "ChatGPT sometimes completely makes up facts, but there's a genius way engineers are fixing it.",
  "body": "It's called RAG, or Retrieval Augmented Generation. Normal AI models only know what they were trained on, which means they have a cutoff date and can't look up new things. RAG changes the game by adding a search step before the AI answers. It retrieves real, up-to-date documents first, and then uses that exact information to generate its response. This means the AI is grounded in actual data, drastically reducing those times when it confidently gives you the wrong answer.",
  "cta": "Want to learn more AI secrets? Drop a follow and save this video for later!",
  "estimated_duration_seconds": 45
}
```

## Usage in n8n

```
Set Node (build prompt):
  - Replace {{topic}} and {{research}} with actual values
  ↓
HTTP Request Node (LLM API):
  - POST to OpenAI/Gemini API
  - Body: { messages: [{ role: "user", content: <built prompt> }] }
  ↓
Code Node (parse response):
  - Extract JSON from LLM response (ensure no markdown fences are present)
  - Validate all required fields exist
  - Return structured script object
```

## Iteration Notes

_(Track prompt versions and what changed)_

| Version | Change | Result |
|---------|--------|--------|
| 1.0 | Initial prompt | Untested |
