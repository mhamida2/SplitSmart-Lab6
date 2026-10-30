# Feature Specification: SplitSmart

**Feature Branch**: `001-splitsmart`

**Created**: 2026-10-30

**Status**: Draft

**Input**: User description: "SplitSmart customer requirements as recorded in UserRequirement.md and governed by .specify/memory/constitution.md"

## Student-Confirmed Lab Decisions

The following decisions are confirmed for this Lab 6 documentation scope and apply throughout this specification:

- Email/password authentication is required for the lab specification.
- Invalid credentials deny access.
- In-app notifications are required for the course MVP only; email, push, and SMS are outside the proposed MVP.
- For group expenses, notifications are sent to all current group members except the expense creator.
- For direct expenses, notifications are sent to the payer and all participants except the creator.
- The course MVP uses single-stage approval.
- For group expenses, the group administrator approves or rejects the expense, including administrator-created expenses.
- For direct expenses, all involved users other than the creator must approve; any rejection rejects the expense.
- Pending or rejected expenses never affect balances; approved expenses apply to balances exactly once.
- Rejected expenses remain visible to authorized users with a required rejection reason; rejected is distinct from pending.
- Payments are recorded as settlements already made outside the application; they are not transfer processing.
- Partial or full settlement is allowed, but payments exceeding the current debt between the selected debtor and creditor in the selected group/direct context are rejected.
- Basic expense, balance, and payment summaries are mandatory; date-range/custom reports are optional enhancements.
- Java Swing is a course desktop MVP prototype only. Web, Android, and iOS remain full-product requirements.
- Lab 6 is requirements documentation only; implementation, automated tests, and working software are future obligations.

---

## Actors

- **User**: An individual who creates an account, joins groups, records expenses, reviews balances, and settles obligations.
- **Group Creator / Administrator**: A user who initiates a group and manages who belongs to it.
- **Group Member**: A user who participates in shared expenses within a group.
- **Approver**: A user or role responsible for reviewing and approving expenses before they affect balances.
- **System**: The SplitSmart application, including notification delivery, balance calculation, payment recording, and reporting features.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Create a personal account and access expense information (Priority: P1)
A new user wants to create a personal account and view only their own expense and balance information.

**Why this priority**: Without a user account and restricted access, the system cannot safely manage personal financial data or group activity.

**Independent Test**: A user can create an account, sign in, and see only their own records and balances within the scope of their permissions.

**Acceptance Scenarios**:
1. **Given** a user has not created an account, **When** they provide the required account data, **Then** the system creates a user account and allows them to log in.
2. **Given** a user has an email and password account, **When** they sign in with valid credentials, **Then** the system grants access to their permitted expense and balance information.
3. **Given** a user enters invalid credentials, **When** they attempt to sign in, **Then** the system denies access and does not reveal any protected information.
4. **Given** a user is signed in, **When** they access the system, **Then** they can view only their own expense and balance information and not another user’s private information.

---

### User Story 2 - Create and manage groups for shared spending (Priority: P1)
A user wants to organize shared expenses with roommates, trip participants, family, or coworkers by creating and managing a group.

**Why this priority**: Group-based tracking is central to the product’s purpose and required for all shared-expense workflows.

**Independent Test**: A user can create a group, add participants, and manage group membership within that group context.

**Acceptance Scenarios**:
1. **Given** a signed-in user, **When** they create a group, **Then** they become the administrator of that group.
2. **Given** a group administrator, **When** they edit group details or add or remove members, **Then** the changes are applied only by the administrator.
3. **Given** a current group member, **When** they create an expense within that group, **Then** the expense is accepted if the payer and participants are valid members of the group.
4. **Given** an existing group, **When** a member is removed, **Then** the member cannot participate in new group expenses but existing obligations, payments, and historical expense records remain preserved.

---

### User Story 3 - Create a shared expense with controlled split logic and approval (Priority: P1)
A user wants to record a shared expense that includes participants, an amount, a date, a description, a split method, and an optional receipt image.

**Why this priority**: Expense creation is the core workflow that drives balance and payment calculations.

**Independent Test**: A user can create an expense, assign participants, define split rules, and submit it for approval before the system updates balances.

