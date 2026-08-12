# Netki API Reference

Client-facing reference for the Netki OnboardID API. All endpoints are served
from `https://kyc.myverify.info`.

> [!NOTE]
> For the full, described overview — what each document covers, how the pieces
> fit together, and where to start — see [OnboardID API](../onboarid-api.md).
> This page is just a quick index of the reference documents in this folder.

## Documents

| Document | Purpose |
|---|---|
| [Conventions](./conventions.md) | Cross-cutting rules for every endpoint |
| [Authentication](./authentication.md) | JWT login and token refresh |
| [Businesses](./businesses.md) | List businesses and their app configuration |
| [Access Codes](./access_codes.md) | Access code lifecycle |
| [Polling](./polling.md) | Poll transactions and results |
| [Custom Dashboard](./custom_dashboard.md) | Manually approve, fail, or restart transactions |
| [Callbacks](./callbacks2.md) | Webhook payloads and delivery |
| [Callback Best Practices](./best_practices_internal_callbacks.md) | Recommended callback handling |
| [Deep Link Access](./howto_deeplinks_access.md) | Deep-link access flow |
| [AML-Only Processing](./AML_only_transaction_processing.md) | Standalone AML screening |
| [Password Reset](./password_reset.md) | Password reset endpoints |
| [API Error Codes](./api_error_codes.md) | Error code registry |
