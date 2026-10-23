# Expense Claim Automation
An automated expense claim processing system built with n8n, Google Sheets, and Gmail. Validates claim amounts, detects duplicate claims, records valid submissions, sends email notifications, and maintains audit logs.

## Problem
Manual expense claim processing involves repetitive data entry,
invalid amounts, duplicate submissions, and delayed acknowledgements.

## Features
- Collects expense claims through an online form.
- Rejects amounts less than or equal to zero.
- Detects previously recorded Claim IDs.
- Records valid claims in Google Sheets.
- Sends acknowledgement or rejection emails through Gmail.
- Maintains an audit log for each completed processing branch.

## Platforms
- n8n Cloud: form, workflow orchestration, and execution monitoring
- Google Sheets: claim records and audit logs
- Gmail: email notifications

## Workflow
Form submission → Amount validation → Duplicate check →
Save valid claim → Send acknowledgement → Record audit entry.

Invalid amounts and duplicate claims follow separate rejection
branches with email notifications and audit entries.

## Setup
1. Import Workflow/Expense_Claim_Automation.json into n8n.
2. Connect your Google Sheets and Gmail credentials.
3. Create a spreadsheet with the tabs and headers listed below.
4. Select your spreadsheet and corresponding tabs in every
   Google Sheets node.
5. Select your Gmail credential in all three email nodes.
6. Test all three scenarios, publish the workflow, and open
   the Production URL from the form trigger.

### Claims tab headers
Claim ID, Employee Name, Email, Amount, Category, Description,
Status, Submitted At, Execution ID

### Audit Log tab headers
Execution ID, Claim ID, Employee Name, Amount, Outcome,
Reason, Timestamp

## Test Scenarios
| Scenario | Expected result |
|---|---|
| New Claim ID with a positive amount | Claim saved, acknowledgement sent, audit entry added |
| Amount of zero or a negative value | Rejection email and audit entry; no claim saved |
| Previously recorded Claim ID | Duplicate rejection email and audit entry; no additional claim saved |

## Limitations
- Valid claims receive Submitted status; approval and payment require further review.
- Duplicate checking is intended for sequential submissions.
- Gmail or Google Sheets failures require investigation through n8n executions.
- Cloud availability depends on the hosting subscription.

## Future Enhancements
- Manager approval workflow
- Receipt upload and OCR verification
- Department-specific expense limits
- Automatic retries and technical failure alerts
