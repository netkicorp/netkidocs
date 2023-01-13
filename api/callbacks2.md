## API endpoints used in this section

https://kyc.myverify.info/api/token-auth/

https://kyc.myverify.info/api/transactions/UUID/make-callback/

https://kyc.myverify.info//api/business/businesses/UUID/config/




The Netki system will send callbacks to your servers upon the successful completion of a user going through the KYC process in the MyVerify app.  You won't need to manually make callbacks for that but knowing how to do so will be helpful for testing your server response and getting sample data.  To setup your account for callbacks please contact your account executive and they can assist with getting that enabled.  Or you can use the following endpoint to enable/disable callbacks.

##### Transaction Callbacks

The following actions will kick off an automatic callback:

* User completes the KYC flow in the MyVerify app and transaction processes on the backend (regardless of ending status).
* Transaction status changes (goes from hold to approved/declined/restarted).
* User uploads an Accredited Investor document via Netki.
* User finishes Verified Investor workflow.
* User completes eForm _after_ having gone through KYC flow in MyVerify app.
* New user is added via the Business On-Boarding Form (UBO or Corporate Officer)


You'll want to pop over to the Access Codes Lookup HowTo section for directions how to get your Business ID before the next steps.

##### Business Configuration

Your business configuration has all of your callback information in it.  To see what you have setup in your config you'll want to do the following (credentials are write only so won't show up here):


```bash
curl -X "POST" "https://kyc.myverify.info/api/token-auth/" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -d $'{
  "username": "client_username",
  "password": "client_password"
}'
```


```bash
curl -X "GET" "https://kyc.myverify.info/api/business/businesses/604e1738-4716-4bdd-867b-4942186b1e1c/config/" \
     -H 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwianRpIjoiZGJhNzQ5ZTBiZGNjNGM1NmJjMWI0NmQ1MWY4YzIzM2YiLCJleHAiOjE1MjQ4NTUwODAsInVzZXJfaWQiOjF9.tmubREi5qH2KZTBBK-Lf047gnyVllk_jNLD9qp0aesE'
```

You'll get a response similar to:

```json
{
  "count": 1,
  "next": null,
  "previous": null,
  "results": [
    {
      "id": "0a652407-dac4-45ec-9ad0-5ef79b4d8138",
      "callback_url": "https://mycompany.com/callbacks/",
      "callback_authentication_scheme": "basic",
      "callback_expected_status": 200,
      "callback_retry_limit": 8,
      "is_callback_paused": false,
      "is_callback_enabled": true
    }
  ]
}
```

Let's break down the response:

* id: this field is the ID of your configuration object.  You'll want to save this for when making updates to your configuration.
* callback_url: this is the URL that we will send callbacks to.
* callback_authentication_scheme: the selected scheme for callbacks.  Current options are: basic and noauth.  If you would like us to add a different type please contact your account rep.
* callback_expected_status: this is the status that your system should give us upon a successful callback.  We default to 200.
* callback_retry_limit: this is how many times we will retry a callback before pausing callbacks due to errors.
* is_callback_paused: this denotes if callbacks are paused or not.  If callbacks are paused new callbacks will be created but won't be sent.  This will need to be changed to false and the restart-callbacks endpoint will need to be hit (we'll cover that later on this page).
* is_callback_enabled: if this is false then we won't even try and send callbacks.

To update your config you can use the following:

```bash
curl -X "PUT" "https://kyc.myverify.info/api/business/businesses/0a652407-dac4-45ec-9ad0-5ef79b4d8138/config/0a652407-dac4-45ec-9ad0-5ef79b4d8138/" \
     -H 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwianRpIjoiMjY2Nzc3ZGQ5N2Y2NDJhM2FlZDY2OGQ0YmM1YTBlMzciLCJleHAiOjE1Mzg1MDk2ODYsInVzZXJfaWQiOjEyfQ.38q0CVC-u479Jzbd0IAfXVPeqL_ZXZaBHNZa5UHwzV0' \
     -H 'Content-Type: application/json; charset=utf-8' \
     -d $'{
  "is_callback_enabled": "false"
}'
```

