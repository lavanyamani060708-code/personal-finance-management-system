# Software Requirements Specification (SRS)
## Personal Finance Management System (PFMS)

**Document version:** 1.0  
**Status:** Academic project draft  
**Prepared for:** Software Engineering / SRS submission  
**Date:** 29 September 2026

---

## Document Control

| Item | Details |
|---|---|
| Project name | Personal Finance Management System (PFMS) |
| Document type | Software Requirements Specification |
| Version | 1.0 |
| Intended audience | Students, evaluator, developers, testers |
| Standard structure | IEEE 830-style SRS |

> **Note:** This is an IEEE 830-style academic specification. The requirements below define a proposed first version and should be confirmed with the project supervisor before implementation.

# 1. Introduction

## 1.1 Purpose

This Software Requirements Specification (SRS) describes the functional and non-functional requirements for the Personal Finance Management System (PFMS). It defines the system's scope, users, interfaces, constraints, assumptions, and expected behavior so that developers and testers can build and verify the application.

The intended readers are the project team, academic evaluator, testers, and anyone responsible for maintaining the application.

## 1.2 Scope

PFMS is a web-based application that helps individual users record and organize personal income and expenses, create budgets, monitor spending, set savings goals, and view financial summaries.

The system will allow a registered user to:
- Create an account and securely sign in.
- Record, edit, view, and delete income and expense transactions.
- Categorize transactions.
- Create monthly budgets and monitor spending against them.
- Set savings goals and record contributions.
- View summaries and reports for a selected date range.
- Search and filter transaction history.
- Export transaction data as a CSV file.

The first version is intended for personal financial organization. It will **not** connect to banks, initiate payments, provide investment advice, calculate tax liabilities, or verify the accuracy of user-entered financial data.

## 1.3 Definitions, Acronyms, and Abbreviations

| Term | Definition |
|---|---|
| PFMS | Personal Finance Management System |
| SRS | Software Requirements Specification |
| User | A registered individual using the system |
| Transaction | A recorded income or expense entry |
| Income | Money received by the user |
| Expense | Money spent by the user |
| Budget | A planned spending limit for a period and category |
| Savings goal | A target amount a user intends to save |
| Dashboard | Main screen showing financial summaries |
| CSV | Comma-Separated Values file format |
| UI | User Interface |
| DB | Database |

## 1.4 References

- IEEE Std 830-1998, *IEEE Recommended Practice for Software Requirements Specifications* (structure used as a guide).
- Project-specific requirements and decisions documented by the project team.

## 1.5 Overview

Section 2 describes the product context, users, operating environment, constraints, and assumptions. Section 3 lists the external interface requirements, functional requirements, data requirements, and non-functional requirements. Section 4 provides use cases and acceptance criteria. Section 5 contains additional notes.

# 2. Overall Description

## 2.1 Product Perspective

PFMS is a standalone, browser-based personal finance application. It consists of:
1. A frontend web interface for user interaction.
2. An application/backend layer that validates and processes requests.
3. A database that stores user accounts, transactions, budgets, and savings goals.

The system is not required to integrate with external banking services in version 1.

### High-level system context

```text
User
  |
  v
Web Browser / User Interface
  |
  v
Application Logic and Authentication
  |
  v
Database
```

## 2.2 Product Functions

At a high level, the system will:
- Register users and authenticate sign-ins.
- Maintain a user's profile and preferred currency.
- Manage income and expense transactions.
- Manage transaction categories.
- Create, update, and monitor budgets.
- Create and track savings goals.
- Display dashboards, summaries, and charts.
- Search, filter, and export transaction history.
- Provide validation and clear error messages.

## 2.3 User Classes and Characteristics

| User class | Description | Main permissions |
|---|---|---|
| Guest | Person who has not signed in | Register, sign in, reset password |
| Registered user | Individual managing personal finances | Access and manage only their own profile, transactions, budgets, goals, and reports |
| System administrator (optional) | Authorized person responsible for technical administration | Manage system configuration and operational health; no routine access to users' financial records |

