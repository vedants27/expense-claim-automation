# Screenshots and Test Evidence

The screenshots below show email outputs from the Expense Claim Automation workflow. Place the four PNG files in `Documentation/Screenshots/` with their original filenames so these image links work.

## 1. Duplicate Claim Rejection — EXP001 (Earlier Credential Test)

![Duplicate Claim Rejection — EXP001](Screenshots/Screenshot%202026-10-23%20225415.png)

- **Screenshot:** `Screenshot 2026-10-23 225415.png`
- **Observed output:** The email identifies EXP001 as an existing Claim ID and explains that no additional claim was created.
- **Test stage:** This screenshot shows the earlier test using Varun's Gmail account. The sender credential was subsequently corrected to Vedant's account and retested. This image documents the earlier duplicate rejection, not the final sender configuration.
- **Final evidence:** Replace this image with a duplicate rejection received from Vedant's account when available, and update the filename and caption accordingly.

## 2. Production Claim Acknowledgement — EXP007

![Production Claim Acknowledgement — EXP007](Screenshots/Screenshot%202026-10-23%20225429.png)

- **Screenshot:** `Screenshot 2026-10-23 225429.png`
- **Observed output:** Acknowledgement for EXP007, amount INR 850, category Travel, status Submitted.
- **Sender displayed:** Vedant.Shitole Btech2023.
- **Verification:** The operator confirmed that this claim was submitted through the published production form and that the spreadsheet updated. The screenshot directly shows the resulting email.
- **Business meaning:** Receipt is confirmed; approval and reimbursement remain pending review.

## 3. Invalid Amount Rejection — EXP006

![Invalid Amount Rejection — EXP006](Screenshots/Screenshot%202026-10-23%20225456.png)

- **Screenshot:** `Screenshot 2026-10-23 225456.png`
- **Observed output:** Rejection for EXP006 with entered amount INR -100.
- **Reason:** The expense amount must be greater than zero.
- **Sender displayed:** Vedant.Shitole Btech2023.
- **Verification:** Demonstrates the email output for the invalid-amount exception branch.

## 4. Successful Claim Acknowledgement — EXP005

![Successful Claim Acknowledgement — EXP005](Screenshots/Screenshot%202026-10-23%20225507.png)

- **Screenshot:** `Screenshot 2026-10-23 225507.png`
- **Observed output:** Acknowledgement for EXP005, amount INR 600, category Travel, status Submitted.
- **Sender displayed:** Vedant.Shitole Btech2023.
- **Verification:** Demonstrates the successful submission email output.

## Supporting Visuals

If retained from the documentation package, these files provide additional context:

- `Screenshots/01_Workflow_Published.png`: n8n publishing confirmation.
- `Diagrams/Workflow_Architecture.png`: explanatory diagram of the 11-node workflow.

## Evidence Coverage

These four email screenshots show successful acknowledgement and both rejection messages. They do not independently show spreadsheet rows or the executed node paths. Add genuine screenshots of the Claims tab, Audit Log tab, complete workflow and execution branches to document those outputs visually. Spreadsheet results already confirmed during testing are recorded separately in `Test_Results.md`.
