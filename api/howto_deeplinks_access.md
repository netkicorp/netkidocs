# Deep Links

Deep links are how you route your end users into the Netki verification flow.
Each deep link carries an access code and sends the user to the right starting
point — straight into the app, or through a form first.

## Deep link types

Depending on your process, you may use any of these:

1. **Straight to the app** — the user goes directly into the verification app.
2. **Individual form, then app** — the user fills out an individual form, then
   is sent into the app automatically.
3. **Corporate form, then app** — the user fills out a corporate form, then is
   sent into the app automatically.

## Access codes in deep links

Access codes are agnostic — you choose which deep link to embed a code into.
The same rules apply to every deep link:

> [!WARNING]
> Each access code is **one-time use**. Embed a **unique code per user** and
> change it for every link you send. Handing the same code to more than one
> person causes collisions and failures.

Access codes are **free**, so do not recycle or reissue them — once a code has
been given out, don't hand it out again. Pull your codes from the API and
assign them to users from your own system; Netki can only tell whether a code
has been *used*, not whether you've *assigned* it. See
[Access Codes](./access_codes.md) for how to list codes and check usage, and
its best-practices note on treating the API as the source of truth for
existence and usage while tracking distribution on your side.

## Straight to the app (no form)

To send a user directly into the app for individual KYC, use the app deep link
with the access code in the `service_code` parameter:

```
https://daiu.app.link/yBE7efy4PI?service_code=<ACCESS_CODE>
```

Replace `<ACCESS_CODE>` with a unique access code for that user.

## Through a form first

To send a user through a form (individual or corporate) before the app, you
use a custom link for your form with a place for the access code at the end,
in the same way as the app deep link above. Each business's form link is
different — your account manager provides yours when you set up that flow.
