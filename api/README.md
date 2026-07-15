# Netki API Reference

Client-facing reference for the Netki OnboardID API. All endpoints are served
from `https://kyc.myverify.info`.

New integrators should read in this order:

1. [API Conventions](./conventions.md) — base URL, auth, pagination, errors.
2. [Authentication](./authentication.md) — obtain and refresh your token.
3. [Access Codes](./access_codes.md) — create and manage onboarding access codes.
4. [Callbacks](./callbacks2.md) — receive transaction results via webhook.
5. [Polling](./polling.md) — pull transaction status and results.

## Documents

| Document | Purpose |
|---|---|
| [Conventions](./conventions.md) | Cross-cutting rules for every endpoint |
| [Authentication](./authentication.md) | JWT login and token refresh |
| [Access Codes](./access_codes.md) | Access code lifecycle |
| [Polling](./polling.md) | Poll transactions and results |
| [Callbacks](./callbacks2.md) | Webhook payloads and delivery |
| [Internal Callback Best Practices](./best_practices_internal_callbacks.md) | Recommended callback handling |
| [Deep Link Access](./howto_deeplinks_access.md) | Deep-link access flow |
| [AML-Only Processing](./AML_only_transaction_processing.md) | Standalone AML screening |
| [Password Reset](./password_reset.md) | Password reset endpoints |
| [API Error Codes](./api_error_codes.md) | Error code registry |