**Acceptance Scenarios**:
1. **Given** a valid group and participating users, **When** a user records an expense with amount, date, description, split method, and involved users, **Then** the system captures the expense record for review.
2. **Given** a valid group expense, **When** the payer and participants are checked against the group membership, **Then** only valid group members may be included.
3. **Given** an entered amount is blank, nonnumeric, zero, or negative, **When** the user tries to save or submit the expense, **Then** the system rejects the entry, shows a clear validation message, and leaves existing records and balances unchanged.
4. **Given** a percentage-based split, **When** the sum of all percentage allocations is not exactly 100%, **Then** the system rejects the split, shows the current total, and keeps the expense from being saved or submitted for approval.
5. **Given** a percentage-based split with a total of exactly 100%, **When** the user submits the expense for approval, **Then** the system accepts the allocation for review.
6. **Given** a valid equal split or percentage split, **When** the total amount is not evenly divisible among participants, **Then** the system allocates the exact share using BigDecimal, rounds each share down to two decimal places, distributes any leftover cents one at a time to participants with the largest discarded fractional remainders, and breaks ties by ascending unique user ID so the total equals the original amount.
7. **Given** a participant assigned 0% in a percentage-based split, **When** the expense is approved, **Then** that participant receives no charge.
8. **Given** an expense is awaiting review, **When** the approval process is completed, **Then** the system applies the approved expense to user balances.
9. **Given** an expense includes an optional receipt image, **When** it is attached to the expense record, **Then** the receipt is associated with that expense and available for review.

---

### User Story 4 - Receive notifications and approve or review expenses (Priority: P1)
A group member needs to know when another member creates a new expense and needs a review path before the expense affects balances.

**Why this priority**: The customer explicitly requires notifications and an approval workflow, both of which are required for safe financial accounting.

**Independent Test**: A user receives a notification about a new expense and can review or approve it according to the configured workflow.

**Acceptance Scenarios**:
1. **Given** a user is a current group member in a group with a new group expense, **When** the expense is created by another member, **Then** the system sends an in-app notification to all current group members except the expense creator.
2. **Given** a direct expense involving multiple users, **When** the expense is created by one user, **Then** the system sends an in-app notification to the payer and all participants except the expense creator.
3. **Given** a user is notified of a new expense, **When** they open the notification or review queue, **Then** they can access the pending expense for approval or rejection.
4. **Given** the course MVP approval workflow, **When** an expense is submitted for approval, **Then** the system does not apply the expense to balances until the required approver(s) approve it.
5. **Given** an expense is submitted for approval, **When** it is reviewed and approved, **Then** the system updates balances only after approval.

---

### User Story 5 - Track balances and record payments (Priority: P1)
A user needs to see what they owe or are owed and record payments to settle balances.

**Why this priority**: This is a direct operational necessity for shared-expense systems and a key customer requirement.

**Independent Test**: After approved expenses are posted, the system shows balances between users and supports payment recording that updates those balances.

**Acceptance Scenarios**:
1. **Given** approved expenses are in the system, **When** balances are calculated, **Then** each user can view amounts owed to or by each other user.
2. **Given** a user records a payment, **When** the payment is blank, nonnumeric, zero, negative, or exceeds the current debt between the selected debtor and creditor in the selected group/direct context, **Then** the system rejects the payment, shows a clear validation message, and leaves existing records and balances unchanged.
3. **Given** a user records a payment with more than two decimal places, **When** the data is submitted, **Then** the system rejects the payment before saving.
4. **Given** a valid payment to settle part or all of a current balance, **When** the payment is saved, **Then** the balance reflects the payment exactly once and the payment history is preserved.
5. **Given** a payment is recorded outside the application, **When** the user enters it into the system, **Then** the payment is recorded as an acknowledged settlement without transferring funds through the application itself.

---

### User Story 6 - Generate reports and summaries (Priority: P2)
A user wants to review expenses, balances, and payments in a summarized format and, if needed, with date-range filtering.

**Why this priority**: Reporting supports user oversight and is explicitly required by the customer.

**Independent Test**: A user can request summary reports for expense, balance, and payment activity, and optional custom/date-range reporting is clearly separated as an enhancement.

**Acceptance Scenarios**:
1. **Given** multiple expenses and payments exist, **When** a user requests a basic summary, **Then** the system returns the relevant expense, balance, and payment totals for the selected user or group.
2. **Given** a report is requested for a user or group, **When** the report is generated, **Then** it includes the relevant balances and payment activity for those records.
3. **Given** date-range or custom report features are considered, **When** they are implemented beyond the lab, **Then** they are treated as optional enhancements and not as mandatory lab deliverables.

