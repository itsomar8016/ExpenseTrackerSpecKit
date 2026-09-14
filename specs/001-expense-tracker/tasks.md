# Tasks: Expense Tracker

**Input**: Design documents from `/specs/001-expense-tracker/`

**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md, contracts/

**Tests**: Automated API and frontend validation tests are required by the feature specification and constitution.

**Organization**: Tasks are grouped by user story so each story can be implemented and tested independently.

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Initialize the monorepo, frontend, backend, and baseline tooling.

- [ ] T001 Create the project structure for the frontend and backend under `apps/web` and `apps/api`, including the shared root manifest and common TypeScript setup in `package.json`, `tsconfig.base.json`, and `.gitignore`
- [ ] T002 Initialize the React + Vite + TypeScript + Tailwind frontend in `apps/web/package.json`, `apps/web/vite.config.ts`, `apps/web/tailwind.config.js`, and `apps/web/index.html`
- [ ] T003 Initialize the Node.js + Express + TypeScript backend in `apps/api/package.json`, `apps/api/tsconfig.json`, `apps/api/src/app.ts`, and `apps/api/src/server.ts`
- [ ] T004 [P] Configure shared linting, formatting, and test scripts in the root `package.json` and the package-level configs for `apps/web` and `apps/api`
- [ ] T005 [P] Create the local SQLite bootstrap and environment configuration in `apps/api/src/config/env.ts`, `apps/api/src/db/connection.ts`, and `apps/api/data/.gitkeep`

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Establish the core backend contracts and frontend money conversion foundation before story delivery.

**Critical**: No user story work can begin until this phase is complete.

- [ ] T006 Create the SQLite schema for the `expenses` table in `apps/api/src/db/schema.ts` with `amount INTEGER NOT NULL CHECK (amount > 0)` and an explicit comment documenting integer paise storage
- [ ] T007 Implement repository patterns and data access for create, read, update, and delete in `apps/api/src/db/repository.ts`
- [ ] T008 Implement the backend validation schema in `apps/api/src/validation/expenseSchema.ts` so `amount` must be a positive integer number of paise greater than zero, with no floating-point values accepted
- [ ] T009 Create the structured error handling layer in `apps/api/src/middleware/errorHandler.ts` and ensure validation failures return actionable 400 responses
- [ ] T010 [P] Create a shared frontend money conversion utility in `apps/web/src/utils/money.ts` that accepts user-visible INR values and converts to integer paise before API calls
- [ ] T011 [P] Define the frontend expense type and API contract models in `apps/web/src/types/expense.ts` and `apps/web/src/services/api.ts` using integer paise for all API payloads
- [ ] T012 Create the route and service structure for expense operations in `apps/api/src/routes/expenses.ts` and `apps/api/src/services/expenseService.ts`

**Checkpoint**: Foundation ready - user story implementation can now begin in parallel.

---

## Phase 3: User Story 1 - Record an Expense (Priority: P1) 🎯 MVP

**Goal**: Save a valid expense with amount, category, description, and date, while rejecting invalid inputs with clear backend validation.

**Independent Test**: Add a valid expense through the form, save it, and confirm it appears in the list with the entered details.

### Tests for User Story 1

- [ ] T013 [P] [US1] Write failing API contract tests for successful and invalid `POST /expenses` requests in `apps/api/tests/api/expenses.test.ts`
- [ ] T014 [P] [US1] Write a failing frontend form validation test for a valid expense submission and an invalid zero/negative amount in `apps/web/src/components/ExpenseForm.test.tsx`

### Implementation for User Story 1

