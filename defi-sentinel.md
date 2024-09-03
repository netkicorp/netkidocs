# Defi Sentinel Documentation
[Auth](#auth)
[Sentinel Request](#sentinel-request)
[Request Identity Proof](#request-identity-proof)
[Rule Sets](#rule-sets)

## Overview
The DeFi Sentinel system allows our clients to provide real time compliance at the time of the transaction including KYC/AML, wallet screening, transaction screening, securities and tax compliance. By enabling Sentinel DeFi protocols will be ale to meet the compliance requirements of institutions wishing to deploy capital into DeFi as well as emerging global regulations.

The high level process for this will be that our clients will submit a ruleset ID (from a list of custom rulesets that we create with the client), a smart contract address, and a list of wallet addressess.  We will return a Sentinel Request Object that contains entities for each of the wallet addressess.  Using the entity IDs the client can ask for a request for proof that can be submitted to their end-user in QR form who can use their identity wallet to submit the requested data (data changes based upon ruleset but standard is full name, birthdate, country, and person type for AML purposes).  Sentinel will process the defined rules and then provide an access token to our client which can be submitted to their smart contract.


## API Endpoints
To facilitate the process above the following endpoints will be used.


### Auth
The Sentinel API uses a JWT ([JSON Web Token](https://jwt.io/)) scheme for authentication and communication.

Your account executive will be able to provide you with your login credentials.

Access to the system is a two step process.  First you will need to login to get your access token.

Sample login call and response using Curl

```bash
curl -X "POST" "https://defi-sentinel.com/api/token/" \
    --form email=email \
    --form password=password
```

```json
{
  "refresh": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoicmVmcmVzaCIsImp0aSI6ImM5Yzg5MzBjMDU2ODQyMDE4NzcyYzc4MmVkZWNmMDIxIiwiZXhwIjoxNTI0OTM5NjgwLCJ1c2VyX2lkIjoxfQ.-MeXDc78wH0RxxNjrnOvC8DKK3Ujf1RAR7Ygjq4KMbE",
  "access": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwianRpIjoiZGJhNzQ5ZTBiZGNjNGM1NmJjMWI0NmQ1MWY4YzIzM2YiLCJleHAiOjE1MjQ4NTUwODAsInVzZXJfaWQiOjF9.tmubREi5qH2KZTBBK-Lf047gnyVllk_jNLD9qp0aesE"
}
```

 Sample refresh call and response using Curl 


```bash
curl -X "POST" "https://defi-sentinel.com/api/token/refresh/" \
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


### Sentinel Request
Sample Sentinel Request submission and response.  The entity ID returned in this request will be used to [request an identity proof](#request-identity-proof).  This should be called and refreshed anytime that the QR Code is going to be shown to the user to ensure that the request is still active.  Caching the request can result in errors with signatures.

```bash
curl -X "POST" "https://defi-sentinel.com/api/sentinel-requests/" \
--header "Content-Type: application/json" \
--header "Authorization: Bearer <JWT Token>" \
--data "{
    "smart_contract_address": "0x3d3763eC0a50CE1AdF83d0b5D99FBE0e3fEB43fb",
    "wallet_addresses": ["0x6f1ca141a28487f28efat64fb83a9033b02a8352"],
    "ruleset_id": "092ccb9d-68d3-438b-8e1b-bcee53456e1a"
}"
```

```json
{
    "id": "f174a4ef-9fa1-457b-8c57-6dfc18ab0a74",
    "state": "ongoing",
    "created": "2024-08-30T20:54:17.887226Z",
    "updated": "2024-08-30T20:54:17.887249Z",
    "business": {
        "id": "1591c56f-8c39-41f4-a346-5452eef525cd",
        "name": "Netkidemo"
    },
    "request_transactions": [
        {
            "id": "be371786-a8c3-430a-812c-5bce61bffc78",
            "client": {
                "id": "1591c56f-8c39-41f4-a346-5452eef525cd",
                "name": "Netkidemo"
            },
            "state": "new",
            "created": "2024-08-30T20:54:17.888992Z",
            "updated": "2024-08-30T20:54:17.889005Z",
            "transaction_entity": {
                "id": "bb6b0c28-8948-4274-91ba-e768b5e1ecaf",
                "identifier": "0x6f1ca141a28487f28efat64fb83a9033b02a8352",
                "date_time_of_validation": null,
                "entity_w3c_proofs": [
                    {
                        "w3c_raw_proof": "",
                        "name": "country",
                        "value": "",
                        "is_valid": false
                    },
                    {
                        "w3c_raw_proof": "",
                        "name": "birthDate",
                        "value": "",
                        "is_valid": false
                    },
                    {
                        "w3c_raw_proof": "",
                        "name": "personType",
                        "value": "",
                        "is_valid": false
                    },
                    {
                        "w3c_raw_proof": "",
                        "name": "name",
                        "value": "",
                        "is_valid": false
                    }
                ]
            },
            "sentinel_request": "f174a4ef-9fa1-457b-8c57-6dfc18ab0a74"
        }
    ]
}
```

### Request Identity Proof
Using the entity ID(s) that you get from the [Sentinel Request](#sentinel-request) endpoint you will use this to get a W3C Identity proof request.

```bash
curl -X "POST" 'https://defi-sentinel.com/api/generate-proof-request/' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <JWT token>' \
--data '{
    "entity_id": "bb6b0c28-8948-4274-91ba-e768b5e1ecaf"
}'
```

```json
{
    "id": "f62ce61e-e000-4162-80cb-806ee2ed180c",
    "thid": "7f38a193-0918-4a48-9fac-36adfdb8b542",
    "from": "did:polygonid:polygon:amoy:2qUU2Lxn4nyzn955xWccaHhhTkbzCRXhe9un4kLQ9y",
    "typ": "application/iden3comm-plain-json",
    "type": "https://iden3-communication.io/authorization/1.0/request",
    "body": {
        "reason": "test flow",
        "message": "",
        "callbackUrl": "https://defi-sentinal.com/api/callback?sessionId=1",
        "scope": [
            {
                "id": 2345678,
                "circuitId": "credentialAtomicQueryV3-beta.1",
                "query": {
                    "allowedIssuers": [
                        "*"
                    ],
                    "type": "ProofOfCountry",
                    "context": "https://raw.githubusercontent.com/netkicorp/netki-iden3-schemas/main/schemas/json-ld/proof-of-country.jsonld",
                    "credentialSubject": {
                        "country": {}
                    }
                }
            },
            {
                "id": 5482734,
                "circuitId": "credentialAtomicQueryV3-beta.1",
                "query": {
                    "allowedIssuers": [
                        "*"
                    ],
                    "type": "ProofOfBirthDate",
                    "context": "https://raw.githubusercontent.com/netkicorp/netki-iden3-schemas/main/schemas/json-ld/proof-of-birth-date.jsonld",
                    "credentialSubject": {
                        "birthDate": {}
                    }
                }
            },
            {
                "id": 6922734,
                "circuitId": "credentialAtomicQueryV3-beta.1",
                "query": {
                    "allowedIssuers": [
                        "*"
                    ],
                    "type": "ProofOfName",
                    "context": "https://raw.githubusercontent.com/netkicorp/netki-iden3-schemas/main/schemas/json-ld/proof-of-name.jsonld",
                    "credentialSubject": {
                        "name": {}
                    }
                }
            },
            {
                "id": 9024734,
                "circuitId": "credentialAtomicQueryV3-beta.1",
                "query": {
                    "allowedIssuers": [
                        "*"
                    ],
                    "type": "ProofOfPersonType",
                    "context": "https://raw.githubusercontent.com/netkicorp/netki-iden3-schemas/main/schemas/json-ld/proof-of-person-type.jsonld",
                    "credentialSubject": {
                        "personType": {}
                    }
                }
            }
        ]
    }
}
```

You will need to present the information in the JSON response here to your end-user as a QR Code so that they can scan it with their identity wallet to submit the proofs to us.


### Rule Sets
You can view your rulesets that are available to pass into a [Sentinel Request](#sentinel-request) from this endpoint.

```bash
curl -X "GET" 'https://defi-sentinel.com/api/rule-sets/' \
--header 'Authorization: Bearer <JWT Token>'
```

```json
[
    {
        "id": "092ccb9d-68d3-438b-8e1b-bcee53456e1a",
        "name": "Demo Ruleset",
        "description": "This ruleset is a demo set flow to make sure we have everything we need for things",
        "rules": [
            {
                "name": "Identity",
                "description": "The full name of the entity associated with this credential",
                "parameters": {
                    "credential_validation": [
                        "name",
                        "personType",
                        "birthDate",
                        "country"
                    ]
                },
                "rule_type": 1
            },
            {
                "name": "Sanctions AML",
                "description": "Sanctions only AML for the entity associated with this credential",
                "parameters": {
                    "aml": "Sanctions"
                },
                "rule_type": 2
            }
        ]
    }
]
```


### Final Processing

Once we've received all of the data we will run things through the ruleset selected and using the callbacks setup, provide you with an access token.  The token can vary depending on use case and requirements, we'll work with our clients to define one that works for their use cases.
