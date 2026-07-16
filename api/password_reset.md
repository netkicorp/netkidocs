# Password Reset

Resets the password for the account you use to access the Netki API and
compliance dashboard. It is a two-step flow: request a reset, then confirm the
new password with the token from the email that gets sent.

> [!NOTE]
> These two endpoints are served at the root of the domain, **not** under
> `/api/`, and they do **not** require a bearer token — they are how a user
> who cannot log in recovers access.

## Endpoints

| Method | Path | Purpose |
|---|---|---|
| `POST` | `/password-reset/` | Start a password reset for an account |
| `POST` | `/password-reset-confirm/` | Set a new password using the emailed token |

## Request a password reset

`POST /password-reset/`

Starts the reset process for the account with the given email address. On
success, an email containing a reset token is sent to that address.

**Request fields**

| Field | Required | Type | Description |
|---|---|---|---|
| `email` | yes | string | Email address (or username) of the account to reset |

**Request**

```bash
curl -X "POST" "https://kyc.myverify.info/password-reset/" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -d $'{
  "email": "jane.doe@example.com"
}'
```

**Response `200`**

```json
{
  "result": true,
  "message": "Password reset process initiated."
}
```

**Response fields**

| Field | Type | Description |
|---|---|---|
| `result` | boolean | `true` when the reset email was sent |
| `message` | string | Human-readable status message |

**Errors**

| Status | Code | Meaning |
|---|---|---|
| `404` | — | The account could not be reset (for example the email address is not recognized). Body is `{ "result": false, "message": "..." }` |

## Confirm a password reset

`POST /password-reset-confirm/`

Sets the new password using the token from the reset email. Requires a valid
reCAPTCHA response.

**Request fields**

| Field | Required | Type | Description |
|---|---|---|---|
| `password` | yes | string | The new password to set |
| `recaptcha` | yes | string | A valid reCAPTCHA response token |
| `token` | yes | string | The reset token from the password-reset email |

**Request**

```bash
curl -X "POST" "https://kyc.myverify.info/password-reset-confirm/" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -d $'{
  "password": "<new-password>",
  "recaptcha": "<recaptcha-response-token>",
  "token": "<reset-token-from-email>"
}'
```

**Response `200`**

```json
{
  "message": "Password reset."
}
```

**Response fields**

| Field | Type | Description |
|---|---|---|
| `message` | string | `Password reset.` on success; an error message if the token or reCAPTCHA was invalid |