- [ ] T015 [US1] Implement the backend `POST /expenses` create flow in `apps/api/src/routes/expenses.ts` and `apps/api/src/services/expenseService.ts` using integer paise values and validation errors for missing or invalid fields
- [ ] T016 [US1] Enforce `amount > 0` as an integer number of paise in the validation schema and repository path in `apps/api/src/validation/expenseSchema.ts` and `apps/api/src/db/repository.ts`
- [ ] T017 [US1] Build the expense form UI and submission workflow in `apps/web/src/components/ExpenseForm.tsx` with accessible labels, inline validation, and paise-to-INR conversion handling for user display
- [ ] T018 [US1] Wire the form to the API client in `apps/web/src/services/api.ts` so it converts rupees and paise to integer paise before sending requests
- [ ] T019 [US1] Add success and error feedback handling in `apps/web/src/components/ExpenseForm.tsx` and `apps/web/src/App.tsx` so create failures leave existing data unchanged and show actionable errors

**Checkpoint**: At this point, User Story 1 should be fully functional and independently testable.

---

## Phase 4: User Story 2 - Review Expense History (Priority: P1)

**Goal**: Display all saved expenses and summary totals with an empty state when no entries exist.

**Independent Test**: Load the app with seeded expenses and verify the list, count, and total match the saved data.

### Tests for User Story 2

- [ ] T020 [P] [US2] Write failing API tests for `GET /expenses` and summary totals in `apps/api/tests/api/expenses.test.ts`
- [ ] T021 [P] [US2] Write a failing frontend test for rendering a non-empty list, summary values, and the empty state in `apps/web/src/components/ExpenseList.test.tsx` and `apps/web/src/App.test.tsx`

### Implementation for User Story 2

- [ ] T022 [US2] Implement the backend read and summary logic for `GET /expenses` in `apps/api/src/services/expenseService.ts` and `apps/api/src/routes/expenses.ts`, returning `expenses` plus `summary.total_amount` and `summary.expense_count` in integer paise
- [ ] T023 [US2] Build the expense list and summary components in `apps/web/src/components/ExpenseList.tsx` and `apps/web/src/components/SummaryCard.tsx` using INR display formatting from paise values
- [ ] T024 [US2] Implement the initial fetch and empty-state behavior in `apps/web/src/hooks/useExpenses.ts` and `apps/web/src/App.tsx` so loading and no-data flows are clear and accessible
- [ ] T025 [US2] Ensure the list shows `amount`, `category`, `description`, and `expense_date` values consistently with the API contract in `apps/web/src/types/expense.ts`

**Checkpoint**: At this point, User Stories 1 and 2 should both work independently.

---

## Phase 5: User Story 3 - Maintain and Filter Expenses (Priority: P2)

**Goal**: Support update, deletion, and filtering by category and date range while preserving accurate summaries.

**Independent Test**: Edit an expense, confirm delete behavior, and apply category/date filters to verify the list and totals update correctly.

### Tests for User Story 3

- [ ] T026 [P] [US3] Write failing API tests for `PUT /expenses/:id`, `DELETE /expenses/:id`, and date/category filtering in `apps/api/tests/api/expenses.test.ts`
- [ ] T027 [P] [US3] Write failing frontend tests for edit, delete confirmation, filter clearing, and empty-result states in `apps/web/src/components/FilterBar.test.tsx` and `apps/web/src/App.test.tsx`

### Implementation for User Story 3

- [ ] T028 [US3] Implement update and delete routes and repository actions in `apps/api/src/routes/expenses.ts`, `apps/api/src/services/expenseService.ts`, and `apps/api/src/db/repository.ts` with validation for positive integer paise values and explicit deletion confirmation requirements
- [ ] T029 [US3] Add filter support for category and date-range queries in `apps/api/src/services/expenseService.ts` and `apps/api/src/db/repository.ts`, ensuring validation rejects invalid ranges and totals are recalculated from the filtered result set
- [ ] T030 [US3] Build the filter controls and row actions in `apps/web/src/components/FilterBar.tsx`, `apps/web/src/components/ExpenseList.tsx`, and `apps/web/src/App.tsx` so category and date filters and delete confirmation behave predictably
- [ ] T031 [US3] Add edit-mode handling and save/cancel flows in `apps/web/src/components/ExpenseForm.tsx` and `apps/web/src/components/ExpenseList.tsx` with paise conversion before submission and accessible keyboard support
- [ ] T032 [US3] Ensure empty-result states and summary totals reflect the filtered list in `apps/web/src/components/EmptyState.tsx`, `apps/web/src/components/SummaryCard.tsx`, and `apps/web/src/hooks/useExpenses.ts`

