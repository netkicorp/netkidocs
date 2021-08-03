## Information Polling

API endpoints used in this section

https://kyc.myverify.info/api/token-auth/

https://kyc.myverify.info/api/transactions/UUID/

https://kyc.myverify.info/api/transactions/UUID/make-completed/

https://kyc.myverify.info/api/transactions/UUID/make-restarted/

https://kyc.myverify.info/api/transactions/UUID/make-failed/




The core of the the KYC experience is to be able to view the current status of what's going on with your users, addressing any issues as needed, and updating the state of transactions based upon the information that you have received.

Each individual who goes through the KYC process will get a transaction ID.  This ID will have all of the details needed to track the status of the user through the system.  To get a full list of transactions in your account you will do the following:


```bash
curl -X "POST" "https://kyc.myverify.info/api/token-auth/" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -d $'{
  "username": "client_username",
  "password": "client_password"
}'
```


```bash
curl -X "GET" "https://kyc.myverify.info/api/transactions/" \
     -H 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwianRpIjoiZGJhNzQ5ZTBiZGNjNGM1NmJjMWI0NmQ1MWY4YzIzM2YiLCJleHAiOjE1MjQ4NTUwODAsInVzZXJfaWQiOjF9.tmubREi5qH2KZTBBK-Lf047gnyVllk_jNLD9qp0aesE'
```

```json
{
  "count": 1,
  "next": null,
  "previous": null,
  "results": [
    {
      "id": "83d0d0b0-68e8-4746-8197-ca4d18a21e2c",
      "client": "604e1738-4716-4bdd-867b-4942186b1e1c",
      "transaction_identity": {
        "id": "f6ee3bb3-8955-4b6a-b012-f75caa0de364",
        "identity_emails": [
          {
            "id": "f2a172cc-6716-462e-89f0-3468dec53721",
            "created": "2018-04-30T17:57:54.139181Z",
            "updated": "2018-04-30T17:57:54.139198Z",
            "is_active": true,
            "email": "test@email.com",
            "identity": "f6ee3bb3-8955-4b6a-b012-f75caa0de364"
          }
        ],
        "identity_phone_numbers": [
          {
            "id": "1d93d2a2-8ddc-43bc-9c93-03126d9071db",
            "created": "2018-04-30T17:57:54.166477Z",
            "updated": "2018-04-30T20:09:14.845833Z",
            "is_active": true,
            "phone_number": "+12345550100",
            "identity": "f6ee3bb3-8955-4b6a-b012-f75caa0de364"
          }
        ],
        "identity_addresses": [
          {
            "id": "734fa958-f604-4558-a3f7-fde62d6d4617",
            "created": "2018-04-30T17:57:54.118040Z",
            "updated": "2018-04-30T17:57:54.118059Z",
            "is_active": true,
            "address": "12345 Oompa Loompa Rd",
            "unit": "",
            "city": "12345 Oompa Loompa Rd",
            "state": "TX",
            "postalcode": "40110",
            "score": 0,
            "country_code": "US",
            "identity": "f6ee3bb3-8955-4b6a-b012-f75caa0de364"
          }
        ],
        "identity_documents": [],
        "identity_data_sources": [],
        "identity_data_listings": [],
        "identity_media_references": [],
        "identity_access_code": null,
        "identity_accredited_investor_status": {
          "id": "999b679e-5b79-42c1-b68e-cae3a2559fca",
          "identity": {
            "id": "f6ee3bb3-8955-4b6a-b012-f75caa0de364",
            "transaction": "83d0d0b0-68e8-4746-8197-ca4d18a21e2c",
            "first_name": "John",
            "last_name": "Doe"
          },
          "created": "2018-04-30T18:09:10.632229Z",
          "updated": "2018-04-30T18:09:10.647964Z",
          "is_active": true,
          "status": "open",
          "vendor_status": "waiting_for_investor_acceptance",
          "raw_data": {
            "id": 1876,
            "status": "waiting_for_investor_acceptance",
            "message": "The verification process is waiting for the investor to start.",
            "investor": {
              "id": 1559
            },
            "deal_name": null,
            "created_at": "2018-04-30T11:09:10.451-07:00",
            "legal_name": "JOHN DOE",
            "portal_name": "MyVerify",
            "webhook_url": "http://localhost:8000/verify-investor/callback/",
            "investor_url": "http://verifyinvestor-staging.herokuapp.com/investor/verification-requests/1876/accept",
            "redirect_url": "http://localhost:8000/verify-investor/complete/",
            "internal_status": "open",
            "waiting_for_info": false,
            "verified_expires_at": null
          },
          "message": "The verification process is waiting for the investor to start."
        },
        "identity_json_objects": [],
        "errors": [],
        "created": "2018-04-30T17:57:54.093394Z",
        "updated": "2018-04-30T20:09:14.836752Z",
        "first_name": "John",
        "last_name": "Doe",
        "middle_name": "Tester",
        "alias": null,
        "country_code": "US",
        "selected_country_code": "US",
        "locale": null,
        "state": "new",
        "is_active": true,
        "death_date": "2016-01-01",
        "birth_location": null,
        "status": "unknown",
        "client_guid": "tester-guid",
        "birth_date": "2001-11-22",
        "gender": null,
        "height": null,
        "weight": null,
        "eye_color": null,
        "hair_color": null,
        "investor_type": "private_party",
        "ssn": null,
        "medical_license": null,
        "insurance_license": null,
        "drivers_license": null,
        "passport_number": null,
        "is_accredited_investor": true,
        "title": null,
        "ownership_percentage": null,
        "notes": "",
        "phone_is_validated": true,
        "transaction": "83d0d0b0-68e8-4746-8197-ca4d18a21e2c",
        "business": "604e1738-4716-4bdd-867b-4942186b1e1c"
      },
      "transaction_metadata": {
        "id": 112,
        "created": "2018-04-30T17:57:55.421248Z",
        "updated": "2018-04-30T17:57:55.421268Z",
        "is_active": true,
        "country_code": "US",
        "state": "TX",
        "gender": null,
        "birth_date": null,
        "locale": "",
        "app_name": null,
        "platform": null,
        "platform_version": null,
        "app_version": null,
        "sdk_version": null,
        "data_hits": null,
        "media_hits": null,
        "face_match_score": null,
        "quality_score": null,
        "transaction": "83d0d0b0-68e8-4746-8197-ca4d18a21e2c"
      },
      "transaction_callbacks": [],
      "required_fields": [],
      "errors": [],
      "created": "2018-04-30T17:56:34.688474Z",
      "updated": "2018-04-30T17:56:34.688512Z",
      "is_active": true,
      "state": "new",
      "notes": null,
      "contettype": 29
    }
  ]
}
```

