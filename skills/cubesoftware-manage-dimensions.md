---
name: Manage Cube dimensions
description: List, create, read, and update the dimensions that structure a Cube company's model.
api: openapi/cubesoftware-openapi-original.yml
operations: [DimensionList, DimensionCreate, DimensionRetrieve, DimensionUpdate, dimensions_status_create]
---

# Manage Cube dimensions

Use this skill to work with the dimensions that shape a Cube company's financial model.

## Prerequisites
- OAuth 2.0 access token (`read` for reads, `write` for changes) — `authentication/cubesoftware-authentication.yml`.
- `X-Company-ID` header for the target company on every request.

## Steps
1. **List dimensions** — `GET /dimensions` (`DimensionList`).
2. **Create a dimension** — `POST /dimensions` (`DimensionCreate`).
3. **Read one** — `GET /dimensions/{dimension_id}` (`DimensionRetrieve`).
4. **Update one** — `POST /dimensions/{dimension_id}` (`DimensionUpdate`).
5. **Enable / disable** — `POST /dimensions/{dimension_id}/status` (`dimensions_status_create`).

## Rules
- Send `X-Company-ID` on every call.
- Writes are not idempotent — read current state (`DimensionRetrieve`) before re-issuing an update.
- Map error codes with `errors/cubesoftware-problem-types.yml`; respect OAuth scopes in `scopes/cubesoftware-scopes.yml`.
