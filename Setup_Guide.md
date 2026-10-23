# Setup and deployment

## Prerequisites

- An n8n instance supporting Form Trigger, If, Google Sheets and Gmail nodes.
- A Google account with permission to edit the target spreadsheet and send Gmail messages.
- The exported Expense Claim Automation JSON.

## 1. Prepare the spreadsheet

Create a spreadsheet named **Expense Claim Records** with the following tabs. Paste each tab-separated header line into cell A1 of the corresponding tab.

### Claims

```text
Claim ID	Employee Name	Email	Amount	Category	Description	Status	Submitted At	Execution ID
```

### Audit Log

```text
Execution ID	Claim ID	Employee Name	Amount	Outcome	Reason	Timestamp
```

Use these exact header names and keep the header row intact. Claims contains accepted submissions; Audit Log records submitted and rejected outcomes after their email node completes.

## 2. Import and reconnect

1. Create a workflow in n8n and use its import-from-file option to import the submitted JSON.
2. Connect a Google Sheets credential with access to your spreadsheet.
3. In **Check Duplicate Claim** and **Save New Claim**, select your spreadsheet and the **Claims** tab.
4. In the three **Log ...** nodes, select your spreadsheet and **Audit Log** tab.
5. Recheck the column mappings after changing spreadsheets. Save and audit operations must use **Append Row**.
6. Connect your intended sender account in Gmail. Select this same credential in all three Gmail nodes. Credential names are labels; verify the underlying connected Google account.
7. In **Check Duplicate Claim → Settings**, retain **Always Output Data**. This lets the workflow continue when no existing claim matches.

The export references the original spreadsheet and credential identifiers. Importing does not give another user access to those accounts; replace the references with your own resources.

## 3. Understand the form

| Field | Type | Required |
|---|---|---|
| Claim ID | Text | Yes |
| Employee Name | Text | Yes |
| Email | Email | Yes |
| Amount | Number, INR | Yes |
| Category | Text | Yes |
| Description | Multiline text | No |

## 4. Test

Use your own recipient address. Test a unique ID with a positive amount, a unique ID with a negative amount, and then repeat the first ID. See Test_Results.md for expected outputs. Use a new ID for every fresh acceptance test.

## 5. Publish and use the production form

1. Save the workflow and publish it.
2. Open **On form submission** and copy its **Production URL**.
3. Open that URL in a browser tab and submit a new claim. Do not manually execute the workflow for this production test.
4. Verify the email, Claims row, Audit Log entry and execution details.
5. Use the production URL shown by your own instance; an imported workflow has a different hosting context.

Keep the hosting instance available for demonstrations. If the cloud trial or subscription ends, the hosted form may become unavailable.
