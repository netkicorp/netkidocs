# AML-Only Processing

Some businesses want AML/sanctions screening on its own, without running
someone through the full document-and-selfie verification flow. This
endpoint screens a person, a company, or a blockchain address and hands you
back an ID you can use to retrieve the result — the same way you would for
any other transaction.

## Endpoints

| Method | Path | Purpose |
|---|---|---|
| `POST` | `/api/single-aml/` | Run a standalone AML/sanctions screening |

## Authentication

> [!NOTE]
> This endpoint requires the `Authorization` header described in
> [API Conventions](./conventions.md#authentication). It is available to
> client admins and managers on your account.

## Run an AML check

`POST /api/single-aml/`

This call is **asynchronous**. It validates your request, creates a record,
and schedules the actual screening — the response only confirms that
screening was scheduled. See [Getting your results](#getting-your-results)
below for how to retrieve the outcome.

Set `aml_type` to choose what you're screening:

- `individual` — screens a person by name (and optionally date of birth).
  Creates a transaction.
- `blockchain` — screens a wallet or entity address on a blockchain.
  Creates a transaction.
- `corporate` — screens a company by name. Creates a business record
  instead of a transaction.

**Request fields**

| Field | Required | Type | Description |
|---|---|---|---|
| `aml_type` | yes | string | `individual`, `corporate`, or `blockchain` |
| `filter` | no | string | Selects which screening filter to apply. Netki can set up custom search filters for your business — contact your account manager to have one created. If omitted, your account's standard filter is used. Applies to `individual` and `corporate` checks only — `blockchain` checks don't use a filter |
| `first_name` / `last_name` | individual, see note | string | Provide both together as one valid way to identify who you're screening |
| `middle_name` | no | string | Individual checks only |
| `full_name` | individual (alternative) or corporate (required) | string | Alternative to `first_name`/`last_name` for names that aren't split into parts (common for many Asian countries' names). Required for `corporate` |
| `date_of_birth` | no | string (date, `YYYY-MM-DD`) | Individual checks only. Reduces false positives |
| `blockchain_address` | required for `blockchain` | string | The wallet or entity address to screen |
| `business_id` | no | string (UUID) | Run the check under a specific business you own, or a direct child business of yours. Defaults to your own business. Don't send this for `corporate` checks |

For `individual`, provide either `first_name` + `last_name`, or `full_name`
— one of the two is required. Name fields may not contain numbers, and no
special characters other than a hyphen are allowed.

### Individual

**Request**

```bash
curl -X "POST" "https://kyc.myverify.info/api/single-aml/" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -H 'Authorization: Bearer eyJ...<truncated>' \
     -d $'{
  "aml_type": "individual",
  "first_name": "Jane",
  "last_name": "Doe",
  "date_of_birth": "1990-04-12",
  "filter": "default"
}'
```

**Response `200`**

```json
{
  "message": "Created transaction 83d0d0b0-68e8-4746-8197-ca4d18a21e2c and scheduled for run through AML.",
  "transaction_id": "83d0d0b0-68e8-4746-8197-ca4d18a21e2c"
}
```

### Blockchain

**Request**

```bash
curl -X "POST" "https://kyc.myverify.info/api/single-aml/" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -H 'Authorization: Bearer eyJ...<truncated>' \
     -d $'{
  "aml_type": "blockchain",
  "blockchain_address": "bc1qexampleaddressnotarealwallet0000"
}'
```

**Response `200`**

```json
{
  "message": "Created transaction 2f6f2f0a-6a2e-4a3f-9d0c-1a2b3c4d5e6f and scheduled for run through AML.",
  "transaction_id": "2f6f2f0a-6a2e-4a3f-9d0c-1a2b3c4d5e6f"
}
```

### Corporate

Corporate checks create a business record rather than a transaction, and
only accept `full_name` and `filter` — sending any individual-only field
(`first_name`, `middle_name`, `last_name`, `date_of_birth`, or `business_id`)
is rejected. See [Errors](#errors) below.

**Request**

```bash
curl -X "POST" "https://kyc.myverify.info/api/single-aml/" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -H 'Authorization: Bearer eyJ...<truncated>' \
     -d $'{
  "aml_type": "corporate",
  "full_name": "Acme Example Holdings LLC",
  "filter": "default"
}'
```

**Response `200`**

```json
{
  "message": "Created business 9c2e1a3f-7b4d-4e5a-8f6c-1d2e3f4a5b6c and scheduled for run through AML.",
  "business_id": "9c2e1a3f-7b4d-4e5a-8f6c-1d2e3f4a5b6c"
}
```

**Response fields**

| Field | Type | Description |
|---|---|---|
| `message` | string | Confirmation message |
| `transaction_id` | string (UUID) | Present for `individual` and `blockchain`. The created transaction — see [Getting your results](#getting-your-results) |
| `business_id` | string (UUID) | Present for `corporate`. The created business record — see [Getting your results](#getting-your-results) |

## Errors

> [!NOTE]
> An invalid `aml_type` currently returns HTTP `200`, not an error status —
> check the `message` field for `"Invalid AML Type.  Needs to be corporate,
> individual, or blockchain."` rather than relying on the status code alone.

| Status | Code | Meaning |
|---|---|---|
| `200` | — | `aml_type` was not one of `individual`, `corporate`, or `blockchain` |
| `400` | — | `date_of_birth` could not be parsed, or is in the future |
| `400` | — | No usable name or address was supplied — provide `first_name` + `last_name`, `full_name`, or `blockchain_address` |
| `400` | — | A name field is blank, contains a number, or contains a disallowed special character (only hyphens are permitted besides letters) |
| `400` | — | `business_id` was supplied but is not a valid UUID |
| `400` | — | `aml_type` is `corporate` but `full_name` was not supplied |
| `400` | — | `aml_type` is `corporate` but an individual-only field (`first_name`, `middle_name`, `last_name`, `date_of_birth`, or `business_id`) was also supplied |
| `401` | `not_authenticated` | No `Authorization` header was sent |
| `401` | `token_not_valid` | Access token is expired, invalid, or malformed |
| `403` | — | Your account does not have permission to use this endpoint |
| `404` | — | `filter` does not match a screening filter configured for your business |
| `404` | — | `business_id` does not exist, or isn't your business or a direct child business of yours |
| `429` | — | Too many requests — this endpoint is rate-limited |

## Getting your results

This endpoint only schedules the screening — it does not return a result
directly. What you do next depends on which `aml_type` you ran:

- **`individual` and `blockchain`** return a `transaction_id`. Poll it with
  [`GET /api/transactions/{id}/`](./polling.md), or configure a callback
  URL to be notified when screening finishes — see
  [Callbacks](./callbacks2.md) and
  [Callback Best Practices](./best_practices_internal_callbacks.md).
- **`corporate`** returns a `business_id` for the newly created business
  record. Results for corporate checks are delivered by callback rather
  than through the transaction-polling endpoints above — see
  [Callbacks](./callbacks2.md) and
  [Callback Best Practices](./best_practices_internal_callbacks.md) to make
  sure your callback URL is configured before you run a corporate check.

> [!TIP]
> Set up a callback URL even if you plan to poll — a lapse in your polling
> schedule won't cause you to miss a result if a callback was also
> delivered.
