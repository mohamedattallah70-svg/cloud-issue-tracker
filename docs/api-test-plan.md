# API Test Plan — Issues API

Written before implementation. In Lab 4 these cases become the acceptance checklist.

| Test ID | Type | Request | Expected Result | Contract rule |
|---|---|---|---|---|
| T01 | Positive | `GET /api/issues` | 200; JSON array returned. | Contract 1 |
| T02 | Positive | `POST /api/issues` with valid title, description, priority | 201; complete created issue returned with server-generated id, status `Open` and createdAt. | Contract 2 |
| T03 | Negative | `POST /api/issues` missing title | 400; `VALIDATION_ERROR`. | title required |
| T04 | Negative | `POST /api/issues` with priority = `Critical` | 400; `VALIDATION_ERROR`. | priority enum |
| T05 | Positive | `PATCH /api/issues/{id}/status` on an existing issue with status = `Resolved` | 200; returned status is `Resolved`. | Contract 3 |
| T06 | Negative | `PATCH /api/issues/{id}/status` with status = `Deleted` | 400; `VALIDATION_ERROR`. | status enum |
| T07 | Negative | `PATCH /api/issues/UNKNOWN-ID/status` | 404; `ISSUE_NOT_FOUND`. | id must exist |
| T08 | Positive | `GET /api/issues` when no issues exist | 200; empty JSON array `[]`. | Empty is not an error |
| T09 | Negative | `POST /api/issues` including `id`, `status: "Resolved"`, `createdAt` | 201; server ignores those fields — returned issue has a server id, status `Open` and server createdAt. | Server-owned fields |
| T10 | Negative | `POST /api/issues` with title `"Hi"` (2 chars) | 400; `VALIDATION_ERROR`. | title 3–100 chars |
| T11 | Negative | `POST /api/issues` with body sent as plain text (no `Content-Type: application/json`) | 415; `UNSUPPORTED_MEDIA_TYPE`. | JSON required |