The first version may omit an in-app administrator dashboard. A registered user must not be able to view or modify another user's financial information.

## 2.4 Operating Environment

The application is expected to run in:
- Modern browsers such as Chrome, Firefox, Edge, and Safari.
- Desktop and laptop devices, with responsive support for mobile browsers.
- A server or managed hosting environment that supports the chosen application stack.
- A database service supported by the selected implementation.

Exact framework, hosting provider, and database are implementation decisions.

## 2.5 Design and Implementation Constraints

- The application must be accessible through a web browser.
- User financial records must be associated with the authenticated account.
- Monetary values must be stored and calculated with appropriate decimal precision; binary floating-point should not be used for currency arithmetic.
- The system must validate user input on both client and server sides where applicable.
- Authentication secrets must not be stored as plain text.
- The application must not claim to connect to banks or verify transactions unless such a feature is separately designed and approved.
- The initial release is limited to one user-defined primary currency per account.

## 2.6 User Documentation

The project should include:
- A short user guide explaining registration, transaction entry, budgets, goals, and reports.
- A README describing setup, configuration, and how to run the application.
- Clear labels, validation messages, and empty-state guidance within the interface.

## 2.7 Assumptions and Dependencies

- Users have access to an internet-connected device and a supported browser.
- Users manually enter or import their own financial information.
- Each account has one primary currency in the first release.
- The system relies on a database and authentication mechanism selected during implementation.
- Email-based password reset depends on a configured email service; if unavailable, the reset flow must be clearly marked as unsupported rather than simulated as successful.
- Financial summaries are only as accurate as the data entered by the user.

# 3. Specific Requirements

## 3.1 External Interface Requirements

### 3.1.1 User Interface

The interface shall provide:
- Registration and sign-in screens.
- A dashboard with income, expenses, balance, budget status, and savings progress.
- A transaction form and transaction history page.
- Budget creation and management screens.
- Savings goal creation and progress screens.
- Reports with date-range and category filters.
- A profile/settings screen.
- Responsive layouts for desktop and mobile-sized screens.
- Accessible form labels, keyboard navigation, and readable contrast.

### 3.1.2 Software Interfaces

The system may use:
- A database for persistent storage.
- An authentication service or secure application-managed authentication.
- An email service for password reset, if implemented.
- A charting library for visual summaries.
- A CSV generation component for data export.

The specific products and APIs are not mandated by this SRS.

### 3.1.3 Hardware Interfaces

No specialized hardware interface is required. The user accesses the system using a general-purpose device with a supported browser.

### 3.1.4 Communication Interfaces

- Client-server communication shall use HTTPS in a deployed environment.
- Data exchanged between the frontend and backend shall use a documented format such as JSON where an API is used.
- The application shall display a clear message when a network or service error prevents an operation from completing.

## 3.2 Functional Requirements

Each requirement is assigned a unique identifier and uses “shall” to indicate a mandatory behavior.

### 3.2.1 Account and Authentication

**FR-001 — User registration:** The system shall allow a guest to create an account using the required registration fields.

**FR-002 — Duplicate account validation:** The system shall reject registration when the chosen unique identifier, such as email address, is already registered.

**FR-003 — Sign-in:** The system shall authenticate a registered user using valid credentials.

**FR-004 — Invalid sign-in:** The system shall reject invalid credentials and display a non-revealing error message.

**FR-005 — Sign-out:** The system shall allow an authenticated user to sign out and end the active session.

**FR-006 — Password reset:** If email reset is configured, the system shall allow a user to request a password reset through a time-limited reset mechanism.

**FR-007 — Session protection:** The system shall restrict protected pages and operations to authenticated users.

### 3.2.2 Profile and Preferences

**FR-008 — View profile:** The system shall allow a user to view their account profile and preferences.

