# Research: Personal Expense Tracker

## Decision: Use a simple 2-tier web app with a local SQLite database

The application will be implemented as a small local web app with two clear runtime boundaries:

- Frontend: React + TypeScript + Vite + Tailwind CSS
- Backend: Node.js + Express + TypeScript
- Storage: SQLite file on the local machine

This matches the requirement for a single-user, local expense tracker without authentication, while keeping the architecture approachable and maintainable.

## Rationale

- The product scope is intentionally limited to expense tracking and summary reporting.
- A REST API between the frontend and backend preserves separation of concerns and matches the requirement for business logic and persistence to live on the backend.
- SQLite is appropriate because the app is local-only, data volumes are modest, and persistence must survive application restarts without cloud infrastructure.
- React and Vite provide a lightweight modern frontend without unnecessary complexity.
- Express with TypeScript keeps the backend simple while allowing structured validation, routes, and data access layers.

## Alternatives considered

### 1) Single fullstack app with direct DB access in the frontend

Rejected because it violates the constitution’s layered architecture requirement and mixes presentation, business rules, and persistence responsibilities.

### 2) PostgreSQL or other server-based database

Rejected because the app is local-only, should avoid infrastructure setup, and requires no multi-user or networked deployment.

### 3) Framework-heavy stack (Next.js, NestJS, Prisma)

Rejected because they add significant setup and abstractions for a small single-user product. The project favors a maintainable but minimalist architecture.

### 4) Client-side-only storage in browser localStorage

Rejected because it makes validation and persistence logic harder to enforce consistently, does not provide a server boundary, and reduces the quality of business-rule testing and data integrity controls.

## Key technical decisions

### Currency and amount handling

- All expenses use INR (₹) as a single fixed currency.
- The database amount column stores integer paise, and the API request/response amount field uses integer paise as well.
- The frontend accepts user-facing INR values in rupees and paise, then converts them to integer paise before sending API requests.
- Monetary values will be stored as integer paise to avoid floating-point precision issues; no multi-currency support and no floating-point monetary persistence are introduced.
- Example: ₹125.50 is represented as `12550` paise in both the database and API payloads.

### Validation strategy

- Frontend validation provides immediate user feedback.
- Backend validation is the source of truth.
- Validation will reject missing fields, invalid amounts, invalid dates, blank text values after trim, and invalid date ranges.

### Data access approach

- Use a repository layer for SQLite access.
- Keep SQL statements centralized in a `db/repository.ts` or similar module.
- Use parameterized queries to prevent SQL injection and protect local data.

### Error handling

- API endpoints return structured error payloads with HTTP status codes.
- Validation failures return 400 responses.
- Not-found errors return 404.
- Unexpected server failures return 500 with a safe message.

### Testing strategy

- Unit tests for validation and summaries
- API tests for CRUD, validation, and persistence boundaries
- Frontend tests for user-visible behavior, empty states, and filtered summary updates

## Open concerns resolved

- Currency: fixed to INR and not configurable in the initial release.
- Auth: out of scope for the initial version.
- Deployment: local developer environment only.
- Scope: no budgets, recurring expenses, or income tracking.
