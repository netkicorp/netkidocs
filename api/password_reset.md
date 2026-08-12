# Password Reset

Resets the password for the account you use to access the Netki API and
compliance dashboard. It is a two-step flow: request a reset code by email,
then confirm the new password using that code.

> [!NOTE]
> These endpoints are public — they do **not** require a bearer token, since
> they exist for a user who cannot log in. They are rate-limited; sending too
> many requests returns `429 Too Many Requests`.

## Endpoints

| Method | Path | Purpose |
|---|---|---|
| `POST` | `/api/password-reset/` | Email a password reset code to an account |
| `POST` | `/api/password-reset/confirm/` | Set a new password using the emailed code |

## Request a password reset code

`POST /api/password-reset/`

Looks up an active account by email address and, if one matches, emails it a
password reset code.

To avoid revealing whether an email address has an account, this endpoint
**always returns the same `200` response** whether or not a matching account
was found.

**Request fields**

| Field | Required | Type | Description |
|---|---|---|---|
| `email` | yes | string | Email address of the account to reset |

**Request**

```bash
curl -X "POST" "https://kyc.myverify.info/api/password-reset/" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -d $'{
  "email": "jane.doe@example.com"
}'
```

**Response `200`**

```json
{
  "detail": "If an account matches, a password reset code has been sent."
}
```

**Response fields**

| Field | Type | Description |
|---|---|---|
| `detail` | string | Confirmation message. The same regardless of whether an account matched |

**Errors**

| Status | Code | Meaning |
|---|---|---|
| `400` | — | `email` was missing or not a valid email address |
| `429` | — | Too many requests; wait and retry |

## Confirm a password reset

`POST /api/password-reset/confirm/`

Sets the new password using the code from the reset email.

**Request fields**

| Field | Required | Type | Description |
|---|---|---|---|
| `code` | yes | string | The reset code from the password-reset email |
| `new_password` | yes | string | The new password to set. Must meet Netki's password requirements |

**Request**

```bash
curl -X "POST" "https://kyc.myverify.info/api/password-reset/confirm/" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -d $'{
  "code": "<reset-code-from-email>",
  "new_password": "<new-password>"
}'
```

**Response `200`**

```json
{
  "detail": "Password has been reset."
}
```

**Response fields**

| Field | Type | Description |
|---|---|---|
| `detail` | string | Confirmation that the password was reset |

**Errors**

| Status | Code | Meaning |
|---|---|---|
| `400` | — | The `code` is invalid or expired (`{ "code": ["Invalid or expired reset code."] }`), or `new_password` failed the password requirements (`{ "new_password": [ ... ] }`) |
| `429` | — | Too many requests; wait and retry |
