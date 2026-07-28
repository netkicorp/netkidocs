# Custom Dashboard

Netki provides a Compliance dashboard for reviewing transactions and acting on
them. If you would rather build your own dashboard instead of using the Netki
Compliance dashboard, the endpoints on this page let you move a transaction
along — approve it, fail it, or restart it — the same way a reviewer would in
the Netki dashboard.

To pull the transactions you want to review, see
[Transactions](./polling.md) (`GET /api/transactions/`). Use the endpoints here
to act on one.

> [!NOTE]
> These endpoints back manual, human-in-the-loop review actions. They are meant
> to be triggered by a person reviewing a case — do not use them as part of an
> automated workflow.

## Endpoints

| Method | Path | Purpose |
|---|---|---|
| `PUT` | `/api/transactions/{id}/make-completed/` | Manually approve a transaction |
| `PUT` | `/api/transactions/{id}/make-failed/` | Manually fail a transaction |
| `PUT` | `/api/transactions/{id}/make-restarted/` | Restart a transaction and issue the customer a new access code |

## Authentication

> [!NOTE]
> All endpoints on this page require the `Authorization` header described in
> [API Conventions](./conventions.md#authentication).

## Mark a transaction as completed

`PUT /api/transactions/{id}/make-completed/`

Manually approves a transaction, for example after a compliance reviewer
resolves a `hold`. This can be called from any current state.

**Path / query parameters**

| Name | In | Type | Description |
|---|---|---|---|
| `id` | path | string (UUID) | The transaction's `id` |

**Request**

```bash
curl -X "PUT" "https://kyc.myverify.info/api/transactions/83d0d0b0-68e8-4746-8197-ca4d18a21e2c/make-completed/" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -H 'Authorization: Bearer eyJ...<truncated>' \
     -d $'{
  "notes": "Reviewed and cleared by compliance."
}'
```

**Response `200`**

```json
{
  "message": "Transaction 83d0d0b0-68e8-4746-8197-ca4d18a21e2c moved to completed state."
}
```

**Response fields**

| Field | Type | Description |
|---|---|---|
| `message` | string | Confirmation message |

**Errors**

| Status | Code | Meaning |
|---|---|---|
| `400` | — | AML needs to be run again before this transaction can be completed |
| `401` | `not_authenticated` / `token_not_valid` | Missing or invalid access token |
| `404` | `not_found` | `id` does not exist or is not accessible to your account |

## Mark a transaction as failed

`PUT /api/transactions/{id}/make-failed/`

Manually fails a transaction. Valid from `new`, `processing`, `hold`,
`post_processing`, `completed`, or `customer_review`.

**Path / query parameters**

| Name | In | Type | Description |
|---|---|---|---|
| `id` | path | string (UUID) | The transaction's `id` |

**Request**

```bash
curl -X "PUT" "https://kyc.myverify.info/api/transactions/83d0d0b0-68e8-4746-8197-ca4d18a21e2c/make-failed/" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -H 'Authorization: Bearer eyJ...<truncated>' \
     -d $'{
  "notes": "Declined: sanctions match confirmed by compliance.",
  "reasons": ["sanctions_match_confirmed"]
}'
```

**Request fields**

| Field | Required | Type | Description |
|---|---|---|---|
| `notes` | yes | string | Reason for the decline; required and must not be blank |
| `reasons` | no | array of strings | One or more decline-reason codes configured for your business. Unknown codes are ignored |

**Response `200`**

```json
{
  "message": "Transaction 83d0d0b0-68e8-4746-8197-ca4d18a21e2c moved to failed state."
}
```

**Response fields**

| Field | Type | Description |
|---|---|---|
| `message` | string | Confirmation message |

**Errors**

| Status | Code | Meaning |
|---|---|---|
| `400` | — | `notes` was blank, or the transaction's current state cannot transition to `failed` (for example it is already `failed`, `restarted`, `canceled`, or `quarantine`) |
| `401` | `not_authenticated` / `token_not_valid` | Missing or invalid access token |
| `404` | `not_found` | `id` does not exist or is not accessible to your account |

## Restart a transaction

`PUT /api/transactions/{id}/make-restarted/`

Sends the customer a new SMS with a new access code so they can go through
verification again. Valid from `new`, `failed`, `hold`, `post_processing`,
or `customer_review`. Only meaningful for customers who entered through an
access code (MyVerify app) — SDK integrations don't use access codes and
won't use this endpoint.

The new access code is a *child* of the code the customer originally used.
It is not linked to that customer until someone actually uses it — see
[Restarted codes](./access_codes.md#restarted-codes) for how the
parent/child relationship works.

**Path / query parameters**

| Name | In | Type | Description |
|---|---|---|---|
| `id` | path | string (UUID) | The transaction's `id` |

**Request**

```bash
curl -X "PUT" "https://kyc.myverify.info/api/transactions/83d0d0b0-68e8-4746-8197-ca4d18a21e2c/make-restarted/" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -H 'Authorization: Bearer eyJ...<truncated>' \
     -d $'{
  "notes": "Customer asked to retake their ID photo.",
  "message": "Please try again using the link below."
}'
```

**Request fields**

| Field | Required | Type | Description |
|---|---|---|---|
| `notes` | yes | string | Internal reason for the restart; required and must not be blank |
| `message` | one of `message` / `error_code` required | string | Custom text to include in the SMS sent to the customer |
| `error_code` | one of `message` / `error_code` required | string | An error code to reference instead of custom text |

**Response `200`**

```json
{
  "message": "Transaction 83d0d0b0-68e8-4746-8197-ca4d18a21e2c restarted.",
  "new_access_code": "nktq2jy"
}
```

If the customer's phone number is in a country where SMS links cannot be
sent, the response instead looks like:

```json
{
  "message": "Transaction 83d0d0b0-68e8-4746-8197-ca4d18a21e2c restarted.",
  "new_access_code": "nktq2jy",
  "sms_blocked": true,
  "sms_blocked_message": "SMS with URL cannot be sent to this country. Please provide the access code to the user manually."
}
```

**Response fields**

| Field | Type | Description |
|---|---|---|
| `message` | string | Confirmation message |
| `new_access_code` | string | The new access code issued for the restart. Give this to the same customer who used the original code |
| `sms_blocked` | boolean | Present only when the restart SMS could not be sent |
| `sms_blocked_message` | string | Present only when `sms_blocked` is `true` |

**Errors**

| Status | Code | Meaning |
|---|---|---|
| `400` | — | Neither `message` nor `error_code` was sent, `notes` was blank, or the transaction's current state cannot transition to `restarted` |
| `401` | `not_authenticated` / `token_not_valid` | Missing or invalid access token |
| `404` | `not_found` | `id` does not exist or is not accessible to your account |
| `429` | — | This transaction was restarted very recently; wait before retrying |
