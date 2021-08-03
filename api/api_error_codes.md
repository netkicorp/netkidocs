## Pulling Error Codes

During processing of your documents you may encounter errors or warnings for any number of things. 

In some cases you may want to see and react against our error codes.  For instance if your user took a picture that was too dark or used a document that isn't supported you may gleen information that can help in automating a response. 

Below is an example of a request to get all error codes. 


```
curl -X GET \
  https://kyc.myverify.info/api/errors/errors/ \
  -H 'Accept: */*' \
  -H 'Authorization: Bearer TOKEN' \
```

```
{
    "count": 71,
    "next": null,
    "previous": null,
    "results": [
        {
            "error_code_id": 2523,
            "created": "2018-09-26T16:12:18.816352Z",
            "updated": "2019-06-12T19:30:21.355919Z",
            "is_active": true,
            "error_code_name": "document_birthdate_invalid",
            "rank": 21,
            "category": "document",
            "error_code_description": "Unable to read birth date. Retake photo & ensure birth date is visible",
            "language_code": "en"
        },
        {
            "error_code_id": 2522,
            "created": "2018-09-18T23:17:53.965554Z",
            "updated": "2019-06-12T19:30:21.349950Z",
            "is_active": true,
            "error_code_name": "document_quality",
            "rank": 20,
            "category": "document",
            "error_code_description": "Document quality is low. Retake photo in good lighting",
            "language_code": "en"
        },
        ...
```