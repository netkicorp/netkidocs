# Transactions

Every person who goes through your KYC flow gets a transaction. A
transaction is the master record of that person's verification: their
identity data, uploaded documents, AML/sanctions screening results, and
(for accredited-investor deals) their investor-verification status. Poll
these endpoints to track a transaction's progress, review why something
is on hold or failed, and move a transaction to a new state when your
compliance team makes a decision.

## Endpoints

| Method | Path | Purpose |
|---|---|---|
| `GET` | `/api/transactions/` | List transactions for your business |
| `GET` | `/api/transactions/{id}/` | Retrieve a single transaction |
| `PUT` | `/api/transactions/{id}/make-completed/` | Manually approve a transaction |
| `PUT` | `/api/transactions/{id}/make-failed/` | Manually fail a transaction |
| `PUT` | `/api/transactions/{id}/make-restarted/` | Restart a transaction and issue the customer a new access code |

## Authentication

> [!NOTE]
> All endpoints on this page require the `Authorization` header described in
> [API Conventions](./conventions.md#authentication).

## List transactions

`GET /api/transactions/`

Returns the transactions belonging to your business, most recently created
first. This is the primary way to poll for status changes: run this
periodically (or in response to a callback) and check each transaction's
`state`.

**Path / query parameters**

| Name | In | Type | Description |
|---|---|---|---|
| `search` | query | string | Case-insensitive substring match against the transaction ID, the identity ID, the identity's first or last name, or the identity's phone number. This does **not** match access codes — see [Access Codes](./access_codes.md) to look up a transaction by access code |
| `client_id` | query | string (UUID) | Filter to an exact business ID (useful for staff/parent-business accounts that can see child businesses) |
| `state` | query | string | Filter to an exact transaction state (see [Transaction states](#transaction-states) below) |
| `states` | query | string | Comma-separated list of states; returns transactions matching any of them |
| `transaction_identity` | query | string (UUID) | Filter to an exact identity ID |
| `transaction_identity__country_code` | query | string | Filter to an exact identity country code |
| `created_by` | query | integer | Filter to the internal user/account that created the transaction |
| `first_name` / `last_name` / `full_name` | query | string | Case-insensitive partial match against the identity's name |
| `client_guid` | query | string | Case-insensitive partial match against the GUID you supplied when creating the transaction |
| `phonenumber` | query | string | Case-insensitive partial match against the identity's phone number |
| `created_on` | query | string (date) | Return only transactions created on this exact date |
| `start_date` / `end_date` | query | string (date) | Return transactions created within this date range |
| `ordering` | query | string | One of `created`, `updated`, `state`, or several `transaction_identity__*` fields; prefix with `-` for descending. Defaults to `-created` |
| `page` | query | integer | Page number |
| `page_size` | query | integer | Results per page (default 10, maximum 100) |

**Request**

```bash
curl -X "GET" "https://kyc.myverify.info/api/transactions/" \
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
      "id": "83d0d0b0-68e8-4746-8197-ca4d18a21e2c",
      "client": "604e1738-4716-4bdd-867b-4942186b1e1c",
      "state": "hold",
      "phase": "kyc",
      "completed_by": null,
      "notes": null,
      "is_active": true,
      "created": "2026-06-30T14:22:10.104300Z",
      "updated": "2026-07-01T09:05:44.881200Z",
      "contenttype": 29,
      "transaction_identity": {
        "id": "f6ee3bb3-8955-4b6a-b012-f75caa0de364",
        "first_name": "Jane",
        "last_name": "Doe",
        "middle_name": null,
        "alias": null,
        "country_code": "US",
        "selected_country_code": "US",
        "locale": null,
        "state": "completed",
        "is_active": true,
        "liveness_score": 0.94,
        "death_date": null,
        "birth_location": null,
        "status": "unknown",
        "client_guid": "your-internal-guid-123",
        "birth_date": "1990-04-12",
        "gender": null,
        "height": null,
        "weight": null,
        "eye_color": null,
        "hair_color": null,
        "investor_type": "private_party",
        "ssn": "6789",
        "medical_license": null,
        "insurance_license": null,
        "drivers_license": "D1234567",
        "passport_number": null,
        "resident_number": null,
        "is_accredited_investor": true,
        "title": null,
        "ownership_percentage": null,
        "notes": "",
        "source_of_wealth": null,
        "tax_id": null,
        "phone_is_validated": true,
        "geolocation_point": null,
        "transaction": "83d0d0b0-68e8-4746-8197-ca4d18a21e2c",
        "business": "604e1738-4716-4bdd-867b-4942186b1e1c",
        "created": "2026-06-30T14:22:10.093394Z",
        "updated": "2026-07-01T09:05:44.836752Z",
        "contenttype": 30,
        "identity_emails": [
          {
            "id": "f2a172cc-6716-462e-89f0-3468dec53721",
            "created": "2026-06-30T14:22:10.139181Z",
            "updated": "2026-06-30T14:22:10.139198Z",
            "is_active": true,
            "email": "jane.doe@example.com",
            "identity": "f6ee3bb3-8955-4b6a-b012-f75caa0de364"
          }
        ],
        "identity_phone_numbers": [
          {
            "id": "1d93d2a2-8ddc-43bc-9c93-03126d9071db",
            "created": "2026-06-30T14:22:10.166477Z",
            "updated": "2026-06-30T14:22:10.166477Z",
            "is_active": true,
            "phone_number": "+12345550100",
            "identity": "f6ee3bb3-8955-4b6a-b012-f75caa0de364"
          }
        ],
        "identity_addresses": [
          {
            "id": "734fa958-f604-4558-a3f7-fde62d6d4617",
            "created": "2026-06-30T14:22:10.118040Z",
            "updated": "2026-06-30T14:22:10.118059Z",
            "is_active": true,
            "address": "123 Main St",
            "unit": "",
            "city": "Austin",
            "state": "TX",
            "postalcode": "78701",
            "score": 0,
            "country_code": "US",
            "identity": "f6ee3bb3-8955-4b6a-b012-f75caa0de364"
          }
        ],
        "identity_documents": [
          {
            "id": "b97274d3-47f9-4a0d-a0ec-5e8d20c30318",
            "created": "2026-06-30T14:24:56.066101Z",
            "updated": "2026-06-30T14:24:56.074392Z",
            "is_active": true,
            "identity": "f6ee3bb3-8955-4b6a-b012-f75caa0de364",
            "document": "https://<your-bucket>.s3.amazonaws.com/identities/documents/b12345d3-47f9-4a0d-a0ec-5e8d20c12345.selfie.jpg",
            "document_type": "drivers_license",
            "country_code": "US",
            "expiration_date": "2028-04-12",
            "issue_date": "2020-04-12",
            "state": "completed",
            "document_classification": 4021,
            "reviewer": null,
            "is_reviewed": false,
            "reviewed_date": null,
            "contenttype": 26,
            "mime_type": {
              "id": 3,
              "media_type": "image",
              "extension": "jpg",
              "mime_type": "image/jpeg"
            },
            "identity_document_thumbnail": [
              {
                "id": "0c9a3b1d-1e2f-4a3b-9c4d-5e6f7a8b9c0d",
                "name": "b97274d3-47f9-4a0d-a0ec-5e8d20c30318",
                "image": {
                  "full_size": "https://<your-bucket>.s3.amazonaws.com/identities/documents/thumbs/b97274d3.jpg",
                  "thumbnail": "https://<your-bucket>.s3.amazonaws.com/identities/documents/thumbs/b97274d3.thumbnail__200x200.jpg",
                  "medium_square_crop": "https://<your-bucket>.s3.amazonaws.com/identities/documents/thumbs/b97274d3.crop__400x400.jpg",
                  "small_square_crop": "https://<your-bucket>.s3.amazonaws.com/identities/documents/thumbs/b97274d3.crop__50x50.jpg"
                }
              }
            ],
            "errors": []
          }
        ],
        "identity_data_sources": [
          {
            "id": "a3e07f08-1d56-4e53-bd41-ff1f4fdfa1fe",
            "created": "2026-06-30T14:26:28.984376Z",
            "updated": "2026-06-30T14:27:37.684915Z",
            "is_active": true,
            "identity": "f6ee3bb3-8955-4b6a-b012-f75caa0de364",
            "raw_data": {
              "face_match_score": "94"
            },
            "reference_url": "",
            "comply_search_matches": 1,
            "score": "62",
            "is_reviewed": false,
            "reviewed_date": null,
            "data_provider": {
              "id": 3,
              "data_provider_type": {
                "id": 2,
                "type_description": "Sanctions and Watchlist Screening",
                "identifier": "aml"
              }
            },
            "aml_source_notes": [
              {
                "name": "OFAC SDN List",
                "identifier": "OFAC-SDN-4471",
                "provider_id": "4471",
                "aml_source_countries": [
                  { "country": "US", "provider_id": "4471" }
                ],
                "listing_started": "2019-02-01T00:00:00Z",
                "listing_ended": null,
                "related_url": ""
              }
            ]
          }
        ],
        "identity_data_listings": [
          {
            "id": "feb42133-7737-4a87-95a4-133ad8836303",
            "created": "2026-06-30T14:27:57.819137Z",
            "updated": "2026-06-30T14:27:57.872607Z",
            "is_active": true,
            "notes": null,
            "identity": "f6ee3bb3-8955-4b6a-b012-f75caa0de364",
            "provider_id": "4471",
            "reviewer": null,
            "is_reviewed": false,
            "reviewed_date": null,
            "listing_type": {
              "id": 11,
              "created": "2026-01-18T21:22:41.783260Z",
              "updated": "2026-01-18T21:22:41.783330Z",
              "is_active": true,
              "name": "sanctions",
              "description": "",
              "is_flagged": true
            }
          }
        ],
        "identity_media_references": [],
        "identity_access_code": null,
        "identity_accredited_investor_status": {
          "id": "999b679e-5b79-42c1-b68e-cae3a2559fca",
          "created": "2026-06-30T14:30:10.632229Z",
          "updated": "2026-06-30T14:30:10.647964Z",
          "is_active": true,
          "status": "open",
          "vendor_status": "waiting_for_investor_acceptance",
          "message": "The verification process is waiting for the investor to start.",
          "identity": {
            "id": "f6ee3bb3-8955-4b6a-b012-f75caa0de364",
            "transaction": "83d0d0b0-68e8-4746-8197-ca4d18a21e2c",
            "first_name": "Jane",
            "last_name": "Doe"
          },
          "identity_document_accreditation_status": [],
          "errors": [],
          "raw_data": {
            "id": 1876,
            "status": "waiting_for_investor_acceptance",
            "message": "The verification process is waiting for the investor to start.",
            "investor": { "id": 1559 },
            "deal_name": null,
            "created_at": "2026-06-30T07:30:10.451-07:00",
            "legal_name": "JANE DOE",
            "portal_name": "MyVerify",
            "webhook_url": "https://kyc.myverify.info/verify-investor/callback/",
            "investor_url": "https://kyc.myverify.info/investor/verification-requests/1876/accept",
            "redirect_url": "https://kyc.myverify.info/verify-investor/complete/",
            "internal_status": "open",
            "waiting_for_info": false,
            "verified_expires_at": null
          }
        },
        "identity_json_objects": [],
        "declined_feedback_texts": [],
        "errors": []
      },
      "transaction_callbacks": [
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
      ],
      "transaction_notes": [
        {
          "id": 8842,
          "created": "2026-07-01T09:05:44.700000Z",
          "updated": "2026-07-01T09:05:44.700000Z",
          "is_active": true,
          "note": "Hold Reason: Sanctions screening returned a possible match.",
          "transaction": "83d0d0b0-68e8-4746-8197-ca4d18a21e2c",
          "created_by": null
        }
      ],
      "transaction_declined_reasons": [],
      "required_fields": [],
      "errors": []
    }
  ]
}
```

The envelope (`count`, `next`, `previous`, `results`) follows the standard
list format described in [API Conventions](./conventions.md#pagination).

### Transaction fields (top level)

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | Transaction identifier |
| `client` | string (UUID) | The business this transaction belongs to |
| `state` | string | Current transaction state, see [Transaction states](#transaction-states) |
| `phase` | string | Current processing phase, see [Transaction phases](#transaction-phases) |
| `completed_by` | string \| null | `system` if an automated process last changed the state, `user` if a person did |
| ~~`notes`~~ | string \| null | **Legacy — replaced by the `transaction_notes` array below, and may be removed in a future version.** Free-text notes on the transaction |
| `is_active` | boolean | Whether the transaction record is active |
| `created` | string (ISO-8601) | When the transaction was created |
| `updated` | string (ISO-8601) | When the transaction was last modified |
| `contenttype` | integer | Internal reference identifier; not needed for integration |
| `transaction_identity` | object \| null | The identity going through verification, see [transaction_identity fields](#transaction_identity-fields) |
| `transaction_callbacks` | array | Callback delivery attempts for this transaction, see [transaction_callbacks fields](#transaction_callbacks-fields) |
| `transaction_notes` | array | Timestamped notes recorded as the transaction changed state, see [transaction_notes fields](#transaction_notes-fields) |
| `transaction_declined_reasons` | array | Structured decline reasons attached to a failed transaction, see [transaction_declined_reasons fields](#transaction_declined_reasons-fields) |
| `required_fields` | array | Extra workflow fields your business has configured, see [required_fields fields](#required_fields-fields) |
| `errors` | array | Actionable errors on the transaction itself, see [errors fields](#errors-fields) |

### `transaction_identity` fields

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | Identity identifier |
| `first_name` / `last_name` / `middle_name` / `alias` | string \| null | Name fields as captured or provided |
| `country_code` | string \| null | ISO alpha-2 country code from the identity document |
| `selected_country_code` | string \| null | ISO alpha-2 country code the customer selected in the flow |
| `locale` | string \| null | Locale the customer used |
| ~~`state`~~ | string | **Legacy — not actively used, and may be removed in a future version.** Present in responses but not maintained. Identity-level state: `new`, `processing`, `failed`, `completed`, or `expired` |
| `is_active` | boolean | Whether the identity record is active |
| `liveness_score` | number | Selfie liveness score |
| `death_date` | string (date) \| null | Set if a data source flags the person as deceased |
| `birth_location` | string \| null | Birth location, if captured |
| ~~`status`~~ | string | **Legacy — not actively used, and may be removed in a future version.** Present in responses but not maintained. Free-form identity status label |
| `client_guid` | string \| null | The GUID you supplied when creating the transaction |
| `birth_date` | string (date) \| null | Date of birth |
| `gender` | string \| null | `male`, `female`, `other`, or `unknown` |
| `height` / `weight` / `eye_color` / `hair_color` | string \| null | Physical description fields from the document, where available |
| `investor_type` | string \| null | `private_party` or `corporate_entity` |
| `ssn` | string \| null | Last 4 digits only; the API masks the rest |
| `medical_license` / `insurance_license` / `drivers_license` / `passport_number` / `resident_number` | string \| null | ID numbers captured from documents, when applicable |
| `is_accredited_investor` | boolean | Whether this identity is going through (or has completed) accredited-investor verification |
| `title` | string \| null | Job title, for corporate-entity signers |
| `ownership_percentage` | integer \| null | Ownership percentage, for corporate-entity signers |
| `notes` | string \| null | Free-text notes on the identity |
| `source_of_wealth` | string \| null | Captured for some accredited-investor workflows |
| `tax_id` | string \| null | Captured for some corporate-entity workflows |
| `phone_is_validated` | boolean | Whether the phone number passed validation |
| `geolocation_point` | object \| null | `{ "longitude": <number>, "latitude": <number> }` if a location was captured |
| `transaction` | string (UUID) | The transaction this identity belongs to |
| `business` | string (UUID) | The business this identity belongs to |
| `created` / `updated` | string (ISO-8601) | Timestamps |
| `contenttype` | integer | Internal reference identifier; not needed for integration |
| `identity_emails` | array | See [identity_emails fields](#identity_emails-fields) |
| `identity_phone_numbers` | array | See [identity_phone_numbers fields](#identity_phone_numbers-fields) |
| `identity_addresses` | array | See [identity_addresses fields](#identity_addresses-fields) |
| `identity_documents` | array | Uploaded documents, see [identity_documents fields](#identity_documents-fields) |
| `identity_data_sources` | array | AML/screening results, see [identity_data_sources fields](#identity_data_sources-fields) |
| `identity_data_listings` | array | Sanction/PEP/adverse-media listing flags, see [identity_data_listings fields](#identity_data_listings-fields) |
| `identity_media_references` | array | Adverse-media mentions found during screening, see [identity_media_references fields](#identity_media_references-fields) |
| `identity_access_code` | object \| null | The access code this customer used, see [Access Codes](./access_codes.md) for the shape |
| `identity_accredited_investor_status` | object \| null | Present only for accredited-investor workflows, see [Accredited-investor status](#accredited-investor-status) |
| ~~`identity_json_objects`~~ | array | **Legacy — not actively used, and may be removed in a future version.** Free-form JSON blobs attached by your workflow configuration, see [identity_json_objects fields](#identity_json_objects-fields) |
| `declined_feedback_texts` | array | SMS messages sent to the customer when a restart was triggered, see [declined_feedback_texts fields](#declined_feedback_texts-fields) |
| `errors` | array | Actionable errors on this identity, see [errors fields](#errors-fields) |

### `identity_emails` fields

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | Email record identifier |
| `email` | string | Email address |
| `identity` | string (UUID) | Parent identity |
| `created` / `updated` | string (ISO-8601) | Timestamps |
| `is_active` | boolean | Whether this record is active |

### `identity_phone_numbers` fields

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | Phone record identifier |
| `phone_number` | string | Phone number in E.164 format |
| `identity` | string (UUID) | Parent identity |
| `created` / `updated` | string (ISO-8601) | Timestamps |
| `is_active` | boolean | Whether this record is active |

### `identity_addresses` fields

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | Address record identifier |
| `address` / `unit` / `city` / `state` / `postalcode` | string \| null | Address components |
| `country_code` | string \| null | ISO alpha-2 country code |
| `score` | integer | Internal ranking used when more than one address is on file |
| `identity` | string (UUID) | Parent identity |
| `created` / `updated` | string (ISO-8601) | Timestamps |
| `is_active` | boolean | Whether this record is active |

### `identity_documents` fields

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | Document identifier |
| `document` | string (URL) | Location of the uploaded document image |
| `document_type` | string | Document type, for example `drivers_license`, `passport`, or `selfie` |
| `country_code` | string \| null | ISO alpha-2 country code the document was issued in |
| `expiration_date` / `issue_date` | string (date) \| null | Dates read from the document, if present |
| `state` | string | Document processing state: `new`, `pending`, `processing`, `completed`, `failed`, `disabled`, or `quarantined` |
| `document_classification` | integer \| null | Internal reference to the recognized document type |
| `reviewer` | integer \| null | Internal user ID of whoever manually reviewed the document, if any |
| `is_reviewed` | boolean | Whether a manual review has occurred |
| `reviewed_date` | string (ISO-8601) \| null | When the manual review happened |
| `identity` | string (UUID) | Parent identity |
| `contenttype` | integer | Internal reference identifier; not needed for integration |
| `mime_type` | object \| null | `{ "id", "media_type", "extension", "mime_type" }` describing the file format |
| `identity_document_thumbnail` | array | Generated thumbnails: each entry has `id`, `name`, and an `image` object with `full_size`, `thumbnail`, `medium_square_crop`, and `small_square_crop` URLs |
| `errors` | array | Actionable errors on this document, see [errors fields](#errors-fields) |

### `identity_data_sources` fields

Represents one screening/AML check result.

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | Record identifier |
| `raw_data` | object \| null | Free-form data from the screening check; shape varies by check type |
| `reference_url` | string | Reference link for the check, when one applies |
| `comply_search_matches` | integer | Number of screening matches found |
| `score` | string \| null | Confidence score for the check |
| `is_reviewed` | boolean | Whether a compliance reviewer has looked at this |
| `reviewed_date` | string (ISO-8601) \| null | When it was reviewed |
| `identity` | string (UUID) | Parent identity |
| `data_provider` | object \| null | `{ "id", "data_provider_type": { "id", "type_description", "identifier" } }` |
| `aml_source_notes` | array | See [aml_source_notes fields](#aml_source_notes-fields) |
| `aml_hits` | array | Present when a screening hit was returned; see [aml_hits fields](#aml_hits-fields) |
| `created` / `updated` | string (ISO-8601) | Timestamps |
| `is_active` | boolean | Whether this record is active |

### `aml_source_notes` fields

| Field | Type | Description |
|---|---|---|
| `name` | string | Name of the watchlist or source that produced the note |
| `identifier` | string | Source-assigned identifier |
| `provider_id` | string | Screening-provider reference ID |
| `aml_source_countries` | array | `{ "country": "<alpha-2>", "provider_id" }` entries |
| `listing_started` / `listing_ended` | string (ISO-8601) \| null | When the listing was active |
| `related_url` | string | Reference URL; blank when no public reference applies |

### `aml_hits` fields

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | Hit identifier |
| `name` | string | Name matched by the screening check |
| `score` | number \| null | Match confidence score |
| `provider_id` | string | Screening-provider reference ID |
| `identity_aml_hit_match_types` | array | `{ "name" }` — the type(s) of match (for example sanctions, PEP, adverse media) |
| `identity_aml_hit_assets` | array | `{ "public_url", "asset_type", "source", "provider_id" }` — supporting media for the hit |
| `aml_hit_info` | array | `{ "feed", "name", "source", "occupation", "nationality", "organization" }` — biographical detail on the matched entity |
| `aml_sources` | array | `{ "name", "source_type", "listing_started", "listing_ended", "country", "source_urls": [{ "title", "url" }] }` |

### `identity_data_listings` fields

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | Listing identifier |
| `provider_id` | string | Screening-provider reference ID |
| `notes` | string \| null | Reviewer notes |
| `reviewer` | string \| null | Username of whoever reviewed this listing |
| `is_reviewed` | boolean | Whether it has been reviewed |
| `reviewed_date` | string (ISO-8601) \| null | When it was reviewed |
| `identity` | string (UUID) | Parent identity |
| `listing_type` | object | `{ "id", "name", "description", "is_flagged", "created", "updated", "is_active" }` — the category of listing (for example a sanctions or adverse-media flag) |
| `created` / `updated` | string (ISO-8601) | Timestamps |
| `is_active` | boolean | Whether this record is active |

### `identity_media_references` fields

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | Record identifier |
| `activity_date` | string (ISO-8601) | Date of the media mention |
| `provider_id` | string | Screening-provider reference ID |
| `source` | string | Source reference (often a URL) |
| `title` | string | Headline of the media mention |
| `content` | string | Snippet of the media mention |
| `reviewed_by` | integer \| null | Internal user ID of whoever reviewed this mention |
| `reviewed_date` | string (ISO-8601) \| null | When it was reviewed |
| `identity` | string (UUID) | Parent identity |
| `created` / `updated` | string (ISO-8601) | Timestamps |
| `is_active` | boolean | Whether this record is active |

### `identity_json_objects` fields

> [!NOTE]
> Legacy — this field is not actively used and may be removed in a future
> version. It is still returned for backward compatibility.

| Field | Type | Description |
|---|---|---|
| `id` | integer | Record identifier |
| `data` | object | Free-form JSON payload defined by your workflow configuration |
| `identity` | string (UUID) | Parent identity |
| `created` / `updated` | string (ISO-8601) | Timestamps |
| `is_active` | boolean | Whether this record is active |

### `declined_feedback_texts` fields

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | Record identifier |
| `message` | string | Text of the SMS sent to the customer |

## Accredited-investor status

For accredited-investor deals, `transaction_identity.identity_accredited_investor_status`
tracks the customer's progress through investor verification. It is `null`
until the identity enters that workflow.

### `identity_accredited_investor_status` fields

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | Record identifier |
| `status` | string | `open`, `uploaded`, `hold`, `expired`, `rejected`, or `accepted` |
| `vendor_status` | string \| null | Status label from the investor-verification workflow, informational only |
| `message` | string \| null | Human-readable summary of `status` |
| `identity` | object | `{ "id", "transaction", "first_name", "last_name" }` — abbreviated identity reference |
| `identity_document_accreditation_status` | array | Accreditation documents the customer has uploaded, see below |
| `errors` | array | Actionable errors on the accreditation, see [errors fields](#errors-fields) |
| `raw_data` | object \| null | Informational detail relayed from the investor-verification workflow (see below); not guaranteed to be present or stable |
| `created` / `updated` | string (ISO-8601) | Timestamps |
| `is_active` | boolean | Whether this record is active |

**`identity_document_accreditation_status` entries**

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | Record identifier |
| `status` | string | `uploaded`, `verified`, or `invalid` |
| `accredited_status` | string (UUID) | The parent accreditation record |
| `document` | object \| null | The uploaded document, in the same shape as [identity_documents](#identity_documents-fields) |

**`raw_data` keys you may see**

`raw_data` is relayed as-is from the investor-verification workflow, so its
exact shape can vary. Keys commonly present include:

| Key | Type | Description |
|---|---|---|
| `status` | string | Vendor-side status label |
| `message` | string | Human-readable status message |
| `legal_name` | string | Legal name on file with the investor-verification workflow |
| `deal_name` | string \| null | Name of the deal being verified for, if applicable |
| `investor_url` | string (URL) | The link your customer visits to complete their investor verification, for example `https://kyc.myverify.info/investor/verification-requests/1876/accept` |
| `webhook_url` | string (URL) | Netki-hosted callback endpoint used internally during processing, for example `https://kyc.myverify.info/verify-investor/callback/` |
| `redirect_url` | string (URL) | Page the customer's browser returns to after finishing the investor-verification workflow. Netki-hosted when Netki hosts the accredited-investor form (the usual setup), for example `https://kyc.myverify.info/verify-investor/complete/`; if you host that form yourself, this is your own URL |
| `waiting_for_info` | boolean | Whether the workflow is waiting on more information |
| `verified_expires_at` | string (ISO-8601) \| null | When a completed verification expires, if applicable |

`webhook_url` is a Netki endpoint used internally during processing — you do
not call it yourself. `redirect_url` is where the customer's browser lands
after verification: a Netki page when Netki hosts the accredited-investor form
(the usual case), or your own URL when you host that form. `investor_url` is
the link you would typically act on — for example, to resend it to your
customer.

## Retrieve a transaction

`GET /api/transactions/{id}/`

Returns a single transaction in the same shape as an entry in
[List transactions](#list-transactions) above.

**Path / query parameters**

| Name | In | Type | Description |
|---|---|---|---|
| `id` | path | string (UUID) | The transaction's `id` |

**Request**

```bash
curl -X "GET" "https://kyc.myverify.info/api/transactions/83d0d0b0-68e8-4746-8197-ca4d18a21e2c/" \
     -H 'Authorization: Bearer eyJ...<truncated>'
```

**Response `200`**

Same object shape as an entry in `results` above — see
[Transaction fields (top level)](#transaction-fields-top-level)
and the nested field tables it links to.

**Errors**

| Status | Code | Meaning |
|---|---|---|
| `401` | `not_authenticated` | No `Authorization` header was sent |
| `401` | `token_not_valid` | Access token is expired, invalid, or malformed |
| `404` | `not_found` | `id` does not exist or is not accessible to your account |

## Transaction states

`state` on the transaction (not to be confused with `transaction_identity.state`,
which uses a smaller set of values) will be one of:

| State | Meaning |
|---|---|
| `new` | Just created; transitions to `processing` almost immediately |
| `processing` | Document parsing and AML screening are underway |
| `post_processing` | Automated processing flagged something for a Netki reviewer to resolve, shown as "Netki Review" on the dashboard |
| `customer_review` | Same as `post_processing`, but for workflows where your own compliance team does the review instead of Netki |
| `hold` | Something needs a decision: low face-match score, failed liveness check, a country mismatch, or an AML/media match. Can also be set manually after a reviewer updates identity data |
| `failed` | Verification did not pass — banned document country, underage customer, blacklisted phone number, or a manual decline from `hold` |
| `completed` | Verification passed. Matches "Approved" on the dashboard |
| `canceled` | The transaction was stopped and cannot be restarted |
| `restarted` | The customer was given a new access code to redo verification after a decline. Only used by access-code (MyVerify app) customers — SDK integrations don't use this state |
| `quarantine` | Held for additional review |
| `expired` | The transaction was created but never used |
| `delete_pending` / `deleted` | The transaction is queued for or has completed data deletion |

## Transaction phases

> [!NOTE]
> `phase` reflects Netki's internal processing pipeline and is provided for
> informational purposes only. Drive your integration off the transaction
> `state` — do not rely on `phase` for any logic, as its values and behavior
> may change.

`phase` tracks where in the pipeline a transaction currently is, independent
of `state`:

| Phase | Meaning |
|---|---|
| `new` | Not yet started |
| `document` | Document capture, face match, and OCR processing |
| `identity` | AML/sanctions screening |
| `kyc` | Business-logic decisioning (expiration checks, face-match thresholds, AML review) |
| `finished` | Processing has completed for this pass (the transaction may still be on hold or otherwise need attention) |

## Mark a transaction as completed

`PUT /api/transactions/{id}/make-completed/`

> [!NOTE]
> This endpoint backs a manual compliance-dashboard action. It exists so you
> can build your own review dashboard instead of using the Netki Compliance
> dashboard. It is meant to be triggered by a person reviewing a case — do not
> use it as part of an automated workflow.

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

> [!NOTE]
> This endpoint backs a manual compliance-dashboard action. It exists so you
> can build your own review dashboard instead of using the Netki Compliance
> dashboard. It is meant to be triggered by a person reviewing a case — do not
> use it as part of an automated workflow.

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

> [!NOTE]
> This endpoint backs a manual compliance-dashboard action. It exists so you
> can build your own review dashboard instead of using the Netki Compliance
> dashboard. It is meant to be triggered by a person reviewing a case — do not
> use it as part of an automated workflow.

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

## `transaction_callbacks` fields

| Field | Type | Description |
|---|---|---|
| `id` | integer | Callback attempt identifier |
| `state` | string | `new`, `processing`, `paused`, `restarting`, `failed`, or `completed` |
| `response_raw` | string \| null | The raw response body your endpoint returned |
| `status_code` | integer \| null | The HTTP status your endpoint returned |
| `callback_duration` | string \| null | Seconds the callback attempt took |
| `callback_url` | string \| null | The URL the callback was sent to |
| `callback_counter` | integer | Attempt number for this callback |
| `transaction` | string (UUID) | Parent transaction |

See [Callbacks](./callbacks2.md) for more on how callback delivery works.

## `transaction_notes` fields

| Field | Type | Description |
|---|---|---|
| `id` | integer | Note identifier |
| `note` | string | Note text — typically records why a state change happened |
| `transaction` | string (UUID) | Parent transaction |
| `created_by` | integer \| null | Internal user ID that created the note, if any |
| `created` / `updated` | string (ISO-8601) | Timestamps |
| `is_active` | boolean | Whether this record is active |

## `transaction_declined_reasons` fields

| Field | Type | Description |
|---|---|---|
| `reason` | string | The decline reason text |

## `required_fields` fields

Extra workflow fields configured for your business (most transactions have
none, so this is usually an empty array).

| Field | Type | Description |
|---|---|---|
| `id` | integer | Field identifier |
| `name` | string | Internal field name |
| `data_type` | string | `string`, `integer`, `float`, `date`, `datetime`, or `list` |
| `label` | string | Display label |
| `description` | string | Display description |
| `regex` | string \| null | Validation pattern, if any |
| `keypad` | string \| null | Suggested input keypad, if any |
| `options` | array | For `list` fields: `{ "key", "position", "label" }` entries |
| `created` / `updated` | string (ISO-8601) | Timestamps |
| `is_active` | boolean | Whether this field is active |

## `errors` fields

Appears at the root of the transaction, on `transaction_identity`, on
individual `identity_documents` entries, and on
`identity_accredited_investor_status`. These are the actionable errors for
whatever state the containing object is in.

| Field | Type | Description |
|---|---|---|
| `id` | integer | Error record identifier |
| `object_id` | string (UUID) | The record this error is attached to |
| `content_type` | integer | Internal reference identifier for the type of record `object_id` points to; not needed for integration |
| `error_code` | object | `{ "error_code_id", "error_code_name", "rank", "category", "error_code_description" }` — see [API Error Codes](./api_error_codes.md) for the full registry |
| `created` / `updated` | string (ISO-8601) | Timestamps |
| `is_active` | boolean | Whether this error is still active |

> [!NOTE]
> These errors are written for you, not for your end users — don't pass
> them directly through to a customer-facing UI.
