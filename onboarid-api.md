# OnboardID API

The OnboardID API is how you integrate with the parts of the OnboardID platform
that sit *around* the main KYC flow. The API does not capture a person's
identity documents or selfie — that happens in the **MyVerify app** or through
the **SDK** embedded in your own app. These endpoints are for everything else:
kicking off and tracking verifications, pulling results, automating your
business logic, managing callbacks, and building your own dashboards.

If you want to embed the KYC capture experience directly in your own mobile app
or web portal, start with the SDKs linked from the
[main documentation](./README.md) instead.

> [!NOTE]
> Every endpoint is served from `https://kyc.myverify.info`. Before your first
> call, read [API Conventions](./api/conventions.md) — it covers the base URL,
> the authentication header, pagination, timestamps, and the error format that
> every endpoint shares.

## MyVerify app vs. the SDK

How your end users enter verification determines a few of the docs below:

- **MyVerify app** — you route each person in with a single-use **access code**,
  usually embedded in a [deep link](./api/howto_deeplinks_access.md). The access
  code ties that run back to your records.
- **SDK** — you build your own KYC flow with our SDK, so there are no access
  codes. Instead you set a **`client_guid`** during the SDK flow. It works much
  like an access code: a unique identifier you generate that tracks a single run
  in our system and links it back to a record in yours.

Either way, each run through KYC is a **transaction** with its own transaction
ID. See [Callback Best Practices](./api/best_practices_internal_callbacks.md#track-each-attempt-with-a-unique-identifier)
for how to keep every attempt individually traceable.

## Start here

- **[API Conventions](./api/conventions.md)** — base URL, the `Authorization`
  header, pagination, timestamps/identifiers, and error format shared by every
  endpoint. Read this first.
- **[Authentication](./api/authentication.md)** — obtain and refresh your JWT.
  This is the first call you'll make; every other endpoint needs the token.

## Your account

- **[Businesses](./api/businesses.md)** — list the businesses on your account,
  each with the `id` you'll need for business-scoped calls, and their app
  configuration.

## Onboarding your users

- **[Access Codes](./api/access_codes.md)** — for MyVerify app users: list and
  check the single-use codes that route each person into verification, including
  how restarts work.
- **[Deep Links](./api/howto_deeplinks_access.md)** — MyVerify app only: embed an
  access code in a link that drops a user straight into the app, or through a
  form first. SDK integrations don't use these.

## Getting results

- **[Transactions / Polling](./api/polling.md)** — the transaction record and its
  states: pull a transaction to see status, results, and why something is on hold
  or failed. Polling regularly is a good complement to callbacks.
- **[Callbacks](./api/callbacks2.md)** — have Netki POST transaction updates to
  your server as they happen, and configure delivery, retries, and
  authentication. You can manage these settings here or in the Netki Compliance
  dashboard.
- **[Callback Best Practices](./api/best_practices_internal_callbacks.md)** — how
  to consume callbacks reliably, stay idempotent, and track each attempt.
- **[API Error Codes](./api/api_error_codes.md)** — the registry of codes Netki
  attaches to transactions, identities, and documents when something needs
  attention, and how to look them up.

## Advanced

- **[Custom Dashboard](./api/custom_dashboard.md)** — manually approve, fail, or
  restart transactions if you build your own review dashboard instead of using
  the Netki Compliance dashboard. This is advanced usage; contact your Netki
  account manager before going this route.

## Other endpoints

- **[AML-Only Processing](./api/AML_only_transaction_processing.md)** — run
  standalone AML/sanctions screening on a person, company, or blockchain address
  without the full document-and-selfie flow.
- **[Password Reset](./api/password_reset.md)** — request and confirm a password
  reset for your account.

## The Netki Compliance dashboard

Many of these tasks — reviewing transactions and managing your callback
configuration among them — can also be done in the Netki Compliance dashboard at
[https://compliance.netki.com](https://compliance.netki.com). Changing settings
in either the dashboard or the API requires Admin rights on your account.
