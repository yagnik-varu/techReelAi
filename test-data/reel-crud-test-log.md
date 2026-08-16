# Reel CRUD Test Log

Use this template to manually record the results of your Reel CRUD workflow testing.

## Test Pass

### 1. Create (`reel-create-v1`)
- [ ] Run the create workflow to insert a new reel into the sheet.
- **Actual Result:** 

### 2. Read (`reel-read-v1`)
- [ ] Run the read workflow using the generated `reel_id` and ensure the output matches the nested data model perfectly.
- **Actual Result:** 

### 3. Update 1 (`reel-update-v1`)
- [ ] Run the update workflow to change a specific module or field (e.g., advance the status).
- **Actual Result:** 

### 4. Update 2 (`reel-update-v1`)
- [ ] Run the update workflow again to make another modification and ensure it merges correctly without wiping other data.
- **Actual Result:** 

### 5. Read (Verify Updates)
- [ ] Run the read workflow once more to confirm all previous updates have been persisted and returned correctly.
- **Actual Result:** 

### 6. Query / Filter
- [ ] Test querying the sheet (e.g., filtering by status) to ensure duplicated top-level columns are functioning correctly for UI and n8n lookups.
- **Actual Result:** 
