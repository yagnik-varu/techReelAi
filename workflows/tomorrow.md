In n8n, there is no separate "Catch" or "Status" node. Instead, you can use the **Edit Fields (Set)** node right after your Google Sheets node to create exactly what you are asking for: a simple output that tells you if it succeeded or failed.

Here is how to set up one simple node to give you that clean "Success" or "Fail" output:

1. **Add an Edit Fields (Set) node:**
Connect an **Edit Fields (Set)** node immediately after your Google Sheets node (make sure your Google Sheets node still has "Continue On Fail" enabled in its settings).


2. **Create a Status field:**
In the Edit Fields node, click **Add Assignment** -> **String**.

* **Name**: type `SheetStatus`
* **Value**: click the gear icon, select Add Expression, and paste this exact code: `{{ $json.error ? 'Fail' : 'Success' }}`


3. **Keep all other data (Optional):**
Toggle the **Keep Only Set Fields** switch to off (disabled) if you want to keep the error details and row data alongside your new status.


### What this does:

When you run the workflow, this single node will look at what the Google Sheet did and output a clean, simple field called `SheetStatus`.

* If the sheet writes successfully, the output will say: `"SheetStatus": "Success"`
* If the sheet fails (like your 403 error), the output will say: `"SheetStatus": "Fail"`

You can then easily look at this single node's output to analyze your logs, or use it later in your workflow without having to deal with complex error JSON!