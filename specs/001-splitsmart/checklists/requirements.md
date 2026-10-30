# Requirements Review Checklist

## Scope and Requirement Status

- [ ] Original customer requirements from `UserRequirement.md` have been reviewed as the authoritative product scope.
- [ ] The lab constitution and decision log have been applied as governing constraints for this documentation-only assignment.
- [ ] The Java Swing desktop MVP is clearly labeled as a course prototype and not as a replacement for web, Android, or iOS product requirements.
- [ ] The full-product requirement for web, Android, and iOS remains explicitly separate from the course desktop MVP scope.
- [ ] Lab 6 is documented as requirements-only work; implementation, testing, and working software remain future obligations.

## Requirements Coverage Inventory

- [ ] User accounts and login are documented.
- [ ] Group creation and administration are documented.
- [ ] Group membership and removal behavior are documented.
- [ ] Direct expense and group expense workflows are documented.
- [ ] Expense amount validation is documented.
- [ ] Equal and percentage split behaviors are documented.
- [ ] Receipt support is documented as optional.
- [ ] Notification behavior is documented.
- [ ] Approval and rejection workflows are documented.
- [ ] Balance tracking is documented.
- [ ] Payment recording and settlement rules are documented.
- [ ] Basic summary reporting is documented.
- [ ] Optional custom/date-range reporting is intentionally marked as enhancement-only.
- [ ] Requirement IDs and traceability notes are preserved.

## Student-Confirmed Lab Decisions

- [ ] Notifications use in-app notifications only for the proposed MVP.
- [ ] Notifications are sent to current group members except the expense creator for group expenses.
- [ ] Notifications are sent to the payer and participants except the creator for direct expenses.
- [ ] Email, push, and SMS notifications are explicitly outside the proposed MVP.
- [ ] The course MVP uses single-stage approval.
- [ ] For group expenses, the group administrator approves or rejects the expense.
- [ ] For direct expenses, all involved users except the creator must approve; a single rejection rejects the expense.
- [ ] Pending and rejected expenses are excluded from balance impact.
- [ ] Approved expenses apply to balances exactly once.
- [ ] Rejected expenses remain visible to authorized users with a required rejection reason.
- [ ] Rejected status is separate from pending status.
- [ ] Editing and resubmission rules remain deferred.
- [ ] Payments are recorded as payments already made outside the application and are not processed as transfers.
- [ ] Partial or full settlement is allowed.
- [ ] Payments exceeding current debt in the selected context are rejected.
- [ ] Payment history is preserved and balances update exactly once.
- [ ] Basic expense, balance, and payment summaries are mandatory.
- [ ] Date-range/custom reports are optional enhancements, not mandatory lab deliverables.
- [ ] Java Swing is explicitly identified as a course desktop MVP prototype only.
- [ ] Web, Android, and iOS remain full-product requirements outside this lab scope.

## Remaining Open Questions / Unresolved Issues

- [ ] Approval role and workflow details beyond the course MVP remain intentionally limited and not yet finalized for full product implementation.
- [ ] Notification channel requirements beyond in-app notifications remain unconfirmed for the full product.
- [ ] Reporting format details for optional custom/date-range reports remain unconfirmed.
- [ ] Exact access-control and permission details beyond the documented lab decisions remain unconfirmed.
- [ ] Full product parity requirements for Android and iOS remain future obligations, not lab deliverables.
- [ ] Any final product-level differences between group and direct-expense approval flows remain deferred.

## Honest Review Summary

This checklist reflects the current documentation boundary for Lab 6. It intentionally leaves unresolved questions unclaimed and avoids inventing business rules. The lab is a requirements-documentation exercise, not an implementation task.
