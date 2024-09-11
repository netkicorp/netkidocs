## How to Issue Deeplinks with Access Codes to End Users

### Deeplinks

Deeplinks are how users are routed through your compliance process.  Each Master Deeplink has a specific function.

Depending on your process you may have several deeplinks as follows:

1. Take the end user straight to the app
2. Take the end user to an individual form (which automatically goes to app afterwards)
3. Take the end user to a corporate form (which automatically goes to app afterwards)

### Access Codes


Access codes are agnostic. You choose which deeplink to put the access code into.

Example:

Access code: `abcrt6c`

NOTE: In each of the deeplinks the access codes are embedded and you **must** change them for each user as the codes are **one-time use**. Giving the same access code out to multiple individuals will cause **collisions and failures**. 

Access codes are **free** so do NOT recycle. Once they have been issued do not reissue them. You can keep track of your access codes via the API [here](./access_codes.md).  It's best practices to pull them into your system from the API and assign them from there to your end users.  Be aware that our system can't tell when you've given one out, so we only know if it's been used, not assigned on your end.

#### Straight to App (no forms)

If you want the user to just go directly to the app for KYC as an individual you sent them this link:

	https://daiu.app.link/yBE7efy4PI?service_code=<ACCESS_CODE>


#### Straight to Forms (and automatically sent to app afterwards)

If you want the user to fill out a corporate form and then automatically go to the app for KYC then you will be given a custom link for your form that will have a place at the end for the access_code like the deeplink above.  Each business link is different and will be given to you by your account manager when you sign up for that.

**NOTE:** in each of these the codes are embedded and you must change them for each user as the codes are 1x use and giving them out to multiple individuals will cause collisions and failures. 