**FR-009 — Update profile:** The system shall allow a user to update permitted profile fields.

**FR-010 — Currency preference:** The system shall allow a user to select one supported primary currency and display monetary values using that currency.

**FR-011 — Account ownership:** The system shall ensure that users can access only their own financial records.

### 3.2.3 Transaction Management

**FR-012 — Add transaction:** The system shall allow a user to create an income or expense transaction with amount, type, category, date, and optional note.

**FR-013 — Validate transaction:** The system shall reject transactions with missing required fields, invalid dates, unsupported transaction types, or non-positive amounts.

**FR-014 — View transactions:** The system shall display the user's transactions in a list ordered by date, with a consistent default ordering.

**FR-015 — Edit transaction:** The system shall allow a user to edit a transaction they own.

**FR-016 — Delete transaction:** The system shall allow a user to delete a transaction they own after confirmation.

**FR-017 — Categorize transaction:** The system shall allow a user to assign a transaction to an available category.

**FR-018 — Search transactions:** The system shall allow a user to search transactions by supported text fields, such as notes or category names.

**FR-019 — Filter transactions:** The system shall allow a user to filter transactions by date range, type, and category.

**FR-020 — Transaction totals:** The system shall calculate income and expense totals for the selected period using the user's recorded transactions.

### 3.2.4 Category Management

**FR-021 — Default categories:** The system shall provide default income and expense categories.

**FR-022 — Custom category:** The system shall allow a user to create a custom category with a name and category type.

**FR-023 — Edit category:** The system shall allow a user to rename a custom category they own, subject to validation.

**FR-024 — Category deletion:** The system shall prevent deletion of a category that is still referenced by transactions unless the user first reassigns or otherwise resolves those transactions.

### 3.2.5 Budget Management

**FR-025 — Create budget:** The system shall allow a user to create a budget with a name, spending category, period, and positive limit.

**FR-026 — Update budget:** The system shall allow a user to edit a budget they own.

**FR-027 — View budget status:** The system shall display the budget limit, eligible expenses, remaining amount, and percentage used.

**FR-028 — Budget warning:** The system shall indicate when spending reaches or exceeds a user-defined budget threshold. The default threshold shall be 80% unless the user configures another supported threshold.

**FR-029 — Budget period:** The system shall calculate budget usage only from transactions whose dates fall within the budget period and category rules.

**FR-030 — Delete budget:** The system shall allow a user to delete a budget they own after confirmation.

### 3.2.6 Savings Goals

**FR-031 — Create savings goal:** The system shall allow a user to create a savings goal with a name, target amount, and optional target date.

**FR-032 — Record contribution:** The system shall allow a user to record a contribution toward a savings goal.

**FR-033 — View goal progress:** The system shall display the target amount, contributed amount, remaining amount, and progress percentage.

**FR-034 — Edit or delete goal:** The system shall allow a user to edit or delete a savings goal they own, with confirmation before deletion.

**FR-035 — Goal completion:** The system shall mark a goal as achieved when its recorded contributions meet or exceed its target amount.

### 3.2.7 Dashboard and Reports

**FR-036 — Dashboard summary:** The system shall display total income, total expenses, and net balance for a selected period.

**FR-037 — Period selection:** The system shall allow the user to select common periods such as current month, previous month, and a custom date range.

**FR-038 — Category breakdown:** The system shall display expense totals grouped by category for the selected period.

**FR-039 — Visual reports:** The system shall present financial summaries using readable tables and/or charts.

**FR-040 — Savings overview:** The system shall display the user's active savings goals and progress on the dashboard or a dedicated summary page.

**FR-041 — CSV export:** The system shall allow a user to export their filtered transaction records to a CSV file.

**FR-042 — Empty states:** The system shall display helpful guidance when no transactions, budgets, goals, or report data exist for the selected view.

### 3.2.8 Errors and Data Handling

