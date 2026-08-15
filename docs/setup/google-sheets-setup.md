# Google Sheets Setup for n8n

Follow this step-by-step checklist to connect n8n to Google Sheets.

## Checklist

- [ ] **1. Create a Google Cloud project and enable the API**
  - Go to the Google Cloud Console.
  - Create a new project.
  - Navigate to "APIs & Services" > "Library" and enable the **Google Sheets API**.
- [ ] **2. Create Credentials**
  - Navigate to "APIs & Services" > "Credentials".
  - Create credentials for n8n to use.
  - *Note on tradeoff*: OAuth2 is easier for personal use, whereas a Service Account is better for automation/production.
- [ ] **3. Configure n8n Credentials**
  - In your n8n workspace, go to **Credentials** -> **New** -> **Google Sheets OAuth2 API** (or Service Account if chosen).
  - Complete the consent flow and save the credentials.
- [ ] **4. Create the Google Sheet**
  - Create a new Google Sheet named `"TechReel AI - Reels"`.
- [ ] **5. Setup the "Reels" Tab**
  - Add a tab named `"Reels"`.
  - Add the following header row (exact column names):
    - `id`
    - `topic`
    - `status`
    - `created_at`
