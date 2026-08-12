# API Error Codes

Netki attaches structured error codes to the objects in your verification
flow — a transaction, an identity, an uploaded document, or an
accredited-investor status — whenever something needs your attention or
theirs. Each code is a small record with a stable name, a category, and a
human-readable description. This page documents the endpoint that returns
those records and the shape of the object it returns.

The registry is a living set of codes maintained on the server (currently
70+ and growing), not a fixed list baked into this documentation. Pull the
current registry from the endpoint below rather than hardcoding codes in
your integration — new codes are added over time and existing ones can
change.

## Endpoints

| Method | Path | Purpose |
|---|---|---|
| `GET` | `/api/errors/errors/` | List the current error-code registry |

## Authentication

> [!NOTE]
> This endpoint requires the `Authorization` header described in
> [API Conventions](./conventions.md#authentication).

## List error codes

`GET /api/errors/errors/`

Returns every active error code Netki can attach to an object, most heavily
weighted (highest `rank`) first. This is the source of truth for the
registry — call it to build a lookup table in your own system instead of
maintaining a static copy.

**Path / query parameters**

| Name | In | Type | Description |
|---|---|---|---|
| `category` | query | string | Filter to an exact category — one of the six values in [Categories](#categories) below |
| `rank` | query | integer | Return only codes with a `rank` greater than this value |
| `page` | query | integer | Page number |
| `page_size` | query | integer | Results per page |

**Request**

```bash
curl -X "GET" "https://kyc.myverify.info/api/errors/errors/" \
     -H 'Authorization: Bearer eyJ...<truncated>'
```

**Response `200`**

> [!NOTE]
> The two entries below are real codes shown only as **examples** of the
> shape of the data. The full registry currently holds 70+ codes across all
> six categories — call the endpoint to get the complete, current list. Do
> not treat this excerpt as exhaustive or hardcode it in your integration.

```json
{
  "count": 71,
  "next": "https://kyc.myverify.info/api/errors/errors/?page=2",
  "previous": null,
  "results": [
    {
      "error_code_id": 2523,
      "created": "2026-01-14T16:12:18.816352Z",
      "updated": "2026-02-03T19:30:21.355919Z",
      "is_active": true,
      "error_code_name": "document_birthdate_invalid",
      "rank": 21,
      "category": "document",
      "error_code_description": "Unable to read birth date. Retake photo & ensure birth date is visible",
      "language_code": "en"
    },
    {
      "error_code_id": 2522,
      "created": "2026-01-14T16:10:53.965554Z",
      "updated": "2026-02-03T19:30:21.349950Z",
      "is_active": true,
      "error_code_name": "document_quality",
      "rank": 20,
      "category": "document",
      "error_code_description": "Document quality is low. Retake photo in good lighting",
      "language_code": "en"
    }
  ]
}
```

The envelope (`count`, `next`, `previous`, `results`) follows the standard
list format described in [API Conventions](./conventions.md#pagination).

### Error-code object fields

| Field | Type | Description |
|---|---|---|
| `error_code_id` | integer | Permanent numeric identifier for this code. It never changes, so this is the value to key your integration's logic off of |
| `error_code_name` | string | Machine-readable name for this code (for example `document_quality`). Convenient for readability, but it can change over time — do not rely on it for matching |
| `rank` | integer | Numeric weight used to order the registry and to filter with the `rank` query parameter above; higher generally means a more specific or more severe condition |
| `category` | string | One of the six values in [Categories](#categories) below |
| `error_code_description` | string | Human-readable description of the error, in the language given by `language_code` |
| `language_code` | string | Language of `error_code_description` (for example `en`) |
| `created` / `updated` | string (ISO-8601) | Timestamps |
| `is_active` | boolean | Whether this code is currently active in the registry |

## Categories

`category` is always one of the following six values:

| Category | Meaning |
|---|---|
| `document` | Something about a captured or uploaded document — image quality, an unreadable field, an expired ID, an unsupported document type |
| `transaction` | Something about the overall verification transaction — its state, its processing, or a restart |
| `identity` | Something about the customer's personal or biographical data — a mismatch, an age or status flag |
| `business` | Something related to your business account's configuration |
| `dataprovider` | A result or issue returned by one of the third-party verification checks run during processing (for example AML/sanctions screening) |
| `internal` | Reserved for Netki-internal processing issues. Not generally actionable on your side — if you see one repeatedly, contact Netki support |

## Error codes on transactions and identities

These codes are what populate the `errors` arrays you see when polling —
at the root of a transaction, on `transaction_identity`, on individual
`identity_documents` entries, and on `identity_accredited_investor_status`.
Each entry in an `errors` array embeds one of these error-code objects. See
[errors fields](./polling.md#errors-fields) in [Polling](./polling.md) for
the exact shape of those arrays.

> [!TIP]
> Match on `error_code_id` when you drive logic off a code — it is permanent
> and never changes. The `error_code_name` and `error_code_description` can
> both change over time, so don't key logic off them. Pull the current
> registry periodically rather than caching it indefinitely.
