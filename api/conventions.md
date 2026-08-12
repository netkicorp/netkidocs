# API Conventions

Cross-cutting rules shared by every Netki API endpoint. Individual endpoint
docs link here instead of repeating this material.

## Base URL

All endpoints are served from:

`https://kyc.myverify.info`

Every path in these docs is relative to this base URL.

## Authentication

The API uses JWT bearer tokens. Obtain a token from `/api/token-auth/` and
send it on every request:

`Authorization: Bearer <access-token>`

Access tokens expire; refresh them at `/api/token-refresh/`. See
[Authentication](./authentication.md) for the full flow.

## Pagination

List endpoints return a paginated envelope:

```json
{
  "count": 1,
  "next": null,
  "previous": null,
  "results": []
}
```

- `count` — total number of records.
- `next` / `previous` — absolute URLs for the adjacent pages, or `null`.
- `results` — the array of records for the current page.

## Timestamps and identifiers

- Timestamps are ISO-8601 in UTC, e.g. `2026-07-14T18:33:17.657524Z`.
- Resource identifiers are UUIDs, e.g. `604e1738-4716-4bdd-867b-4942186b1e1c`.

## Errors

Errors return a non-2xx HTTP status with a JSON body describing the problem.
See [API Error Codes](./api_error_codes.md) for the full registry.

## Content type

Send `Content-Type: application/json; charset=utf-8` on all request bodies.
