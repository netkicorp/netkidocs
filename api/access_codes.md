# Access Codes

Access codes are single-use tokens that Netki generates for your customers so
they can enter the correct verification flow in the app. Give each code to
exactly one customer, and use it afterward to look up how that person's
verification went. Codes are provisioned by your account executive — contact
them for more codes.

> [!TIP]
> **Best practices for access codes**
>
> - This endpoint is the source of truth for **which codes exist** and
>   **whether each has been used** — but not for distribution. Netki has no
>   visibility into who you handed a given code to.
> - Pull your codes from this endpoint and store them in **your own database**,
>   and track who each code was distributed to on your side. Use this endpoint
>   to reconcile which of your distributed codes have been used.  You can also
>   reconcile them using the callbacks as the access code will be in the callback
>   data.

## Endpoints

| Method | Path | Purpose |
|---|---|---|
| `GET` | `/api/business/businesses/{business_id}/access-codes/` | List the access codes for a business |
| `GET` | `/api/business/businesses/{business_id}/access-codes/{code}/` | Retrieve a single access code by its code |

## Authentication

> [!NOTE]
> Both endpoints require the `Authorization` header described in
> [API Conventions](./conventions.md#authentication). The `business_id` in
> these paths is the `id` of your business account — retrieve it from
> [Businesses](./businesses.md).

Access codes are read-only through this API — they cannot be created,
updated, or deleted here. New codes are generated for you by your account
executive.

## List access codes

`GET /api/business/businesses/{business_id}/access-codes/`

Returns the access codes belonging to a business, most recently created
first.

**Path / query parameters**

| Name | In | Type | Description |
|---|---|---|---|
| `business_id` | path | string (UUID) | The `id` of your business account (see [Businesses](./businesses.md)) |
| `code` | query | string | Filter to an exact code |
| `is_active` | query | boolean | Whether the code is still valid. `is_active=true` returns unused (still valid) codes; `is_active=false` returns used codes. This is the correct way to check whether a code has been used |
| `parent_code__code` | query | string | Filter to codes that are children of the given parent code |
| `is_null` | query | string | Comma-separated field names that must be null. Use `is_null=parent_code` to return original codes (those that are not restarts) |
| `is_notnull` | query | string | Comma-separated field names that must not be null. Use `is_notnull=parent_code` to return only restart codes |
| `ordering` | query | string | `created` or `updated`, prefix with `-` for descending. Defaults to `-created` |

**Request**

```bash
curl -X "GET" "https://kyc.myverify.info/api/business/businesses/604e1738-4716-4bdd-867b-4942186b1e1c/access-codes/" \
     -H 'Authorization: Bearer eyJ...<truncated>'
```

**Response `200`**

```json
{
  "count": 2,
  "next": null,
  "previous": null,
  "results": [
    {
      "id": "4524f4ad-de8e-482e-a2c5-d60a76a27c62",
      "code": "nkt123",
      "identity": null,
      "parent_code": null,
      "child_codes": [],
      "is_active": true,
      "business": "604e1738-4716-4bdd-867b-4942186b1e1c",
      "created": "2026-06-01T23:24:47.784740Z",
      "updated": "2026-06-01T23:24:47.784757Z"
    },
    {
      "id": "9b6b6f8e-9f2a-4a3f-9a3a-9d5e9b3f2c1a",
      "code": "nkt124",
      "identity": "f25766e2-24b9-4d73-807b-ce9e50194874",
      "parent_code": null,
      "child_codes": [
        {
          "code": "nkt125",
          "is_active": true,
          "identity": null,
          "created": "2026-06-03T09:12:00.100000Z"
        }
      ],
      "is_active": false,
      "business": "604e1738-4716-4bdd-867b-4942186b1e1c",
      "created": "2026-05-28T00:02:24.333916Z",
      "updated": "2026-06-03T09:12:00.100000Z"
    }
  ]
}
```

The envelope (`count`, `next`, `previous`, `results`) follows the standard
list format described in [API Conventions](./conventions.md#pagination).

**Response fields**

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | Access code identifier |
| `code` | string | The access code string given to the customer |
| `identity` | string (UUID) \| null | Identifier of the identity that used this code, or `null` if unused |
| `parent_code` | object \| null | The code this one restarted from, if any (see below) |
| `child_codes` | array | Codes issued as restarts of this code (see below) |
| `is_active` | boolean | `true` while the code is still available to use; `false` once used |
| `business` | string (UUID) | Business this code belongs to |
| `created` | string (ISO-8601) | When the code was generated |
| `updated` | string (ISO-8601) | When the code was last modified (for example, when it was used) |

**`parent_code` / `child_codes` fields**

`parent_code` and each entry in `child_codes` share the same shape:

| Field | Type | Description |
|---|---|---|
| `code` | string | The access code string |
| `is_active` | boolean | Whether this code is still available to use |
| `identity` | string (UUID) \| null | Identifier of the identity that used this code, or `null` if unused |
| `created` | string (ISO-8601) | When this code was generated |

**Errors**

| Status | Code | Meaning |
|---|---|---|
| `401` | `not_authenticated` | No `Authorization` header was sent |
| `401` | `token_not_valid` | Access token is expired, invalid, or malformed |
| `404` | `not_found` | `business_id` does not exist or is not accessible to your account |

## Retrieve an access code

`GET /api/business/businesses/{business_id}/access-codes/{code}/`

Returns a single access code by its code value. Check the `is_active` field to
tell whether the code has been used: `is_active=true` means the code is unused
and still valid; `is_active=false` means it has been used. When a code has been
used, `identity` holds the identifier of the customer who used it.

**Path / query parameters**

| Name | In | Type | Description |
|---|---|---|---|
| `business_id` | path | string (UUID) | The `id` of your business account (see [Businesses](./businesses.md)) |
| `code` | path | string | The access code to look up |

**Request**

```bash
curl -X "GET" "https://kyc.myverify.info/api/business/businesses/604e1738-4716-4bdd-867b-4942186b1e1c/access-codes/nkt123/" \
     -H 'Authorization: Bearer eyJ...<truncated>'
```

**Response `200`** — an unused code

```json
{
  "id": "4524f4ad-de8e-482e-a2c5-d60a76a27c62",
  "code": "nkt123",
  "identity": null,
  "parent_code": null,
  "child_codes": [],
  "is_active": true,
  "business": "604e1738-4716-4bdd-867b-4942186b1e1c",
  "created": "2026-06-01T23:24:47.784740Z",
  "updated": "2026-06-01T23:24:47.784757Z"
}
```

**Response `200`** — a used code

```json
{
  "id": "4524f4ad-de8e-482e-a2c5-d60a76a27c62",
  "code": "nkt123",
  "identity": "f25766e2-24b9-4d73-807b-ce9e50194874",
  "parent_code": null,
  "child_codes": [],
  "is_active": false,
  "business": "604e1738-4716-4bdd-867b-4942186b1e1c",
  "created": "2026-06-01T23:24:47.784740Z",
  "updated": "2026-06-02T01:15:09.333916Z"
}
```

**Response fields**

Same shape as an entry in [List access codes](#list-access-codes) above.

**Errors**

| Status | Code | Meaning |
|---|---|---|
| `401` | `not_authenticated` | No `Authorization` header was sent |
| `401` | `token_not_valid` | Access token is expired, invalid, or malformed |
| `404` | `not_found` | `code` does not exist under this `business_id`, or is not accessible to your account |

## Restarted codes

When a customer restarts a verification, Netki issues a new *child* code
linked back to the original. Codes may be **allocated** (reserved for a
restart) without yet being **used**:

- A code with a non-null `parent_code` is a restart of that parent, and
  should be given to the same person who received the parent code.
- A code with entries in `child_codes` has been restarted; the callback
  delivered for the restart includes the new code, so it's best practice to
  treat it as allocated immediately rather than assuming it is still free
  to hand out elsewhere.

Example of a code that is itself a restart (has a parent):

```json
{
  "id": "c2f3a4b5-6d7e-4f80-9a1b-2c3d4e5f6071",
  "code": "nkt222",
  "identity": null,
  "parent_code": {
    "code": "nkt111",
    "is_active": false,
    "identity": "d92a4fad-cd70-47d5-aed9-df71f060c66f",
    "created": "2026-05-20T17:24:04.495846Z"
  },
  "child_codes": [],
  "is_active": true,
  "business": "604e1738-4716-4bdd-867b-4942186b1e1c",
  "created": "2026-05-26T14:15:32.252862Z",
  "updated": "2026-05-26T14:15:32.259285Z"
}
```

Example of a code that has been restarted (has a child):

```json
{
  "id": "a1b2c3d4-5e6f-4708-8192-a3b4c5d6e7f8",
  "code": "nkt111",
  "identity": "d92a4fad-cd70-47d5-aed9-df71f060c66f",
  "parent_code": null,
  "child_codes": [
    {
      "code": "nkt222",
      "is_active": true,
      "identity": null,
      "created": "2026-05-26T14:15:32.252862Z"
    }
  ],
  "is_active": false,
  "business": "604e1738-4716-4bdd-867b-4942186b1e1c",
  "created": "2026-05-20T17:24:04.495846Z",
  "updated": "2026-05-26T14:15:32.259285Z"
}
```