In the JSON payload you can update multiple options as listed above.  There is one other option that is not listed up there.

* callback_credentials: this is a write only field so won't show up when you look at the config.  For basic auth set your credentials like so: {"callback_credentials": {"username": user, "password": pass}}

##### Transaction Callbacks

To get a transaction ID for doing callbacks, go through the KYC process in the MyVerify app and then login to your [dashboard](https://compliance.netki.com).  Click through to the name that you see and the transaction ID will be at the top of the screen there.  Using that transaction ID you'll want to do the following:


```bash
curl -X "POST" "https://kyc.myverify.info/api/token-auth/" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -d $'{
  "username": "client_username",
  "password": "client_password"
}'
```


```bash
curl -X "POST" "https://kyc.myverify.info/api/transactions/f9736213-8ed6-4bc3-8278-4a0bc1c6f4f9/make-callback/" \
     -H 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwianRpIjoiZGJhNzQ5ZTBiZGNjNGM1NmJjMWI0NmQ1MWY4YzIzM2YiLCJleHAiOjE1MjQ4NTUwODAsInVzZXJfaWQiOjF9.tmubREi5qH2KZTBBK-Lf047gnyVllk_jNLD9qp0aesE'
```


Hitting the make-callback end-point won't give you the results of the callback directly, it will schedule the callback on our back end processor which will then make the callback to your servers.  If you don't get the callback sent to your servers after a few minutes, please ensure that your server is setup correctly to receive them, and try again.  If you are still having problems please contact your account executive for assistance.

```json
{
  "message": "Callback scheduled."
}
```

Here is a sample transaction result.  For more details about the responses you will get please check the [Polling]() page.

```json
{
  "identity": {
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
      "identity_documents": [
        {
          "id": "b97274d3-47f9-4a0d-a0ec-5e8d20c30318",
          "errors": [],
          "mime_type": null,
          "created": "2018-06-20T17:49:56.066101Z",
          "updated": "2018-06-20T17:49:56.074392Z",
          "document": "https://netkipearldev.s3.amazonaws.com/identities/documents/b12345d3-47f9-4a0d-a0ec-5e8d20c12345.selfie.jpg?AWSAccessKeyId=AKIAJKOYXCORCQA2IEMQ&Signature=123o4MHTUov48kmo5co83gNJAXM%3D&Expires=0000803411",
          "document_type": "selfie",
          "expiration_date": null,
          "state": "new",
          "is_active": true,
          "identity": "f6ee3bb3-8955-4b6a-b012-f75caa0de364",
          "document_classification": null,
          "contenttype": 26
        }
      ],
      "identity_data_sources": [
        {
          "id": "a3e07f08-1d56-4e53-bd41-ff1f4fdfa1fe",
          "created": "2018-05-22T23:50:28.984376Z",
          "updated": "2018-06-01T20:40:37.684915Z",
          "is_active": true,
          "raw_data": {
            "face_match_score": "29"
          },
          "reference_url": "",
          "comply_search_matches": 0,
          "score": "29",
          "identity": "f6ee3bb3-8955-4b6a-b012-f75caa0de364",
          "data_provider": 3
        }
      ],
      "identity_data_listings": [
        {
          "id": "feb42133-7737-4a87-95a4-133ad8836303",
          "listing_type": {
            "id": 11,
            "created": "2018-04-18T21:22:41.783260Z",
            "updated": "2018-04-18T21:22:41.783330Z",
            "is_active": true,
            "name": "adverse-media-violent-crime",
            "description": "",
            "is_flagged": false
          },
          "created": "2018-07-12T02:55:57.819137Z",
          "updated": "2018-07-12T02:55:57.872607Z",
          "is_active": true,
          "notes": null,
          "identity": "f6ee3bb3-8955-4b6a-b012-f75caa0de364"
        }
      ],
      "identity_media_references": [
        {
          "id": "2cf52503-7140-49c9-a616-1542d33ce1d4",
          "created": "2018-07-12T02:55:57.837038Z",
          "updated": "2018-07-12T02:55:57.874114Z",
          "is_active": true,
          "activity_date": "2018-07-12T02:55:57.873624Z",
          "source": "http://www.ceutaldia.com/tags/calienta?page=1",
          "title": "Tags calienta - Ceuta al Día - diario digital de Ceuta",
          "content": "Los 14 jugadores de la lista definitiva de Valero Rivera disputarán el trofeo Domingo Bárcenas ante Argentina y Túnez. Leer Miranda Kerr hace caso omiso al frío invernal y lejos de plantarse el disfraz de Mamá Noel, prefiere mostrar su cuerpo desnudo y totalmente recuperado de su embarazo (que también mostró sin nada de ropa) tal y como podemos ver en unas fotografías para «Industrie». La revista muestra a la novia de Orlando Bloom, de 28 años, en unas imágenes en blanco y negro en las que la modelo de «Victorias Secret» luce sus curvas de infarto, de la mano del fotógrafo Willy Vanderperre.",
          "identity": "f6ee3bb3-8955-4b6a-b012-f75caa0de364"
        }
      ],
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
}
```

To view the callbacks for a transaction you'll want to use the following:

```bash
curl -X "GET" "https://kyc.myverify.info/api/transactions/a1f57081-7cdb-407c-b579-80362b83855e/get-callbacks/" \
     -H 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwianRpIjoiZGJhNzQ5ZTBiZGNjNGM1NmJjMWI0NmQ1MWY4YzIzM2YiLCJleHAiOjE1MjQ4NTUwODAsInVzZXJfaWQiOjF9.tmubREi5qH2KZTBBK-Lf047gnyVllk_jNLD9qp0aesE'
```

```json
{
  "count": 1,
  "callbacks": [
    {
      "id": 5957,
      "created": "2018-09-28T03:19:29.103396Z",
      "updated": "2018-09-28T03:21:37.637570Z",
      "is_active": true,
      "state": "completed",
      "response_raw": "OK",
      "status_code": 200,
      "callback_duration": "0.0088900000",
      "callback_url": "https://mycompany.com/callbacks/",
      "callback_counter": 1,
      "transaction": "a1f57081-7cdb-407c-b579-80362b83855e"
    }
  ]
}
```

Viewing the callbacks for a transaction is handy as it will help troubleshoot why something isn't working.  We'll store the response from your endpoint in the response_raw field.  If you are expecting to be getting callbacks for a transaction, you can use the information here to troubleshoot why it's not happening.

Previously we mentioned callbacks getting paused.  You can either pause these yourself via the config updates if you want to queue them up while doing server maintenance or if we run into problems sending callbacks to your endpoint, we will pause after a certain amount of bad requests.  For whatever reason things are paused, once the pause is removed, you'll want to send off all of the transactions that are waiting in the queue.


Updating the config to make is_callbacks_paused = false won't make the paused callbacks automatically queue up to send again.  You'll need to use the restart-callbacks endpoint for that.


```bash
curl -X "POST" "https://kyc.myverify.info/api/transactions/restart-callbacks/" \
     -H 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwianRpIjoiZGJhNzQ5ZTBiZGNjNGM1NmJjMWI0NmQ1MWY4YzIzM2YiLCJleHAiOjE1MjQ4NTUwODAsInVzZXJfaWQiOjF9.tmubREi5qH2KZTBBK-Lf047gnyVllk_jNLD9qp0aesE'
```

```json
{
  "message": "Callbacks restarted."
}
```

Hitting this endpoint will queue up all of the paused callbacks to get sent off to you.
