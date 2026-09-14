# Expense API Contract

## Base URL

`/api`

## Monetary representation

All persisted and API monetary amounts use integer paise.

- `1 rupee = 100 paise`
- The database `amount` column stores integer paise.
- The API `amount` field for requests and responses is also integer paise.
- The frontend may accept and display INR values in rupees and paise to the user, but it must convert to integer paise before sending API requests.
- No multi-currency support and no floating-point monetary persistence are part of this feature.

Example: `₹125.50` is represented as `12550` paise.

## Endpoints

### GET /expenses

Returns the currently visible expense list and summary. Query parameters may include category, date_from, and date_to.

#### Query parameters

- `category` (optional): category filter value
- `date_from` (optional): ISO date `YYYY-MM-DD`
- `date_to` (optional): ISO date `YYYY-MM-DD`

#### Success response

```json
{
  "expenses": [
    {
      "id": 1,
      "amount": 12550,
      "category": "Food",
      "description": "Groceries",
      "expense_date": "2026-09-10",
      "created_at": "2026-09-10T12:00:00.000Z",
      "updated_at": "2026-09-10T12:00:00.000Z"
    }
  ],
  "summary": {
    "total_amount": 12550,
    "expense_count": 1
  }
}
```

#### Error response

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Amount must be a positive integer number of paise greater than zero."
  }
}
```

### POST /expenses

Creates a new expense record.

#### Request body

```json
{
  "amount": 12550,
  "category": "Food",
  "description": "Groceries",
  "expense_date": "2026-09-10"
}
```

`amount` is the integer paise value for the expense. Example: `₹125.50` is sent as `12550`.

#### Success response

- Status: `201 Created`
- Body: the created expense object

#### Validation failure

- Status: `400 Bad Request`
- Body: structured validation error message

### PUT /expenses/:id

Updates an existing expense.

#### Request body

```json
{
  "amount": 16000,
  "category": "Food",
  "description": "Pantry refill",
  "expense_date": "2026-09-12"
}
```

`amount` remains an integer paise value; `₹160.00` is represented as `16000`.

#### Success response

- Status: `200 OK`
- Body: updated expense object

#### Not found response

- Status: `404 Not Found`

### DELETE /expenses/:id

Deletes an expense after client confirmation.

#### Success response

- Status: `204 No Content`

#### Not found response

- Status: `404 Not Found`

## HTTP status codes

- `200 OK` for successful reads and updates
- `201 Created` for successful create
- `204 No Content` for successful delete
- `400 Bad Request` for validation and request-shape issues
- `404 Not Found` if the expense does not exist
- `500 Internal Server Error` for unexpected server errors

## Notes

- All API payloads use JSON.
- The backend is responsible for validating input and enforcing the data model.
- The frontend should treat all non-2xx responses as user-visible error conditions.
- The `amount` field is never a floating-point value; it is always an integer number of paise.
