# Prompt: Researcher

## Metadata
- **Version**: 1.0
- **Module**: Research
- **Provider**: Any LLM (OpenAI, Gemini, Claude)
- **Last Updated**: 2026-08-10

## Objective

Given a tech topic, produce structured research output suitable for writing a 30-60 second Instagram Reel script.

## Input

```json
{
  "topic": "What is RAG in AI?",
  "depth": "beginner",
  "target_audience": "tech-curious non-engineers"
}
```

## Prompt Template

```
Role:
You are a tech researcher preparing material for a short-form video script writer. Your research will be used to create a 30-60 second Instagram Reel about a technical topic.

Context:
- Topic: {{topic}}
- Target Audience: {{target_audience}}
- Depth Level: {{depth}}
- The final reel must be accurate, engaging, and understandable by the target audience.

Task:
Research the given topic and produce a structured summary. Focus on:
1. A simple, accurate explanation a beginner can understand
2. One surprising or counterintuitive fact (for the hook)
3. 2-3 key points that can be explained in under 50 seconds
4. A real-world analogy that makes the concept relatable

Output Format:
Return a JSON object with this exact structure:
{
  "topic": "<topic name>",
  "one_line_explanation": "<explain it in one simple sentence>",
  "hook_fact": "<one surprising fact or question to grab attention>",
  "key_points": [
    "<point 1>",
    "<point 2>",
    "<point 3>"
  ],
  "analogy": "<real-world analogy>",
  "common_misconception": "<one thing people get wrong about this>",
  "technical_accuracy_notes": "<any important nuances to not oversimplify>"
}

Rules:
- Output ONLY valid JSON, no markdown, no explanation
- All text must be understandable by a non-engineer
- "hook_fact" must be genuinely surprising or thought-provoking
- "key_points" should be ordered from simplest to most important
- "analogy" must use an everyday object or experience
- Do NOT use jargon without explaining it
- Keep each field under 2 sentences
```

## Expected Output

```json
{
  "topic": "RAG (Retrieval Augmented Generation)",
  "one_line_explanation": "RAG is a technique that lets AI search for real information before answering, instead of guessing from memory.",
  "hook_fact": "ChatGPT sometimes makes up facts — RAG is how engineers are fixing that.",
  "key_points": [
    "AI models like ChatGPT only know what they were trained on, which has a cutoff date",
    "RAG adds a 'search step' before the AI answers — it retrieves real documents first",
    "This means AI responses are grounded in actual data, dramatically reducing hallucinations"
  ],
  "analogy": "It's like an open-book exam vs a closed-book exam — RAG lets the AI look things up instead of relying purely on memory.",
  "common_misconception": "People think RAG makes AI smarter — it actually just gives AI access to better information.",
  "technical_accuracy_notes": "RAG has two phases: retrieval (finding relevant documents) and generation (using those documents to form a response). The quality of retrieval directly affects output quality."
}
```

## Usage in n8n

```
Set Node (build prompt):
  - Replace {{topic}}, {{target_audience}}, {{depth}} with actual values
  ↓
HTTP Request Node (LLM API):
  - POST to OpenAI/Gemini API
  - Body: { messages: [{ role: "user", content: <built prompt> }] }
  ↓
Code Node (parse response):
  - Extract JSON from LLM response
  - Validate all required fields exist
  - Return structured research object
```

## Iteration Notes

_(Track prompt versions and what changed)_

| Version | Change | Result |
|---------|--------|--------|
| 1.0 | Initial prompt | Untested |
