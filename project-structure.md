# Project Structure

```
TechReel-AI/
│
├── Agent.md                          # AI assistant behavior rules
├── project-structure.md              # This file — project layout reference
│
├── context/                          # All project context documents
│   ├── project-vision.md             # What this project is and why
│   ├── architecture-principles.md    # 7 rules for system design
│   ├── development-roadmap.md        # Phase-by-phase learning plan
│   ├── learning-philosophy.md        # How we approach learning
│   ├── n8n-fundamentals.md           # n8n core concepts reference
│   ├── pipeline-definition.md        # Complete reel generation pipeline
│   ├── tech-stack.md                 # Technology choices and rationale
│   ├── workflow-design-rules.md      # Rules for designing n8n workflows
│   ├── human-review-checkpoints.md   # Where humans approve content
│   ├── provider-abstraction.md       # API provider strategy per stage
│   ├── data-model-strategy.md        # The reel data object definition
│   ├── prompt-engineering-guide.md   # Rules for writing AI prompts
│   ├── observability-strategy.md     # Logging and monitoring plan
│   ├── reliability-strategy.md       # Error handling patterns
│   ├── quality-control-framework.md  # Quality gates per stage
│   ├── implementation-rules.md       # Build small, test always
│   ├── documentation-rules.md        # What to document per phase
│   └── portfolio-goals.md            # Skills to demonstrate
│
├── decisions/                        # Architecture Decision Records
│   └── ADR-001-n8n-as-orchestrator.md
│
├── sessions/                         # Learning session tracking
│   └── current-session.md
│
├── prompts/                          # Versioned prompt templates
│   └── researcher.md
│
└── workflows/                        # n8n workflow JSON exports (Phase 1+)
    └── (empty until Phase 1)
```

## File Purposes

| Folder | Purpose | When Updated |
|--------|---------|-------------|
| `context/` | Project rules and reference docs | Rarely after Phase 0 |
| `decisions/` | Why we chose X over Y | When making big decisions |
| `sessions/` | Daily learning notes | Every session |
| `prompts/` | AI prompt templates | When optimizing prompts |
| `workflows/` | Exported n8n workflow JSONs | When workflows change |