**FR-043 — Operation feedback:** The system shall show success feedback after a completed create, update, or delete operation.

**FR-044 — Failure feedback:** The system shall show a clear error message when an operation fails and shall not report it as successful.

**FR-045 — Data consistency:** The system shall ensure that a failed transaction operation does not leave partially updated totals or records.

**FR-046 — Confirmation:** The system shall request confirmation before destructive actions such as deleting a transaction, budget, or savings goal.

## 3.3 Non-Functional Requirements

### 3.3.1 Performance

**NFR-001:** Under normal test conditions, common page views and standard user actions should complete within 3 seconds for 95% of requests, excluding slow external services.

**NFR-002:** The system should support at least 100 concurrent active users in a representative test environment without critical errors. This is a proposed academic target and must be validated through testing.

**NFR-003:** Transaction lists shall use pagination or another bounded-loading approach when the record count becomes large.

### 3.3.2 Security and Privacy

**NFR-004:** The deployed application shall use HTTPS for data in transit.

**NFR-005:** Passwords shall be stored only using a suitable salted password-hashing mechanism, never as plain text.

**NFR-006:** The system shall enforce authorization checks on the server or trusted data layer for every operation involving private financial records.

**NFR-007:** The system shall validate and safely handle user input to reduce risks such as injection and cross-site scripting.

**NFR-008:** The system shall not expose passwords, authentication tokens, or unnecessary financial data in application logs or error messages.

**NFR-009:** The system shall provide a way for a user to request deletion of their account and associated personal financial data, subject to the project's defined retention and backup policy.

**NFR-010:** The system shall collect only personal information needed for the stated functionality.

### 3.3.3 Reliability and Availability

**NFR-011:** The system shall preserve successfully saved records across normal application restarts.

**NFR-012:** The system should achieve 99% monthly availability in the intended hosted environment, excluding planned maintenance. This target is subject to hosting capabilities.

**NFR-013:** The system shall handle database or network failures gracefully and shall not display a successful-save message unless persistence is confirmed.

**NFR-014:** A backup and recovery approach shall be documented for the deployed database.

### 3.3.4 Usability and Accessibility

**NFR-015:** A first-time user should be able to add an income or expense transaction using the interface without consulting external documentation.

**NFR-016:** The interface shall use consistent labels, navigation, date formats, and currency formatting.

**NFR-017:** Forms shall identify required fields and explain validation errors in plain language.

**NFR-018:** The interface should support keyboard navigation and provide labels for form controls.

### 3.3.5 Maintainability and Portability

**NFR-019:** The application code shall be organized into understandable modules with consistent naming and documentation.

**NFR-020:** Configuration secrets shall be kept outside source control and supplied through environment configuration or a secure secrets manager.

**NFR-021:** The project shall include setup instructions and a list of required dependencies.

**NFR-022:** The application shall support current stable versions of the browsers selected for project testing.

### 3.3.6 Data Accuracy

**NFR-023:** Monetary calculations shall use decimal-safe arithmetic and a documented rounding policy.

**NFR-024:** The system shall use a consistent rule for date boundaries and budget periods.

**NFR-025:** Dashboard totals and reports shall be derived from the same underlying transaction records and calculation rules.

## 3.4 Data Requirements

### 3.4.1 Core Data Entities

| Entity | Key fields | Description |
|---|---|---|
| User | user_id, name, email, password_hash or auth_provider_id, currency, created_at | Account and preferences |
| Category | category_id, user_id or system_default, name, type | Income or expense classification |
| Transaction | transaction_id, user_id, category_id, type, amount, date, note, created_at, updated_at | Recorded income or expense |
| Budget | budget_id, user_id, category_id, name, period_start, period_end or period_type, limit_amount, threshold | Spending plan |
| SavingsGoal | goal_id, user_id, name, target_amount, target_date, status | Savings target |
| GoalContribution | contribution_id, goal_id, user_id, amount, date, note | Amount recorded toward a goal |