**Checkpoint**: All user stories should now be independently functional.

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Tighten the end-to-end quality bar across the full app.

- [ ] T033 [P] Review and update the documentation in `README.md` and `specs/001-expense-tracker/quickstart.md` to match the paise-based amount model and startup flow
- [ ] T034 [P] Run the backend test suite in `apps/api` for validation, CRUD behavior, summary calculations, and persistence boundaries
- [ ] T035 [P] Run the frontend test suite and production build in `apps/web` for form validation, filtering, empty states, and accessible interactions
- [ ] T036 Perform a final cross-check against the feature specification in `specs/001-expense-tracker/spec.md` and the constitution in `.specify/memory/constitution.md` to confirm all acceptance criteria and architecture constraints are satisfied

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies - can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion and blocks all story implementation
- **User Story 1 (Phase 3)**: Depends on Foundational completion
- **User Story 2 (Phase 4)**: Depends on Foundational completion and may build on Story 1 output
- **User Story 3 (Phase 5)**: Depends on Foundational completion and may build on Stories 1 and 2
- **Polish (Phase 6)**: Depends on all desired stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: Can start after Foundational completion and should be delivered as the MVP
- **User Story 2 (P1)**: Can start in parallel with Story 1 once the foundation is ready
- **User Story 3 (P2)**: Can start after Foundational completion and should remain independently testable

### Parallel Opportunities

- Setup tasks `T004` and `T005` can be worked in parallel
- Foundational tasks `T010`, `T011`, and `T012` can be implemented in parallel after Setup completes
- Story tests for each user story can be written in parallel before implementation
- Story-specific model and UI work can proceed in parallel once the shared foundational pieces are ready

---

## Parallel Example: Story Delivery

```bash
# User Story 1
Task: "Write failing API contract tests for POST /expenses in apps/api/tests/api/expenses.test.ts"
Task: "Write failing frontend form validation test in apps/web/src/components/ExpenseForm.test.tsx"

# User Story 2
Task: "Write failing API tests for GET /expenses list and summary in apps/api/tests/api/expenses.test.ts"
Task: "Write a failing frontend test for rendering a non-empty list and empty state in apps/web/src/App.test.tsx"

# User Story 3
Task: "Write failing API tests for filtering, update, and delete behavior in apps/api/tests/api/expenses.test.ts"
Task: "Write failing frontend tests for edit, delete confirmation, and filter behavior in apps/web/src/App.test.tsx"
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Setup
2. Complete Phase 2: Foundational
3. Deliver Phase 3: User Story 1
4. Validate the story independently with the API and form tests
5. Stop and confirm the functional core before extending to the history and maintenance flows

### Incremental Delivery

1. Setup + Foundational create the shared architecture and money conversion rules
2. Add Story 1 to enable creating expenses and validating input
3. Add Story 2 to view history, summary totals, and empty states
4. Add Story 3 to support filtering, editing, and deleting records
5. Finish with cross-cutting polish, documentation, and validation

### Parallel Team Strategy

With multiple developers:

1. Team completes Setup + Foundational together
2. Developer A focuses on Story 1
3. Developer B focuses on Story 2
4. Developer C focuses on Story 3
5. Final polish and validation runs after the story work is complete

---

## Notes

- All tasks follow the required checklist format: checkbox, task ID, optional `[P]`, required story labels for user story phases, and explicit file paths
- The `amount` field is treated consistently as integer paise across all tasks; no floating-point persistence or multi-currency support is allowed
- Every user story is independently completable and testable
- Tests are written before implementation for each story where applicable, in line with the feature requirements and TDD expectations
