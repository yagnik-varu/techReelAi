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

## Testing the Research Workflow

If you don't see the "Pin Data" option, here are two ways to inject this test data into n8n:

### Method 1: Using an "Edit Fields (Set)" Node (Recommended)
1. Add a new **Edit Fields (Set)** node right after your trigger.
2. Click **Add Value** -> **String** for both fields.
3. Set the first field name to `topic` and value to `What is RAG in AI?`
4. Set the second field name to `depth` and value to `beginner`
5. Connect this to the rest of your workflow and click **Test step** or **Execute Workflow**.

### Method 2: Pinning Data (If supported by your n8n version)
1. In the n8n canvas, **right-click** the node you want to inject data into (e.g., the first action node).
2. Click **Pin Data** from the context menu.
3. A popup will appear. Delete what is there and paste the test case as a JSON array. For example:
   ```json
   [
     {
       "topic": "What is RAG in AI?",
       "depth": "beginner"
     }
   ]
   ```
4. Close the popup and run the workflow.

## Webhooks

When working with workflows that contain Webhook triggers, importing the workflow is not enough to make the webhook listen for requests.

You **must** do one of the following before sending a request:
1. **Test Mode**: Click **"Execute Workflow"** (or "Listen for Test Event") in the n8n UI. While testing, you must use the `webhook-test/` prefix in your URL (e.g., `http://localhost:5678/webhook-test/topic-input`).
2. **Production Mode**: Toggle the workflow to **"Active"** in the top right corner of the n8n UI. In production, use the standard `webhook/` prefix in your URL (e.g., `http://localhost:5678/webhook/topic-input`).

## Testing reel-read-v1

When testing the `reel-read-v1` workflow, keep in mind:
- **Set Reel ID Input**: The `reel_id` provided in the "Set Reel ID Input" node must be copied from an actual `id` value that exists in your "TechReel - Reels" Google Sheet. You can create a new record using the `reel-create-v1` workflow first.
- **Output Validation**: The final output of the workflow should exactly match the nested shape documented in [docs/architecture/data-model-strategy.md](docs/architecture/data-model-strategy.md).