That's a lot of data for a single call!  Thankfully, you only need to worry about a few key items (the results are also paged, so if you are expecting to see a transaction in the list and don't, check the next entry):

* Inside the payload is a results object.  This is an array of JSON objects that have all of your transaction details.
* The transaction payload includes the identity associated with the transaction and all identity data include the Verified Investor status.
* The transaction data has field for "state".  Typically you will see the following states: Completed, Hold, or Failed.
* The transaction has a notes field that has details as to the current state.  If it's on hold, why?  If it failed, why?
* For Verified Investor customers, the transactions also include "identity_accredited_investor_status" which shows the current status.
* The results of this call are paged.  You'll see a count and next/previous keys in the results which show your total records and the URL to access the rest of them.

#### Transaction States
Let's talk about the transaction states a bit more.  The full list of transaction states are as follows:

* **new** - New: when a transaction is first created it will be in the new state.  You'll probably never actually see this state, as it changes to processing almost immediately after creation.
* **processing** - Processing: the transaction is being processed and we are parsing the information and checking AML to see who's been naughty or nice.
* **post_processing** - Post Processing: we found some stuff that requires some extra looking into on our side.  Typically there was a problem with parsing the information off the ID and we have someone who goes in and will update that and finish the rest of the processing.
* **hold** - Hold: if a transaction is in the hold state it means one of these things: the facial match score between the selfie and the ID image is lower than 80 (by default), the client failed the liveness tests, the country the user selected via the KYC process doesn't match their ID, there is an AML match for Media or potential sanctions.  A transaction can also go on hold after someone manually runs AML from the dashboard after updating the identity information when the automated systems aren't able to.
* **failed** - Failed: a transaction will go into a failed state if they try and use an ID from a banned country (OFAC as well as custom, client provided countries), if the birthdate of the person is below the approved age limit (if enabled), if they try to use a phone number that’s on the blacklist (*if enabled), or if someone clicks decline on the dashboard when someone is on hold.
* **completed** - Completed: Yay!  The user has passed through the AML check without any negative results and we were able to process their ID and their selfie matches the ID. This is also the same state that matched "Approved" on your dashboard.

