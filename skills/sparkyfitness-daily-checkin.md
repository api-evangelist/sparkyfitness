---
name: sparkyfitness-daily-checkin
description: Record and read a SparkyFitness daily check-in — weight, body measurements, mood, sleep and custom metrics — using the upsert semantics correctly.
api: SparkyFitness API
generated: '2026-08-27'
method: generated
source: https://codewithcj.github.io/SparkyFitness/developer/api-reference + openapi/sparkyfitness-openapi.yml
operations:
  - POST /measurements/check-in
  - GET /measurements/check-in/{date}
  - PUT /measurements/check-in/{id}
  - DELETE /measurements/check-in/{id}
  - GET /measurements/check-in/latest-on-or-before-date
  - GET /measurements/check-in-measurements-range/{startDate}/{endDate}
  - POST /measurements/custom-entries
  - GET /measurements/custom-categories
  - POST /mood
  - GET /mood/date/{entryDate}
  - POST /sleep/manual_entry
mcp_equivalents:
  - sparky_manage_checkin
  - sparky_daily_checkin_wizard
---

# Record a SparkyFitness daily check-in

One check-in per user per day. `POST /measurements/check-in` is an **upsert**
keyed on `entry_date`, which makes it the one write in this API you can retry
freely.

## Steps

1. **Read the day.** `GET /measurements/check-in/{date}` (`YYYY-MM-DD`). It
   returns `{}` when there is no entry — that is a normal result, not an error.
2. **Upsert.** `POST /measurements/check-in` with `entry_date` plus any of
   `weight`, `height`, `body_fat_percentage`, `neck`, `waist`, `hips`, `steps`,
   `muscle_mass_kg`, `bone_mass_kg`, `body_water_percentage`.
3. **Add anything the fixed fields do not cover** with
   `POST /measurements/custom-entries`. A custom category is created
   automatically the first time you use a name that does not exist
   (`GET /measurements/custom-categories` lists the existing ones), so check
   before inventing a name — "Blood Pressure Systolic" and "BP Systolic" will
   become two independent series that never merge.
4. **Mood and sleep are separate resources**, not check-in fields:
   `POST /mood` and `POST /sleep/manual_entry`.
5. **Trend.** `GET /measurements/check-in-measurements-range/{startDate}/{endDate}`,
   or `GET /measurements/check-in/latest-on-or-before-date` for the most recent
   value at or before a date — the right call for "what did they weigh when they
   started".

## Rules

- **Units are fixed and canonical.** Masses in kilograms, circumferences and
  height in centimetres, percentages as percentages. The REST API does not
  convert to the user's display preference; the MCP tools do.
- **BMI is neither accepted nor stored.** It is derived from weight and height
  wherever it is shown. Do not try to send it.
- **`null` clears, it does not skip.** Sending `null` for a field erases a value
  previously recorded for that day. If you only mean to add weight, send only
  `entry_date` and `weight` — do not send the other fields as `null`.
- **Reversal:** `DELETE /measurements/check-in/{id}`, or simply upsert the day
  again. No window is documented for either.
- **Retry safety:** the upsert makes this safe. `POST /mood` and
  `POST /sleep/manual_entry` are ordinary creates and are **not** — read before
  retrying those.