---

### User Story 7 - Use the product across supported platforms (Priority: P2)
A user expects the product to be usable on web and mobile platforms as required by the full customer scope.

**Why this priority**: This requirement is explicit in the customer request and forms part of the product’s intended deployment scope.

**Independent Test**: The product is described as web-first with Android and iOS support, but implementation is deferred beyond Lab 6.

**Acceptance Scenarios**:
1. **Given** the full product requirement, **When** implementation proceeds beyond this lab, **Then** the system must be designed and delivered for web, Android, and iOS support.
2. **Given** the course lab context, **When** Java Swing is used, **Then** it is documented as a desktop MVP prototype only and not as a replacement for the full product platform requirement.
3. **Given** the course MVP list, **When** the system is documented, **Then** the documented features are limited to account/login, groups, equal and percentage expense splits, optional receipts, in-app notifications, single-stage approval, balances, payment recording, and basic summaries.

---

### Edge Cases

- What happens when a group contains users whose balances become negative or zero after a transaction?
- What happens when an expense is created with participants who are not in the selected group?
- What happens when a split-by-percentage expense totals less than or greater than 100%? [RESOLVED: reject before save or approval; show current total and validation message; do not normalize automatically]
- What happens when an approval is rejected or withdrawn after being submitted?
- What happens when a payment exceeds the current balance owed by a user? [STUDENT-CONFIRMED LAB DECISION: reject if it exceeds the current debt between the selected debtor and creditor in the selected group/direct context]
- What happens when a receipt image is missing, corrupted, or too large to store? [NEEDS CLARIFICATION]
- What happens when a report is requested for a date range with no matching records?

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The system MUST allow each user to create and maintain a personal account using email and password authentication. Users MUST sign in to access their permitted expense and balance information. Invalid credentials MUST deny access.
- **FR-002**: The system MUST restrict access so a user can view only their own information and the information relevant to groups they belong to, subject to group membership rules. [NEEDS CLARIFICATION: exact access-control model and permission granularity]
- **FR-003**: The system MUST allow any signed-in user to create a group and become that group’s administrator. Only the administrator may edit group details and add or remove members. Any current group member may create expenses within that group.
- **FR-004**: The system MUST allow users to create new expenses containing at least: amount, date, description, split method, and involved users.
- **FR-004A**: The system MUST reject blank, nonnumeric, zero, or negative entered amounts for expenses before saving or submitting for approval. A clear validation message MUST be shown, and existing records and balances MUST remain unchanged.
- **FR-004B**: For the lab’s single-currency model, the system MUST reject any expense amount with more than two decimal places before saving or submission.
- **FR-005**: The system MUST support expense splitting by equal share and by percentage allocation.
- **FR-005A**: For percentage-based expense splits, each participant’s percentage MUST be a valid number between 0% and 100%, inclusive.
- **FR-005B**: Percentage allocations MUST total exactly 100%. The system MUST reject totals below or above 100% before saving or submitting for approval. The system MUST display the current total and a clear validation message. The system MUST NOT automatically normalize percentages or assign an unspecified remainder.
- **FR-005C**: For both equal and percentage split calculations, the system MUST calculate exact shares using `BigDecimal`, round each share down to two decimal places, distribute leftover cents one at a time to participants with the largest discarded fractional remainders, break ties by ascending unique user ID, and ensure the final shares total exactly the original expense amount.
- **FR-005D**: A participant assigned 0% in a percentage-based split MUST receive no charge.
- **FR-006**: The system MUST support an optional receipt image attached to an expense record.
- **FR-007**: The system MUST notify group members when other group members create a new expense.
- **FR-007A**: Student-confirmed lab decision: In-app notifications are required for the proposed MVP. For group expenses, notify all current group members except the expense creator. For direct expenses, notify the payer and all participants except the expense creator. Email, push, and SMS notifications are outside the proposed MVP.
- **FR-008**: The system MUST include an approval workflow for reviewing and approving expenses before they affect balances.
- **FR-008A**: Student-confirmed lab decision: The course MVP uses a single-stage approval workflow. For group expenses, the group administrator approves or rejects, including administrator-created expenses. For direct expenses, all involved users other than the expense creator must approve; any rejection rejects the expense. Pending or rejected expenses never affect balances. Approved expenses apply to balances exactly once.
- **FR-008B**: Student-confirmed lab decision: Rejected expenses remain visible to authorized users with a required rejection reason. Rejected is a distinct status from pending. Editing and resubmission rules remain explicitly deferred.
- **FR-009**: The system MUST ensure that approved expenses update user balances in the system.
- **FR-010**: The system MUST track the balance owed by each user to every other user based on the set of approved expenses in the system.
- **FR-011**: The system MUST allow users to record payments made to settle balances owed.
- **FR-011A**: The system MUST reject blank, nonnumeric, zero, or negative payment amounts before saving. A clear validation message MUST be shown, and existing records and balances MUST remain unchanged.
- **FR-011B**: The system MUST reject any payment amount with more than two decimal places before saving.
- **FR-011C**: Student-confirmed lab decision: Record payments already made outside the application; do not process money transfers. Allow partial or full settlement. Reject payments exceeding the current debt between the selected debtor and creditor in the selected group/direct context. Preserve payment history and update the balance exactly once.
- **FR-012**: The system MUST update balances to reflect recorded payments and maintain payment history.
- **FR-013**: The system MUST provide users with reports and summaries of expenses, balances, and payments.
- **FR-013A**: Student-confirmed lab decision: Basic expense, balance, and payment summaries are mandatory for the course lab.
- **FR-014**: Student-confirmed lab decision: Date-range and custom report features are optional enhancements. They are not mandatory lab deliverables. The system MUST provide basic summaries for expenses, balances, and payments, and any additional date-range/custom reporting remains optional and deferred.
- **FR-015**: The full product MUST support a primary web-based application and also Android and iOS app support. [NEEDS CLARIFICATION: exact deployment and feature parity targets]
- **FR-016**: The system MUST preserve the original customer requirement inventory without dropping any of the stated expense-sharing features from the product scope.
- **FR-017**: Membership changes MUST preserve historical expenses, payments, and outstanding balances. Removing a member prevents participation in new group expenses but does not erase existing obligations.

