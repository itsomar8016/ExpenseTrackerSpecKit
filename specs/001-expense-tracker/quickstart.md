# Quickstart: Expense Tracker

## Prerequisites

- Node.js 20 LTS
- npm or pnpm
- A local terminal environment

## Install dependencies

From the repository root:

```bash
npm install
```

If the app is split into independent package folders, install each app’s dependencies with the package manager used by the repo.

## Run backend

```bash
cd apps/api
npm install
npm run dev
```

Expected result: the backend starts on a local port such as `3001` and initializes the SQLite database file if it does not already exist.

## Run frontend

```bash
cd apps/web
npm install
npm run dev
```

Expected result: the React app starts on a Vite dev server, usually at `http://localhost:5173`.

## Verify the primary workflow

1. Open the frontend in the browser.
2. Add a valid expense with an amount, category, description, and date.
3. Confirm the record appears in the list.
4. Check the summary reflects the new total and count.
5. Filter by category or date.
6. Edit the record and confirm the updated values persist.
7. Delete the record and confirm it is removed after explicit confirmation.
8. Restart the backend and verify the saved expenses remain available.

## Validation commands

```bash
cd apps/api
npm run test
```

```bash
cd apps/web
npm run test
```

Expected result: business rules, validation, API behavior, and UI flows pass the relevant automated checks.

## Production build

```bash
cd apps/api
npm run build

cd ../web
npm run build
```

Expected result: TypeScript compiles cleanly and the frontend bundle is generated for deployment or local preview.
