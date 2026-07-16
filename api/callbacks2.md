# Callbacks

Callbacks let Netki push transaction updates to your servers instead of you
having to poll for them. Configure a `callback_url` for your business and
Netki will POST the transaction to it whenever there's something worth
knowing — a KYC decision, a state change, a document upload. This page
covers how delivery and retries work, how to configure and troubleshoot your
callback endpoint, and the endpoints for managing callback delivery. For the
fields inside the transaction itself, see [Transactions](./polling.md).

## Endpoints

| Method | Path | Purpose |
|---|---|---|
| `GET` | `/api/business/businesses/{business_id}/config/` | View your callback configuration |
| `PUT` | `/api/business/businesses/{business_id}/config/{config_id}/` | Update your callback configuration |
| `POST` | `/api/transactions/{id}/make-callback/` | Manually trigger a callback for a transaction |
| `GET` | `/api/transactions/{id}/get-callbacks/` | List callback delivery attempts for a transaction |
| `PUT` | `/api/transactions/restart-callbacks/` | Resend all paused callbacks |

## Authentication

> [!NOTE]
> All endpoints on this page require the `Authorization` header described in
> [API Conventions](./conventions.md#authentication).

## How callbacks work

When a transaction reaches a state your integration should react to — for
example it moves out of `hold` into `completed`, `failed`, or `restarted` —
Netki sends an HTTP POST to the `callback_url` configured for your business.
Delivery happens in the background: whatever action changed the transaction
(a customer finishing the KYC flow, a compliance reviewer making a decision,
and so on) does not wait for your endpoint to respond.

A delivery attempt only counts as successful if your endpoint returns the
exact HTTP status code configured in `callback_expected_status` (default
`200`). Anything else — a different status code, a connection error, a
timeout — counts as a failed attempt.

### Retry and backoff

If an attempt fails, Netki retries with exponential backoff: the next
attempt waits roughly 30 minutes, then the wait roughly doubles each time
after that (~1 hour, ~2 hours, ~4 hours, and so on). The number of attempts,
including the first, is capped by your business's `callback_retry_limit`
(1-8, default 8).

Once that limit is reached without a successful delivery, Netki stops
retrying and pauses **all** callback delivery for your business
(`is_callback_paused` is set to `true`) rather than continuing to fail
silently. To resume:

1. Fix whatever was rejecting the callbacks — see
   [Configure your callback](#configure-your-callback) below.
2. Set `is_callback_paused` back to `false`.
3. Call [Restart paused callbacks](#restart-paused-callbacks) to resend
   everything that queued up while paused.

> [!NOTE]
> If `is_callback_enabled` is `false`, Netki doesn't attempt callbacks for
> your business at all — nothing is queued, and nothing needs restarting
> later.

## Configure your callback

Your business has one callback configuration, covering the destination URL,
authentication, and retry behavior described above.

### View your callback configuration

`GET /api/business/businesses/{business_id}/config/`

Returns your callback configuration. `callback_credentials` is write-only
and is never included in the response.

**Path parameters**

| Name | In | Type | Description |
|---|---|---|---|
| `business_id` | path | string (UUID) | The `id` of your business account |

**Request**

```bash
curl -X "GET" "https://kyc.myverify.info/api/business/businesses/604e1738-4716-4bdd-867b-4942186b1e1c/config/" \
     -H 'Authorization: Bearer eyJ...<truncated>'
```

**Response `200`**

```json
{
  "count": 1,
  "next": null,
  "previous": null,
  "results": [
    {
      "id": "8f139fc9-0f3f-4a1b-9dc2-d6dfb46fa205",
      "callback_url": "https://your-domain.example/callbacks/",
      "callback_authentication_scheme": "basic",
      "callback_expected_status": 200,
      "callback_retry_limit": 8,
      "is_callback_paused": false,
      "is_callback_enabled": true
    }
  ]
}
```

The envelope (`count`, `next`, `previous`, `results`) follows the standard
list format described in [API Conventions](./conventions.md#pagination);
there will only ever be one entry.

**Response fields**

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | Configuration record identifier. Use this in the URL when updating your configuration |
| `callback_url` | string (URL) \| null | The URL Netki sends callbacks to |
| `callback_authentication_scheme` | string | `basic` or `noauth`. Contact your account executive if you need another scheme supported |
| `callback_expected_status` | integer | The HTTP status code your endpoint must return for a delivery attempt to count as successful. Defaults to `200` |
| `callback_retry_limit` | integer | Number of delivery attempts, including the first, before Netki pauses your callbacks. `1`-`8`, default `8` |
| `is_callback_paused` | boolean | Whether callback delivery is currently paused for your business |
| `is_callback_enabled` | boolean | Whether Netki attempts callbacks for your business at all |

> [!NOTE]
> `callback_credentials` also belongs to this configuration but is write-only
> — you can set it, but it never appears in a response. See
> [Update your callback configuration](#update-your-callback-configuration).

**Errors**

| Status | Code | Meaning |
|---|---|---|
| `401` | `not_authenticated` | No `Authorization` header was sent |
| `401` | `token_not_valid` | Access token is expired, invalid, or malformed |
| `404` | `not_found` | `business_id` does not exist or is not accessible to your account |

### Update your callback configuration

`PUT /api/business/businesses/{business_id}/config/{config_id}/`

Updates your callback configuration. Send only the fields you want to
change.

**Path parameters**

| Name | In | Type | Description |
|---|---|---|---|
| `business_id` | path | string (UUID) | The `id` of your business account |
| `config_id` | path | string (UUID) | The configuration's `id`, from [View your callback configuration](#view-your-callback-configuration) |

**Request**

```bash
curl -X "PUT" "https://kyc.myverify.info/api/business/businesses/604e1738-4716-4bdd-867b-4942186b1e1c/config/8f139fc9-0f3f-4a1b-9dc2-d6dfb46fa205/" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -H 'Authorization: Bearer eyJ...<truncated>' \
     -d $'{
  "callback_url": "https://your-domain.example/callbacks/",
  "callback_authentication_scheme": "basic",
  "callback_credentials": "{\"username\": \"your-username\", \"password\": \"your-password\"}",
  "callback_expected_status": 200,
  "callback_retry_limit": 8,
  "is_callback_enabled": true
}'
```

**Request fields**

| Field | Required | Type | Description |
|---|---|---|---|
| `callback_url` | no | string (URL) | The URL to send callbacks to |
| `callback_authentication_scheme` | no | string | `basic` or `noauth` |
| `callback_credentials` | required if `callback_authentication_scheme` is not `noauth` | string (JSON) | Write-only. A JSON-encoded object with `username` and `password`, for example `{"username": "...", "password": "..."}`, used as HTTP basic-auth credentials on each callback request. Never returned by the API. Switching `callback_authentication_scheme` to `noauth` clears any stored credentials automatically |
| `callback_expected_status` | no | integer | Status code your endpoint returns on success |
| `callback_retry_limit` | no | integer | `1`-`8` |
| `is_callback_paused` | no | boolean | Set to `false` to unpause delivery. This does not resend anything by itself — call [Restart paused callbacks](#restart-paused-callbacks) afterward |
| `is_callback_enabled` | no | boolean | Turn callback delivery on or off entirely |

**Response `200`**

Same shape as [View your callback configuration](#view-your-callback-configuration), reflecting the updated values. `callback_credentials` is never included.

**Errors**

| Status | Code | Meaning |
|---|---|---|
| `400` | — | `callback_credentials` was blank while `callback_authentication_scheme` was set to something other than `noauth` |
| `401` | `not_authenticated` | No `Authorization` header was sent |
| `401` | `token_not_valid` | Access token is expired, invalid, or malformed |
| `404` | `not_found` | `business_id` or `config_id` does not exist or is not accessible to your account |

## The callback body

The body Netki POSTs to your `callback_url` is a transaction object — the
same shape returned by
[`GET /api/transactions/{id}/`](./polling.md#retrieve-a-transaction).
[Transactions](./polling.md) is the single source of truth for every field
in it; this page won't repeat that reference.

The transaction is wrapped in a single top-level key:

```json
{
  "identity": {
    "id": "83d0d0b0-68e8-4746-8197-ca4d18a21e2c",
    "client": "604e1738-4716-4bdd-867b-4942186b1e1c",
    "state": "completed",
    "transaction_identity": { "...": "see ./polling.md" },
    "transaction_metadata": { "...": "see ./polling.md" },
    "transaction_callbacks": [ { "...": "see below" } ]
  }
}
```

> [!NOTE]
> The wrapper key is `identity` for historical reasons — its value is the
> full transaction object, not just the identity. Read the object contained
> inside the `identity` key exactly as you would the body returned by
> [`GET /api/transactions/{id}/`](./polling.md#retrieve-a-transaction).

One difference from a polled transaction: entries in the payload's
`transaction_callbacks` array omit `response_raw`, so a callback can't grow
unbounded if your endpoint echoes back what it received. Otherwise each
entry has the same fields as [the callback record](#the-callback-record)
below.

## The callback record

Every delivery attempt is stored as a callback record. Fetch these with
[List callback attempts](#list-callback-attempts-for-a-transaction) to see
delivery history and troubleshoot a transaction that isn't triggering
callbacks the way you expect.

**Callback record fields**

| Field | Type | Description |
|---|---|---|
| `id` | integer | Callback record identifier |
| `state` | string | Delivery state — see [Callback states](#callback-states) |
| `response_raw` | string \| null | The raw response body your endpoint returned, truncated to 2000 characters |
| `status_code` | integer \| null | The HTTP status code your endpoint returned. `null` if the request itself failed (for example a connection error) |
| `callback_duration` | string \| null | Seconds the request took to complete |
| `callback_url` | string \| null | The URL this attempt was sent to |
| `callback_counter` | integer | Attempt number for this callback, starting at `1` and incrementing on each retry |
| `transaction` | string (UUID) | The transaction this callback belongs to |
| `created` / `updated` | string (ISO-8601) | Timestamps |
| `is_active` | boolean | Whether this record is active |

### Callback states

| State | Meaning |
|---|---|
| `new` | Created and waiting to be sent |
| `processing` | Currently being sent |
| `paused` | Held because callback delivery is paused for your business — either you paused it, or Netki paused it after hitting `callback_retry_limit` |
| `restarting` | Queued for redelivery after [Restart paused callbacks](#restart-paused-callbacks) was called |
| `failed` | The last attempt didn't get the expected status code (or the request itself failed), and Netki has exhausted its retries for this attempt |
| `completed` | Your endpoint returned the expected status code |

## Manually trigger a callback

`POST /api/transactions/{id}/make-callback/`

Queues a callback for the given transaction right now, using your currently
configured `callback_url`. Useful for testing how your endpoint handles the
payload, or for nudging a single transaction without waiting on the retry
schedule. Like automatic callbacks, delivery happens in the background —
this endpoint doesn't return the delivery result inline. Check
[List callback attempts](#list-callback-attempts-for-a-transaction)
afterward to see the outcome. If callbacks are disabled for your business
(`is_callback_enabled` is `false`), no callback is actually sent even though
this endpoint reports success.

**Path parameters**

| Name | In | Type | Description |
|---|---|---|---|
| `id` | path | string (UUID) | The transaction's `id` |

**Request**

```bash
curl -X "POST" "https://kyc.myverify.info/api/transactions/83d0d0b0-68e8-4746-8197-ca4d18a21e2c/make-callback/" \
     -H 'Authorization: Bearer eyJ...<truncated>'
```

**Response `200`**

```json
{
  "message": "Callback scheduled."
}
```

**Response fields**

| Field | Type | Description |
|---|---|---|
| `message` | string | Confirmation message |

**Errors**

| Status | Code | Meaning |
|---|---|---|
| `401` | `not_authenticated` | No `Authorization` header was sent |
| `401` | `token_not_valid` | Access token is expired, invalid, or malformed |
| `404` | `not_found` | `id` does not exist or is not accessible to your account |

## List callback attempts for a transaction

`GET /api/transactions/{id}/get-callbacks/`

Returns every callback record for a transaction, most recent activity
first.

**Path / query parameters**

| Name | In | Type | Description |
|---|---|---|---|
| `id` | path | string (UUID) | The transaction's `id` |
| `state` | query | string | Filter to an exact [callback state](#callback-states) |

**Request**

```bash
curl -X "GET" "https://kyc.myverify.info/api/transactions/83d0d0b0-68e8-4746-8197-ca4d18a21e2c/get-callbacks/" \
     -H 'Authorization: Bearer eyJ...<truncated>'
```

**Response `200`**

```json
{
  "count": 1,
  "callbacks": [
    {
      "id": 5957,
      "created": "2026-07-01T09:05:45.103396Z",
      "updated": "2026-07-01T09:05:47.637570Z",
      "is_active": true,
      "state": "completed",
      "response_raw": "OK",
      "status_code": 200,
      "callback_duration": "0.0088900000",
      "callback_url": "https://your-domain.example/callbacks/",
      "callback_counter": 1,
      "transaction": "83d0d0b0-68e8-4746-8197-ca4d18a21e2c"
    }
  ]
}
```

If the transaction has no callback records at all, the response is instead:

```json
{
  "message": "No callbacks found for that transaction."
}
```

**Response fields**

| Field | Type | Description |
|---|---|---|
| `count` | integer | Number of callback records returned |
| `callbacks` | array | Callback records, see [Callback record fields](#the-callback-record) |
| `message` | string | Present instead of `count`/`callbacks` when there are no callback records for this transaction |

**Errors**

| Status | Code | Meaning |
|---|---|---|
| `401` | `not_authenticated` | No `Authorization` header was sent |
| `401` | `token_not_valid` | Access token is expired, invalid, or malformed |
| `404` | `not_found` | `id` does not exist or is not accessible to your account |

## Restart paused callbacks

`PUT /api/transactions/restart-callbacks/`

Resends every callback currently in the `paused` state for your business.
This is the second step of resuming delivery after Netki auto-pauses your
callbacks — see [Retry and backoff](#retry-and-backoff). Updating
`is_callback_paused` to `false` on its own does not resend anything; you
must call this endpoint afterward.

**Request**

```bash
curl -X "PUT" "https://kyc.myverify.info/api/transactions/restart-callbacks/" \
     -H 'Authorization: Bearer eyJ...<truncated>'
```

**Response `200`** — callbacks queued for redelivery

```json
{
  "message": "Callbacks restarted."
}
```

`message` also comes back `200` (rather than an error status) in two cases
where nothing was queued:

- `is_callback_paused` is still `true` for your business — set it to `false`
  first, see
  [Update your callback configuration](#update-your-callback-configuration).
- `is_callback_enabled` is `false` for your business — enable callbacks
  first.

Check the `message` text to tell these apart from a successful restart.

**Response fields**

| Field | Type | Description |
|---|---|---|
| `message` | string | Confirmation, or an explanation of why nothing was restarted |

**Errors**

| Status | Code | Meaning |
|---|---|---|
| `401` | `not_authenticated` | No `Authorization` header was sent |
| `401` | `token_not_valid` | Access token is expired, invalid, or malformed |
| `404` | `not_found` | There are no callbacks currently in the `paused` state for your business |