### Java Swing Desktop MVP Subset (Student-confirmed lab decision)

The following items describe the proposed course desktop MVP only and are explicitly separate from the full-product scope.

- **FR-MVP-001**: The Java Swing desktop MVP MAY provide the following features: account/login, groups, equal and percentage expense splits, optional receipts, in-app notifications, single-stage approval, balances, payment recording, and basic summaries. [STUDENT-CONFIRMED LAB DECISION]
- **FR-MVP-002**: The Java Swing desktop MVP is a course desktop prototype only and MUST be clearly labeled as such. [STUDENT-CONFIRMED LAB DECISION]
- **FR-MVP-003**: Java Swing MUST NOT be treated as satisfying the customer’s full-product requirement for web, Android, and iOS support. [STUDENT-CONFIRMED LAB DECISION]
- **FR-MVP-004**: Lab 6 requires documentation only. Implementation, automated testing, and working software are future obligations and not deliverables for this lab. [STUDENT-CONFIRMED LAB DECISION]

### Non-Functional Requirements

- **NFR-001**: The system MUST protect user and financial data with appropriate access control and privacy safeguards for personal expense information.
- **NFR-002**: The system MUST maintain financial correctness for balances and payments by using clear validation and consistent calculation rules.
- **NFR-003**: The system MUST support reliable reporting and balance calculations for all approved expenses and recorded payments within the system.
- **NFR-004**: The system MUST be usable enough for typical group and personal finance tasks without requiring specialized training or undocumented procedures.
- **NFR-005**: The system MUST keep the requirements, design artifacts, and implementation aligned with the governing constitution and project traceability practices.
- **NFR-006**: Any future implementation MUST keep all monetary values in `BigDecimal` and avoid floating-point representations for money calculations.
- **NFR-007**: The project MUST maintain a clear separation between GUI concerns and business logic, especially for validation, approvals, and financial calculations.
- **NFR-008**: The project MUST document any design or implementation decisions that differ from the full product requirements, including the Java Swing desktop MVP context.

### Key Entities

