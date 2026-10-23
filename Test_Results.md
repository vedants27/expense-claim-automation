# Test Results — Expense Claim Automation

## Test Method

Manual functional testing covered a valid new claim, an invalid amount, a duplicate Claim ID and a submission through the published production form. Email screenshots provide direct evidence of the displayed messages. Spreadsheet and audit outcomes below are identified as operator-confirmed where no corresponding screenshot is attached.

The exported JSON contains the 11 configured workflow nodes. This configuration check is separate from live execution testing.

## Functional Test Results

| Test ID | Scenario / input | Expected result | Actual result | Status and evidence |
|---|---|---|---|---|
| TC01 | New claim EXP005, INR 600, Travel | Append one claim with Submitted status; send acknowledgement; append Submitted audit | Acknowledgement displays EXP005, INR 600, Travel and Submitted. The operator confirmed the combined Claims/Audit Log results for TC01–TC03. | Pass — email screenshot plus operator-confirmed sheet results |
| TC02 | EXP006, INR -100 | Reject amount; send explanation; do not append a claim; append Rejected audit | Email displays EXP006, INR -100 and the reason that the amount must be greater than zero. Audit outcome was confirmed in the combined test. | Pass — email screenshot plus operator confirmation |
| TC03 | Resubmit EXP005, INR 600 | Reject duplicate; send notification; do not append another claim; append Rejected audit | Operator confirmed duplicate rejection and a combined total of one new claim and three audit entries across TC01–TC03. | Pass — operator confirmation; attached duplicate screenshot is from the earlier EXP001 test |
| TC04 | Earlier duplicate test: EXP001 already present | Send duplicate rejection without creating an additional claim | Screenshot shows the duplicate rejection for EXP001, sent from the earlier Varun credential. Sender was subsequently corrected and retested. | Duplicate notification verified; screenshot represents the earlier credential configuration |
| TC05 | Publish the workflow | Production form becomes available | n8n displayed the Workflow published confirmation. | Pass — publishing confirmation supplied during implementation |
| TC06 | Production form submission: EXP007, INR 850, Travel | Trigger automatically; save claim; send acknowledgement; append Submitted audit | Screenshot shows EXP007 acknowledgement from Vedant's displayed sender account. Operator confirmed email receipt and spreadsheet update without manual execution. The production audit entry was not separately confirmed. | Pass for production trigger, email and claim update; audit verification not separately recorded |
| TC07 | Correct Gmail sender after earlier account mismatch | Messages use the intended Vedant account | Sender credential was corrected and retested. EXP005, EXP006 and EXP007 screenshots display Vedant.Shitole Btech2023. | Pass for the displayed outputs; earlier EXP001 screenshot still shows Varun |

## Screenshot References

| Evidence | File in Screenshots/ | Supports |
|---|---|---|
| Earlier duplicate notification, EXP001 | `Screenshot 2026-10-23 225415.png` | TC04 |
| Production acknowledgement, EXP007 | `Screenshot 2026-10-23 225429.png` | TC06 and sender output in TC07 |
| Invalid amount rejection, EXP006 | `Screenshot 2026-10-23 225456.png` | Email output in TC02 and TC07 |
| Successful acknowledgement, EXP005 | `Screenshot 2026-10-23 225507.png` | Email output in TC01 and TC07 |

See [Screenshot_Index.md](Screenshot_Index.md) for the images and captions.

## Combined Spreadsheet Verification

For the fresh EXP005 submission, EXP006 negative amount and repeat EXP005 submission, the operator confirmed:

- **Claims:** one new accepted-submission row across the three tests.
- **Audit Log:** three new rows, covering one Submitted outcome and two Rejected outcomes.
- **Emails:** acknowledgement and the corresponding rejection notifications were received.

The claim remains **Submitted**. This workflow does not approve or reimburse expenses. Capture the actual sheet rows and execution IDs for stronger visual evidence; the email messages alone do not prove the stored row counts.

## Additional Checks and Limits

| Check | Expected behaviour / limitation | Test evidence |
|---|---|---|
| Amount exactly zero | Rejected by the configured Amount > 0 condition | Configuration verified; separate zero-value live run not recorded |
| Required form field omitted | Form prevents incomplete submission | Required flags present in export; separate live run not recorded |
| Gmail or Sheets service failure | Requires execution inspection; no custom technical recovery flow configured | Not fault-injected |
| Concurrent submissions using the same ID | Separate lookup and append can permit a race; database uniqueness would be needed for a stronger guarantee | Not load-tested |

Audit nodes run after the corresponding Gmail node. A failed send can prevent a downstream audit entry. A valid claim may already be stored before an email failure.

## Reproducing the Main Scenarios

1. Record the starting row counts in Claims and Audit Log.
2. Submit an unused Claim ID with INR 500 and your own recipient address. Check one claim row, acknowledgement and Submitted audit.
3. Submit another unused ID with INR -100. Check rejection, no new claim and a Rejected audit.
4. Submit the first ID again with INR 500. Check duplicate rejection, no additional claim and another Rejected audit.
5. Compare row counts: Claims should increase by one and Audit Log by three after all three branches complete successfully.
6. Save genuine output screenshots and execution IDs. Use different IDs for each new testing session.
