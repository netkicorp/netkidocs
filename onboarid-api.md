# Onboard ID API Docs
The following are docs on how to integrate with different aspects of the Onboard ID platform.  *Please note:* These API endpoints are not for processing KYC.  These are to be used for things outside of the main KYC flow.  These endpoints would mostly be used if you are looking to automate business logic on your end or make your own custom dashboard.  If you aren't using the Myverify app that we provide and want to know how to integrate us into your own KYC app, please look at the SDK docs linked from the [main docs](README.md) page.


### Deeplinks
While this isn't directly related to the API, [here](/api/howto_deeplinks_access.md) we have documentation on how to use deeplinks with our Myverify app and when using our KYB forms submission service.


### Authentication
Information on how to authenticate for the API can be find [here](/api/authentication.md).  This should be the first place you look when getting integrated with our API.

### Callbacks
For communicating state and flow as your users go through our system, as well as to notify you of issues that require your attention, we send callbacks to your servers.  We have a set of [best practices](/api/best_practices_internal_callbacks.md) for how to handle them as well as information on how to manually call them if needed for testing or getting information again that might have been missed [here](/api/callbacks2.md).  You can also check the config for you callbacks there as well as from the dashboard at [https://compliance.netki.com](https://compliance.netki.com).  Please note that the user needs Admin rights to be able to make any changes (in either location).

### Transactions and Access Codes
The transactions are central to how things are tracked in our system.  Each individual run through KYC is tracked via a transaction and will have a transaction ID associated with it.  For users going through our MyVerify app, we also create access codes that are associated with your account.  For more details check [here](/api/access_codes.md).  You can access information on transactions and what the different states mean [here](/api/polling.md).

For clients using our SDK to build custom apps, there are no access codes. Instead, you can set a client_guid during the SDK flow. This client_guid works similarly to an access code, but it is specific to SDK-based implementations.

Here’s how it works:
- *No access codes needed:* Our SDK doesn’t require access codes like MyVerify does.
- *Use client_guid instead:* During the SDK flow, you generate and set a `client_guid`. This unique identifier:
    - Tracks individual sessions or flows in our system.
    - Can be linked to records or processes in your own system.

This way, the `client_guid` helps you track and manage specific interactions without needing access codes, giving you more flexibility and control.

### Error Codes
We provide an endpoint that will lists all of the error codes that we assign to things in our system with details [here](/api/api_error_codes.md).
