# Phase 2: Topic Input - Lessons Learned

## Architecture Notes
- **Workflow Name**: `topic-input-v1`
- **Purpose**: Receives a topic via webhook and saves it to a Google Sheet for processing.
- **Input Contract**:
  - Endpoint: `POST /webhook/topic-input` (or `/webhook-test/topic-input`)
  - Request Body: JSON with `{ "topic": "string", "depth": "string (optional)" }`
- **Output Contract**:
  - `200 OK`: JSON response indicating success, e.g., `{"success": true, "message": "Topic saved successfully"}`
  - `400 Bad Request`: JSON response for missing topic or invalid input.
  - `500 Internal Server Error`: JSON response if saving to Google Sheets or processing fails.
- **Data Destination**: Writes to the **"TechReel - Topics"** Google Sheet.

## Lessons Learned
* [ ] Notes on setting up and managing Webhooks in n8n.
* [ ] Notes on configuring and using OAuth credentials for Google Sheets.
* [ ] Observations on n8n's behavior with Test URL (`/webhook-test/`) vs Production URL (`/webhook/`).

## Problems Faced
* [ ] Describe any problems or roadblocks faced during this phase.
    * Google sheet write faile even if enable from google console.

## Solutions Found
* [ ] Describe how the problems were solved.
* [ ] With google sheet permission we have to also add google drive permission for it.
