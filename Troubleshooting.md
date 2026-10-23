# Troubleshooting and limitations

| Symptom | Check / action |
|---|---|
| No continuation after a new-ID lookup | Enable Always Output Data in Check Duplicate Claim |
| Email comes from the wrong account | Inspect all three Gmail nodes and choose the intended connected account |
| Spreadsheet unavailable | Reconnect Google Sheets credentials and select an accessible document and tab |
| Column mapping errors | Preserve exact headers and refresh the mappings after selecting the target spreadsheet |
| Audit rows overwrite one another | Use Append Row rather than Append or Update Row |
| Production submission does not trigger | Verify the workflow is published and the browser uses Production URL rather than Test URL |
| No visible editor activity for production run | Open the workflow's Executions view; check execution-saving settings if history is absent |
| Valid claim is rejected as duplicate | Check whether its Claim ID already exists; choose an unused ID for a new claim |

## Known implementation boundaries

- This workflow records submissions. It does not approve expenses or issue reimbursement.
- Duplicate checking uses a Google Sheets lookup followed by an append. It is not a transaction and cannot guarantee uniqueness under concurrent submissions.
- The form has no configured authentication; it is suitable for the classroom demonstration rather than unrestricted sensitive-data collection.
- The two handled business exceptions are invalid amount and duplicate Claim ID. No custom technical error workflow or automatic retry policy is configured.
- Audit nodes run after the corresponding Gmail node. If sending fails, its downstream audit entry may not be created. A valid claim may already have been saved before its email fails.
- The category is free text; receipt upload, OCR, policy limits and manager approval are future enhancements.
- Execution IDs help correlate spreadsheet outcomes with n8n executions. Availability of historical executions depends on the instance settings and retention.

## Future improvements

Use a database with a unique Claim ID constraint, implement authenticated forms, introduce a manager approval stage, add receipt verification, and provide an error workflow with controlled retries and failure alerts.
