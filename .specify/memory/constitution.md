# SplitSmart Constitution

## Core Principles

### I. Customer Requirements Are Authoritative
The original SplitSmart customer request in `UserRequirement.md` defines the product scope, priorities, and acceptance intent. Customer requirements are binding and must not be replaced by technology preferences or course constraints.

Customer requirements include:
- user accounts and secure access to personal expense and balance information
- group creation and management for shared expenses
- expense creation with amount, date, description, split method, optional receipt image, and involved users
- notifications when group members create expenses
- approval workflow before expenses affect balances
- balance tracking between users
- payment tracking for reconciled balances
- reporting and summaries by date range and other criteria
- primary web support with Android and iOS app support

These are full-product requirements and remain the source of truth for product scope.

### II. Course Desktop MVP Is a Limited Implementation Choice, Not a Product Requirement
Java Swing is the course desktop MVP implementation choice for this lab. It is a teaching artifact and a desktop prototype only. It must not be described as satisfying the customer’s full-product requirements for web, Android, or iOS delivery.

The customer requirement for primary web deployment and Android/iOS app support remains fully in force. Any future product implementation may involve mobile/web architecture, but Java Swing is not the equivalent of those platform obligations.

### III. Architecture Must Separate UI from Business Logic
The system must follow clear separation of concerns. GUI code, including Swing presentation and event handling, must remain focused on interaction and display. Business rules, financial calculations, validations, approval logic, balance updates, and state transitions must live in domain and service logic outside the UI layer.

This separation is required to:
- reduce coupling between user interface and core operations
- improve maintainability and testing
- protect financial correctness from UI-side errors
- make requirement traceability easier and more reliable

### IV. Object-Oriented Design and Financial Integrity Are Non-Negotiable
The system shall be developed using object-oriented design principles, including encapsulation, cohesion, modularity, clear responsibilities, and loose coupling. Domain concepts such as User, Group, Expense, Approval, Payment, and Balance must be modeled explicitly and consistently.

All money values must use `BigDecimal` rather than floating-point types. Monetary calculations must be explicit, deterministic, and auditable. Expense splitting, balance settlement, and reporting totals must use consistent rounding and scale rules. Financial logic must be treated as a correctness-critical domain concern.

### V. Requirements Traceability, Documentation, and Human Review Are Mandatory
The project must maintain explicit traceability from customer requirements to design decisions, workflow documentation, and validation artifacts. Every requirement should be linked to at least one design element or acceptance mechanism.

The project shall use PlantUML for modeling key system structures and workflows. JUnit 5 shall be used for automated testing in future implementation work. Human review is required for all AI-generated work, including analysis, diagrams, and code suggestions. AI output may support productivity, but it does not replace human responsibility for correctness, architecture, and requirement compliance.

## Additional Constraints

### Technology and Development Constraints
- Java 21 is the target language version for future implementation work.
- Maven is the build and dependency framework for the project.
- Java Swing is the course desktop MVP implementation choice only.
- JUnit 5 is required for future automated testing.
- PlantUML is required for system and workflow documentation.
- Object-oriented design must guide the structural organization of the domain model.
- GUI and business logic must remain separated.
- `BigDecimal` must be used for all money values.
- Requirement traceability must be maintained during design and review.
- Human review of AI-generated output is mandatory before acceptance.

### Lab 6 Scope and Deliverable Boundaries
This lab specifically delivers requirements documentation and project constitution artifacts. It does not deliver production implementation, automated tests, or working software. Implementation, testing, and deployment activities are future development obligations, not deliverables for this lab.

This means:
- requirement definition is in scope
- architecture and governance documentation are in scope
- implementation in Java, Maven, Swing, or other technologies is out of scope for this lab
- automated test execution and verification are future obligations
- a functioning user-facing application is not a required outcome of Lab 6

## Development Workflow and Governance

### Requirement-Driven Delivery
All work must remain aligned to the customer requirements in `UserRequirement.md`. Implementation choices may support delivery, but they may not invalidate or replace the original customer scope.

### Review and Quality Gates
- product and requirement changes must be reviewed against the original customer request
- architecture decisions must be documented explicitly and separated from requirement statements
- financial logic must be reviewed carefully before implementation or approval
- AI-assisted content must be checked by a human reviewer before being accepted
- documentation must reflect the actual project scope and the Lab 6 boundary conditions

### Change Control
This constitution governs the project’s required documentation and decision-making framework. Any amendment must be documented clearly, dated, and approved by the responsible project stakeholders. Technology decisions may change over time, but the underlying customer requirements remain authoritative.

**Version**: 1.0.0 | **Ratified**: 2026-10-30 | **Last Amended**: 2026-10-30
