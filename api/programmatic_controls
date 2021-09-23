# Transaction Conditional Fail Logic - Chivo

Business request:  Provide the capability to programmatically fail transaction based on certain criteria so that we can internally send a user back through their KYC.


## Scenario


Given a `state` of `hold` you may wish to programmatically restart a user. 


Ex: A user is on `hold` for a face match score lower than the allowed threshold.  


The Netki KYC system tracks hold reasons at various levels of depth.  


## Error Data


In order to determine this is a maching scenario you will look in the payload for the error code found here: 

In the payload data object you will traverse: 

	transaction > transaction_identity > errors[] > "error_code_id" = 2013


Example object body: 


```javascript
        "errors": [
        {
            "id": 191898,
            "error_code":
            {
                "error_code_id": 2013,
                "created": "2021-09-10T01:04:57.451887Z",
                "updated": "2021-09-10T01:05:03.002795Z",
                "is_active": true,
                "error_code_name": "identity_rnpn_selfie",
                "rank": 0,
                "category": "identity",
                "error_code_description": "RNPN Document and selfie below match threshold.",
                "language_code": "en"
            },
            "created": "2021-09-16T16:58:31.401152Z",
            "updated": "2021-09-16T16:58:31.401174Z",
            "is_active": true,
            "object_id": "ab8b5aa4-3a04-462e-9dab-22efcd450b1d",
            "content_type": 27
        }],
```

This is the same structured data that is returned from the API polling as what is delivered in the callback.



## API Updates

Once you encounter this scenario you can update the record via the API.  The same functionality that is in the dashboard. 

You will need to use the transaction ID to call the make-failed 

You have the ability to send data in the notes field which we recommend.  This is a free form field.  

For the reason field see below. 

```bash
curl --request PUT \
  --url https://kyc.myverify.info/api/transactions/a44803ed-1e55-47c9-bcaa-cf2b9c9cdc71/make-failed/ \
  --header 'Content-Type: application/json; charset=utf-8' \
  --data '{
  "notes": "FAILED BY CHIVO FOR FACE MISMATCH","reasons": ["other"]
}'
```

Something to note: Once a record is failed it is permanently there. 

If you need a specific error reason we can add one upon request. 
