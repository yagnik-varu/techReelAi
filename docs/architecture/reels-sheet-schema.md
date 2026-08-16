# Reels Sheet Schema

This document outlines the schema and design patterns used for the "TechReel - Reels" Google Sheet, which acts as the primary database for tracking reels through the pipeline.

## Columns

| Column Name  | Description |
| ------------ | ----------- |
| `id`         | A unique identifier for the reel (e.g., a generated UUID or timestamp-based ID). |
| `topic`      | The topic of the reel. |
| `status`     | The current stage of the reel in the pipeline (e.g., `draft`, `researching`, `scripting`, `review`, `published`). |
| `created_at` | The timestamp when the reel record was first created. |
| `updated_at` | The timestamp when the reel record was last modified. |
| `data`       | A JSON string containing the complete, nested reel object (research data, script, audio URL, etc.). |

## The JSON String Pattern

We use a specific pattern where the `data` column stores the full nested reel object as a stringified JSON payload.

### Why this approach?
Google Sheets natively provides flat, two-dimensional storage (rows and columns). However, our data model for a reel is complex and deeply nested (containing arrays of sources, structured script segments, metadata, etc.). 

If we tried to map every nested field to a dedicated column, the sheet would become impossibly wide and brittle whenever our data model changes. By stringifying the entire nested object into a single `data` column, we can seamlessly store complex, hierarchical data structures within a flat Google Sheet. n8n workflows can parse this JSON when reading, and stringify it when writing.

### Why duplicate some fields?
You will notice that fields like `id`, `topic`, `status`, `created_at`, and `updated_at` are duplicated—they exist as top-level flat columns *and* they exist within the parsed JSON object in the `data` column.

This duplication is intentional. It allows us to leverage the Google Sheets UI effectively:
- **Filtering and Sorting**: We can easily sort the sheet by `updated_at` or filter rows by a specific `status` (e.g., show me only `draft` reels) directly in the Google Sheets interface or via simple n8n queries, without having to parse the JSON for every row first.
- **Quick Glancing**: Humans can quickly look at the sheet and understand the state of the system without needing to read the raw JSON payloads.
