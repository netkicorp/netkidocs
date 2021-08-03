
API endpoints used in this section

https://kyc.myverify.info/api/token-auth/

https://kyc.myverify.info/api/business/businesses/

https://kyc.myverify.info/api/token-refresh/



The Netki API uses a JWT ([JSON Web Token](https://jwt.io/)) scheme for authentication and communication.

Your account executive will be able to provide you with your login credentials. Our web service is currently not self-service so your credentials will need to be updated though our support department.

Access to the system is a two step process.  First you will need to login to get your access token.

Sample login call and response using Curl
```bash
curl -X "POST" "https://kyc.myverify.info/api/token-auth/" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -d $'{
  "username": "client_username",
  "password": "client_password"
}'
```

```json
{
  "refresh": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoicmVmcmVzaCIsImp0aSI6ImM5Yzg5MzBjMDU2ODQyMDE4NzcyYzc4MmVkZWNmMDIxIiwiZXhwIjoxNTI0OTM5NjgwLCJ1c2VyX2lkIjoxfQ.-MeXDc78wH0RxxNjrnOvC8DKK3Ujf1RAR7Ygjq4KMbE",
  "access": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwianRpIjoiZGJhNzQ5ZTBiZGNjNGM1NmJjMWI0NmQ1MWY4YzIzM2YiLCJleHAiOjE1MjQ4NTUwODAsInVzZXJfaWQiOjF9.tmubREi5qH2KZTBBK-Lf047gnyVllk_jNLD9qp0aesE"
}
```

 Sample refresh call and response using Curl 


```bash
curl -X "POST" "https://kyc.myverify.info/api/token-refresh/" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -u ':' \
     -d $'{
  "refresh": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoicmVmcmVzaCIsImp0aSI6ImM5Yzg5MzBjMDU2ODQyMDE4NzcyYzc4MmVkZWNmMDIxIiwiZXhwIjoxNTI0OTM5NjgwLCJ1c2VyX2lkIjoxfQ.-MeXDc78wH0RxxNjrnOvC8DKK3Ujf1RAR7Ygjq4KMbE"
}'
```

```json
{
  "access": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwianRpIjoiYWNkZGIxYWQ3ZDJkNDcxNjgzMzQ0MzFiNDRjOGQ5N2YiLCJleHAiOjE1MjY2NzEyNzYsInVzZXJfaWQiOjEyfQ.F8x_uQ3NZCh66x99Ok4H1uzh19AC9GdS_OGM_xlzL44"
}
```

### Sample Business API Call
Using the access code from the login response you will pass that into your Authorization header when access any other API endpoints like so:

 Sample call to businesses end point which will provide your business information and the information for any corporate investors (if you have them) 

```bash
curl -X "GET" "https://kyc.myverify.info/api/business/businesses/" \
     -H 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwianRpIjoiZGJhNzQ5ZTBiZGNjNGM1NmJjMWI0NmQ1MWY4YzIzM2YiLCJleHAiOjE1MjQ4NTUwODAsInVzZXJfaWQiOjF9.tmubREi5qH2KZTBBK-Lf047gnyVllk_jNLD9qp0aesE'
```

If you save your business ID from the results here it will come in handy in the next page under access codes and for any other business related API calls.

```json
{
  "count": 1,
  "next": null,
  "previous": null,
  "results": [
    {
      "id": "604e1738-4716-4bdd-867b-4942186b1e1c",
      "app_context": {
        "id": 7,
        "required_fields": [
          {
            "id": 4,
            "options": [
              {
                "id": 3,
                "created": "2018-04-16T22:26:35.268006Z",
                "updated": "2018-04-16T22:26:35.283482Z",
                "is_active": true,
                "key": "true",
                "position": 0,
                "label": "Yes",
                "language_code": "en"
              },
              {
                "id": 4,
                "created": "2018-04-16T22:26:35.268006Z",
                "updated": "2018-04-16T22:26:35.283482Z",
                "is_active": true,
                "key": "false",
                "position": 0,
                "label": "No",
                "language_code": "en"
              }
            ],
            "created": "2018-04-16T22:26:35.233747Z",
            "updated": "2018-04-16T22:26:35.258242Z",
            "is_active": true,
            "name": "is_accredited_investor",
            "data_type": "list",
            "regex": null,
            "keypad": "list",
            "label": "Are you an Accredited Investor (US Only)",
            "description": "Are you an Accredited Investor (US Only)",
            "language_code": "en"
          }
        ],
        "created": "2018-04-27T18:33:17.657524Z",
        "updated": "2018-04-27T18:33:30.955998Z",
        "is_active": true,
        "logo_dark": "https://netkipearldev.s3.amazonaws.com/businesses/assets/logo_netki_mark_solo_small.png?AWSAccessKeyId=AKIAJQRUEJLMVY3TS25Q&Signature=ovmwObaG%2B7MnAW6Xc0AKlIxyoEA%3D&Expires=1524857788",
        "logo_light": "https://netkipearldev.s3.amazonaws.com/businesses/assets/logo_netki_mark_solo_small.png?AWSAccessKeyId=AKIAJQRUEJLMVY3TS25Q&Signature=ovmwObaG%2B7MnAW6Xc0AKlIxyoEA%3D&Expires=1524857788",
        "redirect_backlink": null,
        "access_code_prefix": "nkt",
        "preferred_restart_contact_method": "sms",
        "restart_limit": "20",
        "phone_pin_timeout": 120,
        "phone_retry_attempt_limit": 1,
        "phone_use_automatic_bypass": false,
        "accredited_investor_flow": "accredited_web_form",
        "business": "604e1738-4716-4bdd-867b-4942186b1e1c",
        "welcome_message": "Welcome to MyVerify!",
        "completed_message": "Thanks for signing up!",
        "sms_verification_message": null,
        "sms_corporate_onboard_message": null,
        "declined_feedback_sms": null,
        "sms_accredited_message": null,
        "language_code": "en"
      },
      "country_blacklist": [],
      "business_documents": [],
      "business_addresses": [],
      "business_media_references": [],
      "business_data_sources": [],
      "business_json_objects": [],
      "errors": [],
      "created": "2018-04-27T18:29:02.798424Z",
      "updated": "2018-04-27T18:31:50.661200Z",
      "is_active": true,
      "name": "Netkitesting",
      "status": "open",
      "established_date": "2018-04-27",
      "business_type": null,
      "email": null,
      "country_code": null,
      "primary_account": 12,
      "parent_business": null,
      "contettype": 30
    }
  ]
}
```