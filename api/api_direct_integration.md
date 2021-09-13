# API Integration 

Below are instructions for communicating directly with the Netki KYC API for image processing. 

## Overview

The API for the Netki KYC service has been streamlined for mobile applications.  Therefore there are very limited calls to the system to allow for minimal back and forth iterations. 


## Workflow

### Authentication 

You will be given a push only client token that you will use to do your initialization and authorization. 


Auth: 

    /api/business/verify-access-code/


```bash
curl --location --request POST 'https://kyc.myverify.info/api/business/verify-access-code/' \
--header 'Content-Type: application/json; charset=utf-8' \
--header 'Authorization: Token <CLIENT TOKEN>'
```

Response:

```javascript
{
  "access_code": "MAINJWT",
  "refresh_code": "REFRESHJWT",
  "business_context": {...
}
```

In the response that you get back you'll have a lot of business context information that you can ignore for now, the only thing you will need to worry about is the access_code which you'll use as the `Bearer` token for future requests in this chain (for image upload and submitting the data).


### Temp Image Upload

After you have captured images of the documents from the users you will need to upload them to our staging endpoint.  This endpoint is a push only service that takes a document and puts it onto a quarantined holding server. 

Images should be stored as JPG with little or no compression.  Do not alter the images in any manner before uploading them. Altering the images may cause them to fail validation checks. 

Best practices would be to upload the images asynchronously so that the user is not forced to wait until the image has fully uploaded. 


```bash
curl --location --request POST 'https://kyc.myverify.info/temp-document/' \
--header 'Content-Type: multipart/form-data; charset=utf-8; boundary=__X_RAW_BOUNDARY__' \
--header 'Authorization: Bearer <YOUR BEARER TOKEN>' \
--form 'document=@"/tmp/front.jpg"'
```

Response:

```javascript
{
  "url": "https://netkipearl-nonprod.s3.amazonaws.com/temp/3e1e1055-f610-4fa3-b801-768e1670bb7a.png?AWSAccessKeyId=AKIAYZACD5PZCRL4OCXM&Signature=BgFl%2B1d4zgjkIVdZlLTvtezk9To%3D&Expires=1630306302",
  "key": "3e1e1055-f610-4fa3-b801-768e1670bb7a.png"
}
```

The response that you get back will include an object with `url` and `key`.  Save all the keys along with what type of document you sent (`passport`, `front`, `back`, `selfie`) as the pair will be needed for the next step.  Please note that these types are specific and need to be one of those four.


### Transaction Creation

The final portion of the flow is to create a `transaction`.  The payload of the data passed to the server will consist of everything that was gathered along the collection process.  This will include the keys that were returned from the previous call to upload the image documents to the temp server. 

Below is a sample payload that is passed to the server for processing.  Please note: if you send through a passport then you need a passport and selfie entry in the identity documents.  If you send through a driver's license then you need a front, back, and selfie entry.  The document field is the key that you got back from the temp image upload in the previous step.


```javascript
{
    "identity": {
        "client_guid": "YOURGUIDHERE",
        "dui_number": "00000000-0",
        "birth_date": "1990-01-01",
        "identity_documents": [
            {
                "document": "a0c25aae-345c-48ab-97e9-83ca1f9b02ab.jpg",
                "document_type": "back"
            },
            {
                "document": "a087770a-5229-4fd1-81ef-135117e72192.jpg",
                "document_type": "front"
            },
            {
                "document": "f195ab7d-1ca0-4378-9c52-fccab9580fe8.jpg",
                "document_type": "selfie"
            }
        ]
    }
}
```


And the sample curl example:


```bash
curl --location --request POST 'https://kyc.myverify.info/api/myverify/' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <YOUR BEARER TOKEN>' \
--data-raw 'PAYLOAD ABOVE'
```


Response: 

```bash
{
    "message": {
        "id": "YOUR-NEW-TXID"
    }
}
```


## Asynchronous Operation

After the transaction is created you will be disconnected.  All processing will happen at the server level and your administrators will be notified via the callback from our production server environment. No further communication with the server will happen from the client as all keys are push-only.  If you wish to notify a user of the status of their KYC process you must relay that information from the server to the mobile client 


## Follow On Actions


The transaction ID that was returned in the final step above may be used to track the transaction itself or to know that the transaction is created.  Ultimately the data that is added in the `client_guid` field is what you use to associate the transaction to your users. 

For more information about gathering information regarding your users please see the documentation found in our github library: https://github.com/netkicorp/netkidocs/tree/master/api