- **User**: Represents an individual with an account and access to their own records and group participation.
- **Group**: Represents a collection of users sharing expenses together.
- **Expense**: Represents an approved or pending expense with amount, date, description, split method, participants, and optional receipt.
- **Approval**: Represents the review and approval state of an expense before it affects balances.
- **Balance**: Represents the net financial relationship between two users or between a user and a group context.
- **Payment**: Represents a recorded settlement of amounts owed.
- **Report**: Represents a summary or filtered view of expenses, balances, or payments.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: A user can create an account, join or create a group, and create a valid expense record in the documented workflow.
- **SC-002**: Approved expenses produce consistent computed balances for all involved users.
- **SC-003**: Payments recorded by users update balances and retain an accurate payment history.
- **SC-004**: A user can request a basic summary covering expenses, balances, and payments for a selected user or group, and date-range/custom versions remain optional enhancements.
- **SC-005**: A human reviewer can trace each customer requirement to a corresponding requirement, design discussion, or future implementation artifact.
- **SC-006**: The Java Swing desktop MVP, if pursued, is explicitly identified as a course prototype and does not replace the full-product web, Android, and iOS requirements.
- **SC-007**: A group administrator can manage group membership, while current members can create expenses without altering historical obligations or payment history when a member is removed.
- **SC-008**: When an expense or payment amount is blank, nonnumeric, zero, or negative, the system rejects the data, shows a clear validation message, and leaves existing records and balances unchanged.
- **SC-009**: Calculated balances may be positive, negative, or zero to represent who owes whom, and zero indicates no outstanding debt.
- **SC-010**: For percentage-based splits, a total percentage of exactly 100% is required before an expense can be approved, and totals outside 100% are rejected with a clear current-total message.
- **SC-011**: For equal and percentage splits, the system calculates exact shares with `BigDecimal`, floors each share to two decimal places, assigns leftover cents to participants with the largest fractional remainder using ascending unique user ID as the tie-breaker, and final shares always sum exactly to the original expense amount.
- **SC-012**: Example: $10 split equally among three participants produces $3.34, $3.33, and $3.33; the lowest user ID receives the extra cent when needed.
- **SC-013**: A user with valid email and password credentials can sign in and access permitted information; invalid credentials are denied access.
- **SC-014**: Student-confirmed lab decision: In-app notifications are delivered to the correct recipients for group and direct expenses, and no notification channel beyond in-app is required for the course MVP.
- **SC-015**: Student-confirmed lab decision: A single-stage approval workflow is enforced for the course MVP; pending or rejected expenses never affect balances, and approved expenses apply to balances exactly once.
- **SC-016**: Student-confirmed lab decision: A payment that exceeds the current debt between the selected debtor and creditor in the selected group/direct context is rejected, while valid partial or full settlements are preserved in payment history and update balances exactly once.
- **SC-017**: Student-confirmed lab decision: Java Swing is a course desktop MVP prototype only; web, Android, and iOS remain full-product requirements, and Lab 6 remains documentation-only.

## Assumptions

- The project is a shared-expense system intended for personal, household, trip, and team use.
- The requirements in `UserRequirement.md` are authoritative and supersede any assumptions introduced by the course context.
- The Java Swing desktop implementation is a learning-oriented prototype and not a full deployment commitment for all target platforms.
- This lab documents only the course MVP scope; full-product requirements and additional operational details remain deferred for later implementation.
- Any broader product-level approval, notification, access-control, or reporting rules beyond the confirmed lab decisions are future design items and not lab deliverables.

## Open Questions

1. What is the exact access-control model for personal and group information beyond the confirmed lab decisions? [FULL-PRODUCT FUTURE DECISION; DEFERRED]
2. What is the final full-product approval model beyond the course MVP? [FULL-PRODUCT FUTURE DECISION; DEFERRED]
3. What are the exact notification requirements for the full product beyond in-app messaging? [FULL-PRODUCT FUTURE DECISION; DEFERRED]
4. What is the exact structure of optional date-range or custom reports for the full product? [FULL-PRODUCT FUTURE DECISION; DEFERRED]
5. What receipt-image constraints are required for the full product: file type, size limit, storage location, and retention period? [FULL-PRODUCT FUTURE DECISION; DEFERRED]
6. What constitutes the minimum required mobile/web parity for the full product, and which features must be available on each platform? [FULL-PRODUCT FUTURE DECISION; DEFERRED]
7. What is the exact role model for group administrators versus ordinary members in the full product? [FULL-PRODUCT FUTURE DECISION; DEFERRED]
