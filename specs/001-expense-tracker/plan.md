# Implementation Plan: Expense Tracker

**Branch**: `001-expense-tracker` | **Date**: 2026-09-14 | **Spec**: `/specs/001-expense-tracker/spec.md`

**Input**: Feature specification from `/specs/001-expense-tracker/spec.md`

## Summary

Build a simple local, single-user expense tracker with a React + TypeScript frontend, an Express + TypeScript backend, and a local SQLite database. The application will support adding, viewing, editing, deleting, and filtering expenses in INR, with all business validation enforced at the backend boundary and all persistence handled through the API. Monetary values will be stored and exchanged as integer paise, while the frontend accepts user-facing rupee and paise inputs and converts them to integer paise before sending API requests. The system will favor simplicity and maintainability over multi-user or cloud features, while keeping a clean separation between UI, API, and storage layers.

## Technical Context

**Language/Version**: Node.js 20 LTS, TypeScript 5.x, React 18, Vite 5

**Primary Dependencies**: React, Vite, Tailwind CSS, Express, SQLite driver (`better-sqlite3`), Zod, CORS, Vitest, Supertest

**Storage**: Local SQLite database file stored in the backend project (`data/expenses.db` or equivalent local file)

**Testing**: Vitest for unit and integration tests, Supertest for HTTP API validation, React Testing Library for component-level UI checks

**Target Platform**: Local desktop browsers with a Node.js server running on the same machine

**Project Type**: Web application with separate frontend and backend services

**Performance Goals**: Filter and render a list of 1,000 expenses in under 2 seconds on a typical local machine; API requests under 200 ms for common CRUD operations with a local SQLite DB

**Constraints**: Single-user local app only, no authentication, no cloud dependencies, fixed INR currency, validate all incoming data at backend boundary, no multi-currency support, all persisted and API amounts as integer paise, no floating-point monetary persistence

**Scale/Scope**: Small personal finance tracker with a single expense entity, summary calculations, and responsive UI across mobile and desktop layouts

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

Passes the constitution because the design explicitly separates frontend, backend, and database responsibilities; validates all external inputs at the backend; uses simple, reusable React composition; keeps the scope limited to a single-user local app; documents the contract and setup; and introduces tests for validation, filtering, and persistence behavior. No constitutional violations require justification.

## Project Structure

### Documentation (this feature)

```text
specs/001-expense-tracker/
├── plan.md              # This file
├── research.md          # Phase 0 output
├── data-model.md        # Phase 1 output
├── quickstart.md        # Phase 1 output
├── contracts/
│   └── expense-api.md   # Phase 1 output
└── tasks.md             # Phase 2 output (not created in this stage)
```

### Source Code (repository root)

```text
.
├── apps/
│   ├── web/
│   │   ├── src/
│   │   │   ├── components/
│   │   │   │   ├── ExpenseForm.tsx
│   │   │   │   ├── ExpenseList.tsx
│   │   │   │   ├── FilterBar.tsx
│   │   │   │   ├── SummaryCard.tsx
│   │   │   │   └── EmptyState.tsx
│   │   │   ├── hooks/
│   │   │   │   └── useExpenses.ts
│   │   │   ├── services/
│   │   │   │   └── api.ts
│   │   │   ├── types/
│   │   │   │   └── expense.ts
│   │   │   ├── utils/
│   │   │   │   └── money.ts
│   │   │   ├── App.tsx
│   │   │   ├── main.tsx
│   │   │   └── index.css
│   │   ├── package.json
│   │   ├── vite.config.ts
│   │   ├── tsconfig.json
│   │   ├── tailwind.config.js
│   │   └── index.html
│   └── api/
│       ├── src/
│       │   ├── config/
│       │   │   └── env.ts
│       │   ├── db/
│       │   │   ├── connection.ts
│       │   │   ├── schema.ts
│       │   │   └── repository.ts
│       │   ├── middleware/
│       │   │   └── errorHandler.ts
│       │   ├── routes/
│       │   │   └── expenses.ts
│       │   ├── services/
│       │   │   └── expenseService.ts
│       │   ├── validation/
│       │   │   └── expenseSchema.ts
│       │   ├── app.ts
│       │   └── server.ts
│       ├── tests/
│       │   ├── api/
│       │   │   └── expenses.test.ts
│       │   └── unit/
│       │       └── validation.test.ts
│       ├── data/
│       │   └── .gitkeep
│       ├── package.json
│       ├── tsconfig.json
│       └── vitest.config.ts
├── package.json
├── tsconfig.base.json
├── README.md
└── .gitignore
```

**Structure Decision**: The repository will use a simple monorepo-style layout with separate `apps/web` and `apps/api` projects to preserve clear ownership of UI logic and backend business logic. The frontend owns presentation and REST client concerns only, while the backend owns validation, persistence, and business rules. A root package manifest will manage scripts and shared tooling without introducing a heavier framework or shared-state abstraction.

## Complexity Tracking

No deviations from the constitution require tracking.

