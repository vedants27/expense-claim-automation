# Workflow reference

## Platforms and responsibilities

| Platform | Responsibility |
|---|---|
| n8n | Form hosting, routing, expressions and execution monitoring |
| Google Sheets | Existing-ID lookup, claim persistence and audit storage |
| Gmail | Acknowledgement and two business-rule rejection messages |

## Nodes

| Node | Behaviour |
|---|---|
| On form submission | Collects the form fields and starts the workflow. |
| Validate Amount | Routes Amount > 0 to duplicate checking; otherwise routes to invalid-amount rejection. |
| Check Duplicate Claim | Looks up Claim ID in Claims; Always Output Data is enabled. |
| Duplicate Found? | Routes a nonempty lookup Claim ID to rejection; an empty result to saving. |
| Save New Claim | Appends the original form data, Submitted status, timestamp and execution ID. |
| Send Claim Acknowledgement | Sends receipt confirmation; approval and reimbursement remain pending review. |
| Reject Invalid Amount | Explains that the amount must be greater than zero. |
| Reject Duplicate Claim | Explains that the Claim ID already exists; does not append a claim. |
| Log Successful Claim | Appends Submitted outcome after the acknowledgement. |
| Log Invalid Amount | Appends Rejected outcome and amount-validation reason. |
| Log Duplicate Claim | Appends Rejected outcome and duplicate-ID reason. |

## Branch order

1. Invalid amount: form → Validate Amount (false) → Reject Invalid Amount → Log Invalid Amount.
2. Duplicate: form → Validate Amount (true) → Check Duplicate Claim → Duplicate Found? (true) → Reject Duplicate Claim → Log Duplicate Claim.
3. Valid new claim: form → Validate Amount (true) → Check Duplicate Claim → Duplicate Found? (false) → Save New Claim → Send Claim Acknowledgement → Log Successful Claim.

Amount validation runs first. A duplicate ID submitted with a negative amount therefore follows the invalid-amount branch.

## Important expressions

```javascript
// Amount validation: compare this value with 0 using Number >
{{ Number($json.Amount) }}

// Lookup filter, Claim ID column
{{ $json["Claim ID"] }}

// Duplicate condition: String is not empty
{{ $json["Claim ID"] ?? "" }}

// Access original form values after nodes have changed the output
{{ $('On form submission').first().json["Claim ID"] }}
{{ $('On form submission').first().json.Email }}
{{ Number($('On form submission').first().json.Amount) }}

// Claim submission timestamp
{{ $('On form submission').first().json.submittedAt }}

// Audit timestamp and execution correlation
{{ $now.toISO() }}
{{ $execution.id }}
```

## Audit values

| Branch | Outcome | Reason |
|---|---|---|
| Valid new claim | Submitted | Claim recorded and acknowledgement sent |
| Invalid amount | Rejected | Amount must be greater than zero |
| Duplicate | Rejected | Duplicate Claim ID |

All three audit nodes use Append Row, preserving the separate history of repeated attempts. See the diagram in Diagrams/Workflow_Architecture.png.
