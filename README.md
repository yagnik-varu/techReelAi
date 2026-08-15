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
