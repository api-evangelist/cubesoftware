---
name: Export Cube data
description: Kick off and monitor a data export from a Cube company, optionally on a recurring schedule.
api: openapi/cubesoftware-openapi-original.yml
operations: [cube_export_create, cube_export_list, cube_export_schedule_create, cube_export_schedule_list]
---

# Export Cube data

Use this skill to pull financial data out of a Cube company via the Cube API.

## Prerequisites
- An OAuth 2.0 access token (authorization code + PKCE `S256`) with the `read` scope. See `authentication/cubesoftware-authentication.yml`.
- The target company id. List the caller's companies with `GET /user/companies` (`user_companies_retrieve`) and pass it as the `X-Company-ID` header on every request.

## Steps
1. **Begin an export** — `POST /cube/export` (`cube_export_create`). Include the `X-Company-ID` header. The response identifies the export job.
2. **Poll for completion** — `GET /cube/export` (`cube_export_list`) to see export jobs and their state; wait until the target export is ready before downloading.
3. **(Optional) Schedule recurring exports** — `POST /cube/export/schedule` (`cube_export_schedule_create`); review existing schedules with `GET /cube/export/schedule` (`cube_export_schedule_list`).

## Rules
- Always send `X-Company-ID` (required on virtually all operations — see `conventions/cubesoftware-conventions.yml`).
- Writes are **not** idempotent; do not blindly retry `cube_export_create` on timeout — check `cube_export_list` first.
- Handle `401` (token invalid/expired — refresh), `403` (missing scope / wrong company), and `404` per `errors/cubesoftware-problem-types.yml`.
