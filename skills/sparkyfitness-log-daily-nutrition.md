---
name: sparkyfitness-log-daily-nutrition
description: Log food and water to a SparkyFitness diary for a given day, and read back the day's nutrition totals.
api: SparkyFitness API
generated: '2026-08-27'
method: generated
source: openapi/sparkyfitness-openapi.yml + conventions/sparkyfitness-conventions.yml
operations:
  - GET /food-entries/by-date/{date}
  - GET /food-entries/nutrition/today
  - GET /foods/foods-paginated
  - GET /foods/barcode/{barcode}
  - POST /foods
  - POST /food-entries
  - PUT /food-entries/{id}
  - DELETE /food-entries/{id}
  - POST /food-entries/copy-yesterday
  - GET /meal-types
mcp_equivalents:
  - sparky_search_foods
  - sparky_manage_food
  - sparky_get_food_diary
  - sparky_get_nutrition_summary
---

# Log daily nutrition in SparkyFitness

SparkyFitness is self-hosted. Your base URL is the operator's own host plus `/api`
(`https://{host}/api`). Authenticate every call with an API key generated in
**Settings -> Developer & Integrations -> API Key Management**, sent as
`Authorization: Bearer <API_KEY>` (`x-api-key: <API_KEY>` also works).

If an MCP client is available, prefer the MCP server at `POST /mcp` on the same
host — its tools honour the user's unit preferences automatically, which the
REST API does not.

## Steps

1. **Read the day first.** `GET /food-entries/by-date/{date}` with `date` as
   `YYYY-MM-DD`. Never log before reading: there is no idempotency key on
   `POST /food-entries`, so a retry after an ambiguous failure creates a
   duplicate row.
2. **Find the food.** Try, in order:
   - `GET /foods/barcode/{barcode}` when you have a barcode;
   - `GET /foods/foods-paginated` with the search parameters to check the user's
     own catalog (returns `{foods: [...], totalCount: n}` — page with
     `limit`/`offset`);
   - `GET /foods/fatsecret/search` or the other provider search operations only
     if the instance has that provider configured.
3. **Create the food only if it is genuinely new.** `POST /foods`. Prefer an
   existing catalog row — a duplicate food pollutes search and favorites for
   every future day.
4. **Log the entry.** `POST /food-entries` referencing the food id, the variant,
   the quantity and the meal type (`GET /meal-types` for the valid set).
5. **Confirm.** `GET /food-entries/nutrition/today`, or
   `GET /food-entries/range/{startDate}/{endDate}` for a window.

## Repeating yesterday

`POST /food-entries/copy-yesterday` copies yesterday's diary to today, and
`POST /food-entries/copy-all-yesterday` copies everything. These are cheaper and
safer than re-logging item by item.

## Rules

- **Units are canonical, not the user's.** The REST API takes and returns
  kilograms, centimetres and millilitres regardless of what the user's display
  preferences say. Only the MCP tools convert.
- **Correcting a mistake:** `PUT /food-entries/{id}` to change it,
  `DELETE /food-entries/{id}` to remove it. Both are immediate and no window
  applies.
- **Do not delete a catalog food to undo a diary entry.** Delete the entry.
  If you must remove a catalog food, call `GET /foods/{id}/deletion-impact`
  first — diary entries reference catalog rows and the delete gives no warning.
- **Errors** are `{"error": "message"}` with no code. 401 = bad or inactive key,
  403 = the key lacks the permission, 404 = absent *or* owned by another user
  (row-level security makes those indistinguishable). See
  `errors/sparkyfitness-problem-types.yml`.
- **429** is not declared on any operation but is real: 100 requests per minute
  per API key by default. Honour `Retry-After`.
