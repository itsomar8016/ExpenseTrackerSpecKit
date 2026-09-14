<!--
Sync Impact Report
- Version change: UNVERSIONED -> 1.0.0
- Modified principles: scaffold placeholders replaced with nine project principles
- Added sections: Additional Constraints, Development Workflow
- Removed sections: none
- Follow-up TODOs: confirm the original ratification date
-->

# Expense Tracker Constitution

## Core Principles

### I. Simplicity and Maintainability
Implementation MUST use the simplest design that satisfies the approved requirements. Code MUST
have clear ownership, cohesive responsibilities, and names that communicate intent. Abstractions
MUST remove demonstrated duplication or complexity; speculative abstractions are prohibited.
The rationale is to keep the expense tracker understandable and changeable as it grows.

### II. Layered Architecture
Frontend, backend, and database responsibilities MUST remain explicitly separated. The frontend
MUST communicate through documented backend contracts, and database access MUST be owned by the
backend or its data-access layer. UI code MUST NOT contain direct database access or business
rules that belong to the backend. This preserves replaceability and makes failures diagnosable.

### III. Reusable React Components
React components MUST have one clear responsibility and MUST expose reusable behavior through
well-defined props or composition. Shared components MUST be used for repeated interaction and
presentation patterns. Components MUST NOT hide unrelated business state or create unnecessary
coupling between screens. This keeps feature work consistent and reduces maintenance cost.

### IV. Responsive and Accessible UI
User-facing workflows MUST remain usable across supported viewport sizes and input methods.
Interactive controls MUST have accessible names, keyboard operation, visible focus states, and
appropriate semantic structure. Layouts MUST reflow without loss of content or functionality.
Accessibility and responsive behavior are acceptance criteria, not post-release polish.

### V. Input Validation
All external input MUST be validated at the backend boundary before it reaches business logic or
persistence. Validation MUST cover required fields, types, ranges, formats, and cross-field
constraints where applicable. The frontend MAY provide immediate feedback, but frontend checks
MUST NOT replace backend validation. Errors MUST be safe, actionable, and consistent.

### VI. Data Integrity
Business invariants MUST be enforced by the backend and, where supported, by database constraints.
Writes that must succeed or fail together MUST be transactional. Monetary values MUST use an
exact representation appropriate to the chosen database and API contracts; floating-point
arithmetic MUST NOT be used for persisted currency values. Invalid or partial records MUST NOT be
silently accepted.

### VII. Automated Business-Logic Testing
Important business rules, calculations, validation behavior, and persistence boundaries MUST have
automated tests. Tests MUST be deterministic, isolated, and readable. A change that alters a
business rule MUST add or update tests that demonstrate the intended behavior before review.
The test suite is a required quality gate for merging changes.

### VIII. Minimal, Justified Dependencies
New runtime or build dependencies MUST have a documented need, a maintained release history, a
compatible license, and a clear benefit over the platform or existing project capabilities.
Dependencies MUST be kept current within the project support policy, and unused dependencies
MUST be removed. This limits security, licensing, and maintenance risk.

### IX. Clear Documentation
Public APIs, non-obvious business rules, setup steps, and operational workflows MUST be documented
near the code or in the project documentation. Documentation MUST be updated in the same change
as the behavior it describes. Examples MUST be kept consistent with current contracts so that
documentation remains usable as an engineering reference.

## Additional Constraints

The application MUST protect personal and financial data through least-privilege access,
appropriate secret handling, and error messages that do not disclose sensitive implementation or
user data. API contracts and persisted schemas MUST be changed deliberately, with migration or
compatibility handling documented when existing data or clients are affected.

## Development Workflow

Every change MUST identify affected layers, update relevant automated tests, and document any
contract or data migration impact. Reviews MUST verify constitution compliance, validation,
data-integrity behavior, accessibility, and dependency justification when applicable. Changes
MUST pass the project's formatting, static-analysis, and test checks before integration.

## Governance
<!-- Example: Constitution supersedes all other practices; Amendments require documentation, approval, migration plan -->

This constitution governs project decisions and supersedes conflicting informal practices.
Amendments MUST be proposed as a documented change to this file, include a Sync Impact Report,
state the reason and affected principles, and be reviewed before adoption. Any amendment that
changes a mandatory rule MUST include the migration or rollout work needed to bring existing code
into compliance.

Versioning follows semantic versioning: MAJOR for backward-incompatible governance changes,
MINOR for new principles or materially expanded requirements, and PATCH for clarifications or
non-semantic corrections. Compliance MUST be reviewed during every change review and during
periodic project health checks; violations MUST be recorded with an owner and remediation plan.

**Version**: 1.0.0 | **Ratified**: 2026-09-14: confirm original adoption date | **Last Amended**: 2026-09-14
