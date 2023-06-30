# AML-Only Transaction Processing

## Who This Is For

Some companies may wish to use Netki services for AML only transaction processing. 

## Implementation

VERB: POST

Endpoint: https://kyc.myverify.info/api/single-aml/

The payload will change depending on the information that you are sending.  No special characters other than - should be used.  And no numbers should be used for names, numbers are ok for blockchain_addresses.

Options:

+ **first_name**: the first name of the person you are running AML on.
+ **middle_name**: the middle name of the person you are running AML on.
+ **last_name**: the last name of the person you are running AML on.
+ **full_name**: the full name of the person you are running AML on. (See description below regarding this field)
+ **date_of_birth**: the date of birth of the person you are running AML on.  This is optional but will reduce false positives.
+ **blockchain_address**: a wallet or entity on a blockchain. 
+ **aml_type**: The type of AML being run: corporate, individual, *or* blockchain.

Payload options are:
```
{
    "first_name": required,
    "middle_name": optional,
    "last_name": required,
    "date_of_birth": optional,
    "aml_type": required
}
```

```
{
    "full_name": required,
    "date_of_birth": optional,
    "aml_type": required
}
```

```
{
  "blockchain_address": required, 
  "aml_type": required
}
```

Use the first option for countries that supply a separate first, middle, and last name.  Use the second option for countries that don't (most Asian countries).

Response type is `JSON`.

## Data Responses

There are 2 ways to receive your users' results and their associated data. We discuss this in length above in the *Callback* documentation section.  You can set your callback URL in the dashboard under Business Settings. Upon receipt and processing of an AML we will send you a callback with all the data.  It is best to plan around setting up API polling so that you will find anything that has come in if there is a lapse in communication. Please see our [Callback Best Practices](https://github.com/netkicorp/netkidocs/blob/master/best_practices_internal_callbacks.md).