### 3.4.2 Data Validation Rules

- Amounts must be greater than zero and within the application's configured supported range.
- Transaction type must be income or expense.
- A transaction category must be valid for its transaction type.
- Dates must be valid calendar dates.
- Budget limits and savings targets must be positive.
- A record's user ownership must be validated for every read and write.
- Deleting or changing referenced categories must not silently corrupt historical transactions.
- The application shall document whether savings contributions are tracking-only or are also represented as transactions. The initial design should avoid double-counting contributions as expenses.

## 3.5 Business Rules

**BR-001:** Net balance for a selected period is calculated as total recorded income minus total recorded expenses for that period.

**BR-002:** Budget spending is the sum of eligible expense transactions in the budget's category and date range.

**BR-003:** Remaining budget is the budget limit minus eligible spending. The interface may display a negative remaining amount when spending exceeds the limit.

**BR-004:** Savings goal progress is calculated as recorded contributions divided by the target amount, displayed as a percentage. The displayed progress may be capped at 100%, while the actual contributed amount remains visible.

**BR-005:** The application records user-entered financial information and does not independently verify that the records reflect real bank activity.

**BR-006:** In version 1, a user account has one primary currency. Currency conversion is outside the scope.

# 4. Use Cases

## 4.1 Use Case UC-01: Register an Account

| Field | Description |
|---|---|
| Primary actor | Guest |
| Preconditions | User is not signed in |
| Trigger | User selects “Create account” |
| Main flow | 1. System displays registration form. 2. User enters required details. 3. System validates the details. 4. System creates the account. 5. System confirms registration and directs the user to sign in or the dashboard. |
| Alternate flow | If data is invalid or the account already exists, the system displays an error and allows correction. |
| Postconditions | A new account exists and is associated with the user. |

## 4.2 Use Case UC-02: Add an Expense

| Field | Description |
|---|---|
| Primary actor | Registered user |
| Preconditions | User is signed in |
| Trigger | User selects “Add transaction” |
| Main flow | 1. System displays the transaction form. 2. User selects Expense, enters amount, category, date, and optional note. 3. User submits the form. 4. System validates the input and ownership. 5. System saves the transaction. 6. System confirms success and refreshes relevant totals. |
| Alternate flow | If validation or saving fails, the system displays an error and retains entered values where safe. |
| Postconditions | The expense is stored and included in applicable summaries and budgets. |

## 4.3 Use Case UC-03: Create a Monthly Budget

| Field | Description |
|---|---|
| Primary actor | Registered user |
| Preconditions | User is signed in and has an available expense category |
| Trigger | User selects “Create budget” |
| Main flow | 1. System displays budget form. 2. User enters budget name, category, period, limit, and optional threshold. 3. System validates the values. 4. System saves the budget. 5. System displays the budget status. |
| Alternate flow | If the limit, period, or category is invalid, the system requests correction. |
| Postconditions | The budget is available for monitoring. |

## 4.4 Use Case UC-04: Create a Savings Goal

| Field | Description |
|---|---|
| Primary actor | Registered user |
| Preconditions | User is signed in |
| Trigger | User selects “Add savings goal” |
| Main flow | 1. System displays goal form. 2. User enters goal name, target amount, and optional target date. 3. System validates and saves the goal. 4. System displays the goal and its initial progress. |
| Alternate flow | If required fields are invalid, the system displays validation messages. |
| Postconditions | The savings goal is stored with zero initial contribution unless contributions are entered separately. |

## 4.5 Use Case UC-05: View Financial Report

| Field | Description |
|---|---|
| Primary actor | Registered user |
| Preconditions | User is signed in |
| Trigger | User opens Reports |
| Main flow | 1. System displays report filters. 2. User selects a date range and optional category/type. 3. System retrieves the user's matching transactions. 4. System calculates totals and category breakdowns. 5. System displays the report. |
| Alternate flow | If no records match, the system displays an empty state with guidance. |
| Postconditions | The report is displayed without changing stored financial records. |