To query transactions individually you'll want to use:

```bash
curl -X "GET" "https://kyc.myverify.info/api/transactions/83d0d0b0-68e8-4746-8197-ca4d18a21e2c/" \
    -H 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwianRpIjoiZGJhNzQ5ZTBiZGNjNGM1NmJjMWI0NmQ1MWY4YzIzM2YiLCJleHAiOjE1MjQ4NTUwODAsInVzZXJfaWQiOjF9.tmubREi5qH2KZTBBK-Lf047gnyVllk_jNLD9qp0aesE'
```

```json
{
  "id": "83d0d0b0-68e8-4746-8197-ca4d18a21e2c",
  "client": "604e1738-4716-4bdd-867b-4942186b1e1c",
  "transaction_identity": {
    "id": "f6ee3bb3-8955-4b6a-b012-f75caa0de364",
    "identity_emails": [
      {
        "id": "f2a172cc-6716-462e-89f0-3468dec53721",
        "created": "2018-04-30T17:57:54.139181Z",
        "updated": "2018-04-30T17:57:54.139198Z",
        "is_active": true,
        "email": "test@email.com",
        "identity": "f6ee3bb3-8955-4b6a-b012-f75caa0de364"
      }
    ],
    "identity_phone_numbers": [
      {
        "id": "1d93d2a2-8ddc-43bc-9c93-03126d9071db",
        "created": "2018-04-30T17:57:54.166477Z",
        "updated": "2018-04-30T20:09:14.845833Z",
        "is_active": true,
        "phone_number": "+12345550100",
        "identity": "f6ee3bb3-8955-4b6a-b012-f75caa0de364"
      }
    ],
    "identity_addresses": [
      {
        "id": "734fa958-f604-4558-a3f7-fde62d6d4617",
        "created": "2018-04-30T17:57:54.118040Z",
        "updated": "2018-04-30T17:57:54.118059Z",
        "is_active": true,
        "address": "12345 Oompa Loompa Rd",
        "unit": "",
        "city": "12345 Oompa Loompa Rd",
        "state": "TX",
        "postalcode": "40110",
        "score": 0,
        "country_code": "US",
        "identity": "f6ee3bb3-8955-4b6a-b012-f75caa0de364"
      }
    ],
    "identity_documents": [],
    "identity_data_sources": [],
    "identity_data_listings": [],
    "identity_media_references": [],
    "identity_access_code": null,
    "identity_accredited_investor_status": {
      "id": "999b679e-5b79-42c1-b68e-cae3a2559fca",
      "identity": {
        "id": "f6ee3bb3-8955-4b6a-b012-f75caa0de364",
        "transaction": "83d0d0b0-68e8-4746-8197-ca4d18a21e2c",
        "first_name": "John",
        "last_name": "Doe"
      },
      "created": "2018-04-30T18:09:10.632229Z",
      "updated": "2018-04-30T18:09:10.647964Z",
      "is_active": true,
      "status": "open",
      "vendor_status": "waiting_for_investor_acceptance",
      "raw_data": {
        "id": 1876,
        "status": "waiting_for_investor_acceptance",
        "message": "The verification process is waiting for the investor to start.",
        "investor": {
          "id": 1559
        },
        "deal_name": null,
        "created_at": "2018-04-30T11:09:10.451-07:00",
        "legal_name": "JOHN DOE",
        "portal_name": "MyVerify",
        "webhook_url": "http://localhost:8000/verify-investor/callback/",
        "investor_url": "http://verifyinvestor-staging.herokuapp.com/investor/verification-requests/1876/accept",
        "redirect_url": "http://localhost:8000/verify-investor/complete/",
        "internal_status": "open",
        "waiting_for_info": false,
        "verified_expires_at": null
      },
      "message": "The verification process is waiting for the investor to start."
    },
    "identity_json_objects": [],
    "errors": [],
    "created": "2018-04-30T17:57:54.093394Z",
    "updated": "2018-04-30T20:09:14.836752Z",
    "first_name": "John",
    "last_name": "Doe",
    "middle_name": "Tester",
    "alias": null,
    "country_code": "US",
    "selected_country_code": "US",
    "locale": null,
    "state": "new",
    "is_active": true,
    "death_date": "2016-01-01",
    "birth_location": null,
    "status": "unknown",
    "client_guid": "tester-guid",
    "birth_date": "2001-11-22",
    "gender": null,
    "height": null,
    "weight": null,
    "eye_color": null,
    "hair_color": null,
    "investor_type": "private_party",
    "ssn": null,
    "medical_license": null,
    "insurance_license": null,
    "drivers_license": null,
    "passport_number": null,
    "is_accredited_investor": true,
    "title": null,
    "ownership_percentage": null,
    "notes": "",
    "phone_is_validated": true,
    "transaction": "83d0d0b0-68e8-4746-8197-ca4d18a21e2c",
    "business": "604e1738-4716-4bdd-867b-4942186b1e1c"
  },
  "transaction_metadata": {
    "id": 112,
    "created": "2018-04-30T17:57:55.421248Z",
    "updated": "2018-04-30T17:57:55.421268Z",
    "is_active": true,
    "country_code": "US",
    "state": "TX",
    "gender": null,
    "birth_date": null,
    "locale": "",
    "app_name": null,
    "platform": null,
    "platform_version": null,
    "app_version": null,
    "sdk_version": null,
    "data_hits": null,
    "media_hits": null,
    "face_match_score": null,
    "quality_score": null,
    "transaction": "83d0d0b0-68e8-4746-8197-ca4d18a21e2c"
  },
  "transaction_callbacks": [],
  "required_fields": [],
  "errors": [],
  "created": "2018-04-30T17:56:34.688474Z",
  "updated": "2018-04-30T17:56:34.688512Z",
  "is_active": true,
  "state": "new",
  "notes": null,
  "contettype": 29
}
```

