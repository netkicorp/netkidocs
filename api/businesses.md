# Businesses

Your account is associated with one or more businesses. This endpoint returns
those businesses — including any corporate investor businesses — along with the
app configuration (`app_context`) for each one. Save the `id` of the business
you need: it is used by access code and other business-scoped endpoints.

## Endpoints

| Method | Path | Purpose |
|---|---|---|
| `GET` | `/api/business/businesses/` | List the businesses associated with your account |
| `GET` | `/api/business/businesses/{id}/` | Retrieve a single business by its identifier |

## Authentication

> [!NOTE]
> These endpoints require the `Authorization` header described in
> [API Conventions](./conventions.md#authentication).

## List businesses

`GET /api/business/businesses/`

Returns the businesses associated with your account, including any corporate
investor businesses, each with its `app_context`.

**Path / query parameters**

None.

**Request**

```bash
curl -X "GET" "https://kyc.myverify.info/api/business/businesses/" \
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
      "id": "604e1738-4716-4bdd-867b-4942186b1e1c",
      "name": "Netki Testing",
      "status": "open",
      "business_type": "exchange",
      "email": null,
      "country_code": null,
      "ein": null,
      "is_investor_business": false,
      "is_active": true,
      "created": "2026-04-27T18:29:02.798424Z",
      "updated": "2026-04-27T18:31:50.661200Z",
      "parent_business": null,
      "primary_account": "8f14e45f-ceea-467e-adc1-0d4e8e6b3e5a",
      "app_context": {
        "id": 7,
        "business": "604e1738-4716-4bdd-867b-4942186b1e1c",
        "logo_dark": "https://<your-bucket>.s3.amazonaws.com/businesses/assets/logo.png",
        "logo_light": "https://<your-bucket>.s3.amazonaws.com/businesses/assets/logo.png",
        "redirect_backlink": null,
        "liveness_algorithm": "default",
        "requires_multiple_taxids": false,
        "access_code_prefix": "nkt",
        "preferred_restart_contact_method": "sms",
        "required_fields": [
          {
            "id": 4,
            "name": "is_accredited_investor",
            "data_type": "list",
            "regex": null,
            "keypad": "list",
            "label": "Are you an Accredited Investor (US Only)",
            "description": "Are you an Accredited Investor (US Only)",
            "language_code": "en",
            "created": "2026-04-16T22:26:35.233747Z",
            "updated": "2026-04-16T22:26:35.258242Z",
            "is_active": true,
            "options": [
              {
                "id": 3,
                "key": "true",
                "position": 0,
                "label": "Yes",
                "language_code": "en",
                "created": "2026-04-16T22:26:35.268006Z",
                "updated": "2026-04-16T22:26:35.283482Z",
                "is_active": true
              },
              {
                "id": 4,
                "key": "false",
                "position": 0,
                "label": "No",
                "language_code": "en",
                "created": "2026-04-16T22:26:35.268006Z",
                "updated": "2026-04-16T22:26:35.283482Z",
                "is_active": true
              }
            ]
          }
        ],
        "accredited_investor_flow": "accredited_vi",
        "has_aml_provider": true,
        "welcome_message": "Welcome to MyVerify!",
        "completed_message": "Thanks for signing up!",
        "sms_verification_message": null,
        "sms_corporate_onboard_message": null,
        "declined_feedback_sms": null,
        "sms_accredited_message": null,
        "invalid_access_code_message": "Access code {access_code} is invalid or has been used. Please contact support.",
        "language_code": "en",
        "created": "2026-04-27T18:33:17.657524Z",
        "updated": "2026-04-27T18:33:30.955998Z",
        "is_active": true
      },
      "business_addresses": [],
      "business_documents": [],
      "business_media_references": [],
      "business_data_sources": [],
      "business_data_listings": [],
      "business_json_objects": [],
      "business_metadata": null,
      "business_notes": [],
      "errors": []
    }
  ]
}
```

The envelope (`count`, `next`, `previous`, `results`) follows the standard
list format described in [API Conventions](./conventions.md#pagination).

**Response fields**

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | Business identifier |
| `name` | string | Business name |
| `status` | string | One of `open`, `hold`, `rejected`, `accepted` |
| `business_type` | string \| null | One of `exchange`, `ico`, `investor`, `bank` |
| `email` | string \| null | Business contact email |
| `country_code` | string \| null | ISO 3166-1 alpha-2 country code |
| `ein` | string \| null | Business tax identification number |
| `is_investor_business` | boolean | Whether this business represents a corporate investor |
| `is_active` | boolean | Whether the business record is active |
| `created` | string (ISO-8601) | When the business was created |
| `updated` | string (ISO-8601) | When the business was last updated |
| `parent_business` | string (UUID) \| null | Identifier of the parent business, if this is a corporate investor |
| `primary_account` | string (UUID) | Identifier of the primary user account for this business |
| `app_context` | object | App configuration for this business (see below) |
| `business_addresses` | array | Addresses on file for this business |
| `business_documents` | array | Documents on file for this business |
| `business_media_references` | array | Media assets on file for this business |
| `business_data_sources` | array | AML/sanctions data sources associated with this business |
| `business_data_listings` | array | AML/sanctions data listing flags for this business |
| ~~`business_json_objects`~~ | array | **Legacy — not actively used, and may be removed in a future version.** Free-form JSON blobs attached by your workflow configuration |
| `business_metadata` | object \| null | Aggregate transaction/hit counters for this business |
| `business_notes` | array | Notes attached to this business |
| `errors` | array | Errors associated with this business |

**`app_context` fields**

| Field | Type | Description |
|---|---|---|
| `id` | integer | App context identifier |
| `business` | string (UUID) | Business this app context belongs to |
| ~~`logo_dark`~~ | string (URL) \| null | **Legacy — not actively used, and may be removed in a future version.** Logo shown on dark backgrounds in the verification app |
| `logo_light` | string (URL) \| null | Logo shown in the verification app |
| `redirect_backlink` | string \| null | URL to redirect to after the flow completes |
| `liveness_algorithm` | string | Liveness detection algorithm in use, e.g. `default` |
| `requires_multiple_taxids` | boolean | Whether both TIN and SSN are collected |
| ~~`access_code_prefix`~~ | string \| null | **Legacy — no longer used, and may be removed in a future version.** Prefix applied to access codes for this business |
| `preferred_restart_contact_method` | string | One of `sms`, `email`, `both`. Defaults to `sms` |
| `required_fields` | array | Custom fields collected during onboarding (see below) |
| `accredited_investor_flow` | string | Which accredited investort flow to use.  `webform`, `accredited_vi` |
| `has_aml_provider` | boolean | Whether an AML/sanctions provider is configured for this business |
| `welcome_message` | string | Welcome text shown in the verification app |
| `completed_message` | string | Completion text shown in the verification app |
| `sms_verification_message` | string \| null | Custom phone verification SMS text |
| `sms_corporate_onboard_message` | string \| null | Custom corporate onboarding SMS text |
| `declined_feedback_sms` | string \| null | Custom SMS text sent on a declined outcome |
| `sms_accredited_message` | string \| null | Custom SMS text for accredited investor flow |
| `invalid_access_code_message` | string | Message shown when an access code is invalid or used |
| `language_code` | string | Language of the returned translated text |
| `created` | string (ISO-8601) | When the app context was created |
| `updated` | string (ISO-8601) | When the app context was last updated |
| `is_active` | boolean | Whether the app context is active |

Each entry in `required_fields` describes a custom onboarding field:

| Field | Type | Description |
|---|---|---|
| `id` | integer | Field identifier |
| `name` | string | Field name |
| `data_type` | string | One of `string`, `integer`, `float`, `date`, `datetime`, `list` |
| `regex` | string \| null | Validation pattern, if any |
| `keypad` | string \| null | Input keypad hint for the app |
| `label` | string | Display label |
| `description` | string | Display description |
| `language_code` | string | Language of the returned translated text |
| `options` | array | For `list` fields, the selectable options (each with `id`, `key`, `position`, `label`, `language_code`) |

**Errors**

| Status | Code | Meaning |
|---|---|---|
| `401` | `not_authenticated` | No `Authorization` header was sent |
| `401` | `token_not_valid` | Access token is expired, invalid, or malformed |

## Retrieve a business

`GET /api/business/businesses/{id}/`

Returns a single business by its identifier. The object has the same shape as
one entry in the `results` array of the list response above.

**Path / query parameters**

| Parameter | In | Required | Description |
|---|---|---|---|
| `id` | path | yes | The business identifier (UUID) |

**Request**

```bash
curl -X "GET" "https://kyc.myverify.info/api/business/businesses/604e1738-4716-4bdd-867b-4942186b1e1c/" \
     -H 'Authorization: Bearer eyJ...<truncated>'
```

**Response `200`**

Returns a single business object — see the
[list response fields](#list-businesses) above for the full field-by-field
breakdown.

**Errors**

| Status | Code | Meaning |
|---|---|---|
| `401` | `not_authenticated` | No `Authorization` header was sent |
| `401` | `token_not_valid` | Access token is expired, invalid, or malformed |
| `404` | — | No business with that identifier exists on your account |
