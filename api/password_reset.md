# Netki API Password Reset

This endpoint is used for Netki dashboard and API users only.




API endpoints used in this section

https://kyc.myverify.info/api/password_reset/

https://kyc.myverify.info/api/password_reset_confirm/



In the event that a user needs to have their password reset, the Netki API has two endpoints to perform this action.

The first endpoint takes an email address as a parameter and always returns an HTTP 200 response.
Always returning an HTTP 200 response prevents the brute force guessing of email accounts.

The second endpoint takes 3 parameters. A `password`, `recaptcha` and `token`.
If the `recaptcha` and `token` values are valid then the user's password will be
reset to the value of `password`.

Sample call to `password_reset/` using curl 
```bash
curl -X "POST" "https://kyc.myverify.info/api/password_reset/" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -d $'{
  "email": "test@example.com"
}'
```

```json
{
  "message": "Password reset process initiated."
}
```



```bash
curl -X "POST" "https://kyc.myverify.info/api/password_reset_confirm/" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -d $'{
    "password": "newpassword",
    "recaptcha": "validrecaptchatoken",
    "token": "validpasswordresettoken"
}'
```

```json
{
  "message": "Password reset."
}
```
