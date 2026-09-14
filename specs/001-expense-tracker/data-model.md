# Data Model: Expense Tracker

## Core entity: Expense

| Field | Type | Constraints | Notes |
|---|---|---|---|
| id | integer | primary key, auto-increment | Unique database identifier |
| amount | integer | > 0, integer paise | Stored as integer paise; 1 rupee = 100 paise |
| category | string | required, trimmed, 1-100 chars | User-selected or entered category |
| description | string | optional, trimmed, max 500 chars | Empty string allowed after whitespace normalization |
| expense_date | string | required, ISO date `YYYY-MM-DD` | Calendar date only |
| created_at | string | required, ISO datetime | UTC timestamp |
| updated_at | string | required, ISO datetime | UTC timestamp |

## Derived entity: Expense Summary

| Field | Type | Notes |
|---|---|---|
| total_amount | integer | Sum of current displayed expenses in integer paise |
| expense_count | integer | Number of records after filtering |

## Derived entity: Expense Filter

| Field | Type | Notes |
|---|---|---|
| category | string or null | Optional category match |
| date_from | string or null | Optional start date |
| date_to | string or null | Optional end date |

## Validation rules

- `amount` must be a positive integer number of paise greater than zero; decimal or floating values are rejected.
- `category` must be present after trimming whitespace.
- `expense_date` must be a valid ISO date and must not be empty.
- `description` may be empty but is normalized by trimming whitespace.
- `date_from` and `date_to` must both be valid dates if supplied.
- `date_from` must be less than or equal to `date_to` for a valid range.
- All writes must validate at the backend boundary before persistence.
- The frontend may accept rupees and paise from the user, but it must convert to integer paise before sending requests to the API.

## State transitions

### Create

1. Validate request payload.
2. Normalize category and description.
3. Insert expense row.
4. Return created entity with 201 status.

### Update

1. Confirm the expense ID exists.
2. Validate the updated payload.
3. Update the row atomically.
4. Return updated entity.

### Delete

1. Confirm the expense ID exists.
2. Require explicit confirmation from the client workflow.
3. Delete the row.
4. Return success metadata or 204.

## SQLite schema

```sql
CREATE TABLE expenses (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  amount INTEGER NOT NULL CHECK (amount > 0),
  category TEXT NOT NULL CHECK (length(trim(category)) > 0),
  description TEXT DEFAULT '',
  expense_date TEXT NOT NULL,
  created_at TEXT NOT NULL,
  updated_at TEXT NOT NULL
);

-- amount is stored in integer paise, e.g. ₹125.50 = 12550 paise

CREATE INDEX idx_expenses_category ON expenses(category);
CREATE INDEX idx_expenses_date ON expenses(expense_date);
```

## Notes

- Database writes should remain simple and local; no transaction boundaries beyond a single record create/update/delete are needed.
- The backend is the only place that owns persistence logic.
- Filtering is done in SQL when practical and in the API service layer when a small amount of post-processing is simpler.
- All monetary values are stored and exchanged as integer paise; there is no support for multi-currency records or floating-point persistence.
- Example: a user-facing amount of ₹12.34 is represented internally and over the API as `1234` paise.
