# Phase 3: Data Model & Storage - Lessons Learned

## Architecture Notes
- **Data Model & Storage Pattern**: We are using Google Sheets as our primary database. To accommodate a deeply nested, complex data model (the "Reel Object") within flat rows and columns, we serialize the entire object into a JSON string and store it in a single `data` column. Key fields like `id`, `topic`, and `status` are duplicated in their own columns to allow for easy filtering and sorting without parsing JSON.

- **Reel Storage Workflows & I/O Contracts**:
  - `reel-create-v1`:
    - **Input**: Topic (string)
    - **Output**: Full initialized Reel object with a generated `id` and `draft` status.
  - `reel-read-v1`:
    - **Input**: `reel_id` (string)
    - **Output**: Parsed Reel object from the database.
  - `reel-update-v1`:
    - **Input**: `reel_id` (string), plus partial data to update (e.g., specific modules or status changes).
    - **Output**: The fully merged and updated Reel object, successfully written back to the sheet.
  - `reel-query-v1`:
    - **Input**: Query parameters (e.g., `status = draft`).
    - **Output**: Array of parsed Reel objects matching the criteria.

## Lessons Learned
* [ ] Notes on using the Merge node and deep-merge logic in n8n.
* [ ] Observations and tradeoffs of using Google Sheets as a database.
* [ ] ...

## Problems Faced
* [ ] Describe any problems or roadblocks faced during this phase.

## Solutions Found
* [ ] Describe how the problems were solved.