Ideally the transaction will show that the user passed KYC verification with flying colors!  However; this is not always the case.  Sometimes the user will have failed KYC or they will be placed on hold.  There can be a few different reasons for this that are outside of the scope of this document.  What we will cover is how to move people on from these states!

If someone gets placed on hold and you would like to go ahead and manually approve them you'll use the make-completed endpoint.  You'll need to make sure to include a notes field with the compliance notes as to why it was completed.  To mark a transaction as failed use make-failed instead.  We'll cover make-restarted after this example.

```bash
curl -X "POST" "https://kyc.myverify.info/api/transactions/83d0d0b0-68e8-4746-8197-ca4d18a21e2c/make-completed/" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -H 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwianRpIjoiZGJhNzQ5ZTBiZGNjNGM1NmJjMWI0NmQ1MWY4YzIzM2YiLCJleHAiOjE1MjQ4NTUwODAsInVzZXJfaWQiOjF9.tmubREi5qH2KZTBBK-Lf047gnyVllk_jNLD9qp0aesE' \
     -d $'{
  "notes": "reason for completing transaction"
}'
```

```json
{
  "message": "Transaction 83d0d0b0-68e8-4746-8197-ca4d18a21e2c moved to completed state."
}
```

The endpoint for make-restarted is almost the same as the others, just replace make-completed with make-restarted and the verb is PUT instead of POST, but the return value is different and what happens on the back end is different.  Only a transaction that is in a failed state can be restarted.  What this does is it will send the user a new SMS with a new link and a new access code to go through KYC again.  The response back for this also includes the new access code that was sent to the user for tracking.


The new access code is *not* linked to the user at this point.  If they were to give this code to someone else, it would then be linked to the other person.  There will be a parent/child relation between the two codes but not the transactions or identities behind them.


```json
{
  "message": "Transaction 83d0d0b0-68e8-4746-8197-ca4d18a21e2c restarted.",
  "new_access_code": "nktq2jy"
}
```