## 4.6 Use Case UC-06: Export Transactions

| Field | Description |
|---|---|
| Primary actor | Registered user |
| Preconditions | User is signed in |
| Trigger | User selects “Export CSV” |
| Main flow | 1. User applies filters. 2. User selects export. 3. System verifies ownership and prepares matching records. 4. System generates a CSV file. 5. Browser offers the file to the user. |
| Alternate flow | If no records match or export fails, the system displays an appropriate message. |
| Postconditions | A CSV containing only the user's authorized matching records is generated. |

# 5. Acceptance Criteria and Verification

| ID | Acceptance criterion | Verification method |
|---|---|---|
| AC-01 | A valid user can register and sign in; invalid credentials are rejected. | Functional test |
| AC-02 | A user can add, edit, view, and delete an income or expense transaction. | Functional test |
| AC-03 | Invalid or non-positive transaction amounts are rejected. | Boundary/validation test |
| AC-04 | Dashboard totals match the underlying transactions for the selected period. | Calculation test |
| AC-05 | Budget usage includes only eligible expenses in the defined period and category. | Functional/calculation test |
| AC-06 | Budget warnings appear at the configured threshold and when the limit is exceeded. | Boundary test |
| AC-07 | Savings goal progress updates correctly after a contribution is recorded. | Calculation test |
| AC-08 | A user cannot read or modify another user's records by changing a request identifier. | Authorization/security test |
| AC-09 | CSV export contains only the signed-in user's records matching the selected filters. | Functional/security test |
| AC-10 | The application displays a clear error if saving fails and does not show false success. | Fault-handling test |
| AC-11 | The interface remains usable on the supported desktop and mobile browser sizes. | Usability/responsive test |
| AC-12 | Setup instructions allow a developer to run the application in a clean environment. | Installation test |

# 6. Out of Scope

The following features are excluded from version 1 unless approved as future enhancements:
- Direct bank account linking or automatic transaction synchronization.
- UPI, card, or other payment processing.
- Stock, cryptocurrency, or investment portfolio tracking.
- Personalized investment, tax, or legal advice.
- Credit scoring or loan approval.
- Multi-currency conversion.
- Shared household accounts and collaborative budgets.
- Automated receipt scanning or OCR.
- AI-based financial predictions.

# 7. Future Enhancements

Possible future additions include:
- Receipt image upload and expense extraction.
- Recurring transactions and reminders.
- Multi-currency support with exchange-rate data.
- Optional secure bank data integration through authorized providers.
- Household or shared budgets.
- Personalized spending insights with clear explanations.
- Mobile application or progressive web app support.

# 8. Appendices

## Appendix A: Suggested Navigation Structure

```text
PFMS
├── Authentication
│   ├── Register
│   ├── Sign in
│   └── Password reset
├── Dashboard
├── Transactions
│   ├── Add transaction
│   ├── Transaction history
│   └── Categories
├── Budgets
├── Savings Goals
├── Reports
└── Profile and Settings
```

## Appendix B: Suggested Technology Stack (Non-Mandatory)

One possible student-friendly stack:
- Frontend: HTML, CSS, JavaScript or React
- Backend: Python with Flask or FastAPI
- Database: SQLite for a local prototype; PostgreSQL or another managed database for deployment
- Charts: a JavaScript charting library
- Version control: Git and GitHub

The stack is a suggestion, not a requirement. The team may select alternatives that meet the requirements.

## Appendix C: GitHub Repository Structure

```text
personal-finance-management-system/
├── README.md
├── SRS.md
├── docs/
│   ├── use-cases.md
│   └── diagrams/
├── src/
├── tests/
└── .gitignore
```

## Appendix D: Revision History

| Version | Date | Description |
|---|---|---|
| 1.0 | 29 September 2026 | Initial academic SRS draft |
