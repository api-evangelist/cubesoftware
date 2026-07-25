---
name: Upload data into a Cube data table
description: Load data into a Cube data table using the multipart upload + import flow.
api: openapi/cubesoftware-openapi-original.yml
operations: [retrieve_3, data_tables_upload_create, data_tables_upload_part_create, data_tables_upload_import_create]
---

# Upload data into a Cube data table

Use this skill to push a dataset into an existing Cube data table.

## Prerequisites
- OAuth 2.0 access token with the `write` scope (`authentication/cubesoftware-authentication.yml`).
- The `X-Company-ID` header for the target company on every request.
- A `data_table_id`. Confirm the table with `GET /data-tables/{data_table_id}` (`retrieve_3`).

## Steps
1. **Create an upload** — `POST /data-tables/{data_table_id}/upload` (`data_tables_upload_create`). Returns a `data_table_upload_id`.
2. **Upload the data in parts** — `POST /data-tables/{data_table_id}/upload/{data_table_upload_id}/part` (`data_tables_upload_part_create`) for each chunk.
3. **Start the import** — `POST /data-tables/{data_table_id}/upload/{data_table_upload_id}/import` (`data_tables_upload_import_create`) to commit the uploaded data into the table.

## Rules
- Send `X-Company-ID` on every call.
- Sequence matters: create → part(s) → import. Do not call import before all parts are uploaded.
- On `400` (validation) inspect the message body; on `409` a conflicting upload may already be in progress. See `errors/cubesoftware-problem-types.yml`.
