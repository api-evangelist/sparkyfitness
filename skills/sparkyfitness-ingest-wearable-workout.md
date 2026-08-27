---
name: sparkyfitness-ingest-wearable-workout
description: Push a workout session with GPS, heart-rate and lap telemetry into SparkyFitness from a wearable or health platform, safely and idempotently.
api: SparkyFitness API
generated: '2026-08-27'
method: generated
source: https://codewithcj.github.io/SparkyFitness/developer/api-reference + openapi/sparkyfitness-openapi.yml
operations:
  - POST /api/health-data
  - POST /measurements/health-data
  - POST /exercise-entries
  - POST /exercise-entries/import-fit
  - GET /exercise-entries/by-date
  - GET /synced-data/sources
  - DELETE /synced-data/sources/{source}
---

# Ingest a wearable workout into SparkyFitness

This is the highest-risk write surface in the API and the one with the most
traps. Read all of it before posting.

**Endpoint caveat.** The public API reference documents `POST /api/health-data`.
That endpoint is real and is what iOS Shortcuts and the Android app use, but it
is **not in the OpenAPI document** — it is mounted from a directory outside the
project's swagger scan paths. The contract instead documents
`POST /api/measurements/health-data`, a different route in a different file with
the same partial-success envelope. If you generated a client from the spec, you
have the second one. Prefer the documented `POST /api/health-data` and treat the
reference page as authoritative for its body shape.

**Auth.** API key with the `health_data_write` permission, as
`Authorization: Bearer <API_KEY>` or `X-API-Key`. Without the permission you get
`403 Forbidden: API Key does not have health_data_write permission`.

## Steps

1. **Always send `source_id`.** It is the deduplication key and it is what makes
   this operation retry-safe. Re-posting the same `source_id` updates the
   existing entry instead of creating a second one. A record without a
   `source_id` is not deduplicated, and a Nutrition record without one is not
   written at all — it comes back in `skipped[]`.
2. **Send `X-Workout-Model-Version`.** Absent, per-set durations are read as
   **minutes**. `2` or higher means **seconds**. `3` additionally signals that
   the optional telemetry objects may be present. Getting this wrong is a
   60x error in the stored data.
3. **Build the session.** `type: "ExerciseSession"` (or `"Workout"`) with
   `activityType`, `startTime`, `endTime`, `duration` (seconds),
   `caloriesBurned`, `distance`, `source`, `source_id`, and optionally `sets[]`,
   `telemetry`, `gps_points[]`, `hr_samples[]`, `laps[]`.
4. **Watch the distance units.** `distance` on the session is **kilometres**.
   `dist` on a `gps_point` is cumulative **metres**. Same request body, two
   different units.
5. **Downsample before uploading.** The reverse proxy caps bodies at 10 MB.
   Roughly 2000 GPS points and 1200 heart-rate samples keeps a session near
   250 KB.
6. **Send `hr_samples[]` even when you also put `hr` on gps_points** — an indoor
   workout with no GPS otherwise produces no heart-rate chart. Samples are
   **merged** into the day's existing series, not replaced.
7. **Send only the lap windows** in `laps[]` (`lap_index`, `start_time`,
   `end_time`). Per-lap distance, heart rate, speed, cadence, power and
   elevation are computed server-side. Heart-rate **zones** are always derived
   server-side from the series and the profile date of birth; there is no field
   to supply them.
8. **Read the response body, not the status code.** A well-formed request
   returns **200 even when records failed**. `processed[]`, `errors[]` and
   `skipped[]` are always present. A 200 with a non-empty `errors[]` means the
   *remaining* records were saved. Only a malformed body (invalid JSON, or an
   array containing non-objects) returns 400.
   > Older servers returned 400 when any record in a batch failed. If your
   > automation branches on the 400, it is broken against current versions.
9. **Verify.** `GET /exercise-entries/by-date` for the day.

## Undoing an ingest

`DELETE /synced-data/sources/{source}` removes the data synced from one named
source; `GET /synced-data/sources` lists them. Individual entries go with
`DELETE /exercise-entries/{id}`. No time window is stated on either — but
neither is one promised, so do not assume an old entry is still removable
without checking.

## Telemetry field names

`telemetry` keys are `exercise_entries` column names and **unknown keys are
silently ignored** — a typo costs you the value with no error. Known keys:
`avg_heart_rate`, `max_heart_rate`, `avg_speed_mps`, `max_speed_mps`,
`avg_cadence`, `max_cadence`, `avg_power_watts`, `max_power_watts`,
`elevation_gain_meters`, `elevation_loss_meters`, `min_elevation_meters`,
`max_elevation_meters`, `floors_climbed`, `stroke_count`, `moving_time_seconds`,
`elapsed_time_seconds`, `active_calories`, `ground_contact_time_ms`,
`vertical_oscillation_mm`, `stride_length_cm`. Anything you omit is derived from
the series where possible; anything you send is never overwritten.
