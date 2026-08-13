# TechReel AI

TechReel AI is an n8n-based automated content pipeline.

## Getting Started

1. Create a `.env` file based on `.env.example` and populate it with required values.
2. Start the local n8n instance:
   ```bash
   docker compose up -d
   ```
3. Open [http://localhost:5678](http://localhost:5678) in your web browser.

## How to import a workflow

1. Go to the n8n UI in your browser.
2. Navigate to **Workflows**.
3. Select **Import from File**.
4. Choose the specific `.json` file from the `workflows/` directory.
