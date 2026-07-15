# Authentication

The Netki API uses JSON Web Tokens (JWTs) for authentication. Obtain a token
pair by logging in, then use the access token on every subsequent request
until it expires.

## Endpoints

| Method | Path | Purpose |
|---|---|---|
| `POST` | `/api/token-auth/` | Log in with a username and password and receive an access/refresh token pair |
| `POST` | `/api/token-refresh/` | Exchange a refresh token for a new access token |

## Authentication

> [!NOTE]
> `POST /api/token-auth/` and `POST /api/token-refresh/` are how you establish
> and renew your credentials — they do not themselves require a bearer token.
> Every other endpoint requires the `Authorization` header described in
> [API Conventions](./conventions.md#authentication).

Your account executive will provide your login credentials. The API is not
self-service; credential changes go through support.

## Log in

`POST /api/token-auth/`

Exchanges a username and password for an access/refresh token pair.

**Path / query parameters**

None.

**Request**

```bash
curl -X "POST" "https://kyc.myverify.info/api/token-auth/" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -d $'{
  "username": "client_username",
  "password": "client_password"
}'
```

**Response `200`**

```json
{
  "refresh": "eyJ...<truncated>",
  "access": "eyJ...<truncated>"
}
```

**Response fields**

| Field | Type | Description |
|---|---|---|
| `refresh` | string | Long-lived token used to obtain new access tokens at `/api/token-refresh/` |
| `access` | string | Short-lived token sent as `Authorization: Bearer <access>` on all other requests |

**Errors**

| Status | Code | Meaning |
|---|---|---|
| `401` | `no_active_account` | Username or password is incorrect |

## Refresh a token

`POST /api/token-refresh/`

Exchanges a refresh token for a new access token.

**Path / query parameters**

None.

**Request**

```bash
curl -X "POST" "https://kyc.myverify.info/api/token-refresh/" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -u ':' \
     -d $'{
  "refresh": "eyJ...<truncated>"
}'
```

**Response `200`**

```json
{
  "access": "eyJ...<truncated>"
}
```

**Response fields**

| Field | Type | Description |
|---|---|---|
| `access` | string | New short-lived access token |

**Errors**

| Status | Code | Meaning |
|---|---|---|
| `401` | `token_not_valid` | Refresh token is expired, invalid, or malformed |
