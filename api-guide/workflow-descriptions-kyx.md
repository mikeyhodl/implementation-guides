# Workflow Descriptions

## Table of Contents
- [Workflow 10011: Standalone ID and Identity Verification](#workflow-11-standalone-id-and-identity-verification)
- [Workflow 10013: ID and Identity Verification + Screening + Proof Of Residency + Address Validation + US DL Verification](#workflow-10013-id-and-identity-verification--screening--proof-of-residency--address-validation--us-dl-verification)
- [Retrieval](#retrieval-2)

Workflows are specified using the `key` attribute in the `workflowDefinition` object:
```
"workflowDefinition": {
    "key": DEFINITION_KEY,
    "credentials": []
}
```

## Workflow 10011: Standalone ID and Identity Verification
This workflow verifies a photo ID document and returns a) whether that document is valid, and b) data extracted from that document. It also compares the user’s face with the photo on the ID and performs a liveness check to ensure the person is physically present.

### Required Credentials
* ID
* Selfie
* Facemap

### Stored credentials
* ID
* Selfie
* Facemap

### Prerequisites
* Identity Verification must be enabled for your account. (Contact your Jumio account manager for activation.)
* Allowed channels: API, SDK, WEB

### Capabilities
In order of dependency:
* [usability](kyx%2Bapi_guide.md#capabilitiesusability) --> [imageChecks](kyx%2Bapi_guide.md#capabilitiesimagechecks) --> [extraction](kyx%2Bapi_guide.md#capabilitiesextraction) --> [dataChecks](kyx%2Bapi_guide.md#capabilitiesdatachecks)
* [usability](kyx%2Bapi_guide.md#capabilitiesusability) --> [liveness](kyx%2Bapi_guide.md#capabilitiesliveness)

### Example

### Initiate Account Creation
HTTP Request Method: __POST__
* US: `https://account.amer-1.jumio.ai/api/v1/accounts`
* EU: `https://account.emea-1.jumio.ai/api/v1/accounts`
* SG: `https://account.apac-1.jumio.ai/api/v1/accounts`

__Request:__
```
curl --location --request POST 'https://account.amer-1.jumiopp.link/api/v1/accounts' \
--header 'Content-Type: application/json' \
--header 'Authorization: Basic YOUR_ACCESS_TOKEN' \
--data-raw '{
    "customerInternalReference":"CUSTOMER_REFERENCE",
    "workflowDefinition":{
        "key": 10011
    },
    "userReference": "YOUR_USER_REFERENCE",
    "callbackUrl":"YOUR_CALLBACK_URL",
    "tokenLifetime": "5m"
}'
```

__Response:__
```
{
    "timestamp": "2022-01-06T13:41:20.769Z",
    "account": {
        "id": "11111111-1111-1111-1111-aaaaaaaaaaaa"
    },
    "sdk": {
        "token": "XXXX"
    },
    "workflowExecution": {
        "id": "22222222-2222-2222-2222-aaaaaaaaaaaa",
        "credentials": [
            {
                "id": "33333333-3333-3333-aaaaaaaaaaaa",
                "category": "ID",
                "allowedChannels": [
                    "WEB",
                    "API",
                    "SDK"
                ],
                "api": {
                    "token": "XXXX",
                    "parts": {
                        "front": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/FRONT",
                        "back": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/BACK"
                    },
                    "workflowExecution": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa"
                }
            },
            {
                "id": "33333333-3333-3333-bbbbbbbbbbbb",
                "category": "SELFIE",
                "allowedChannels": [
                    "WEB",
                    "API",
                    "SDK"
                ],
                "api": {
                    "token": "XXXX",
                    "parts": {
                        "face": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-bbbbbbbbbbbb/parts/FACE"
                    },
                    "workflowExecution": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa"
                }
            },
            {
                "id": "33333333-3333-3333-cccccccccccc",
                "category": "FACEMAP",
                "allowedChannels": [
                    "WEB",
                    "SDK"
                ]
            }
        ]
    }
}
```

#### Upload

HTTP Request Method: __POST__
* US: `https://api.amer-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>/credentials/<credentialsId>/parts/<classifier>`
* EU: `https://api.emea-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>/credentials/<credentialsId>/parts/<classifier>`
* SG: `https://api.apac-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>/credentials/<credentialsId>/parts/<classifier>`

__Request Path Parameters:__
| Parameter           | Type   | Note                                      |
|---------------------|--------|-------------------------------------------|
| accountId           | string | UUID of the account                       |
| workflowExecutionId | string | UUID of the workflow                      |
| credentialsId       | string | UUID of the credentials                   |
| classifier          | string | FRONT, BACK                               |

__Request Body:__
| Key  | Value                                                          |
|------|----------------------------------------------------------------|
| file | JPEG, PNG  (max. size 10 MB and max resolution of 8000 x 8000) |

##### Upload Document FRONT
__Request:__
```
curl --location --request POST 'https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/FRONT' \
--header 'Authorization: Bearer xxx' \
--form 'file=@"/J:/User/2020-03-01/faces/front.jpg"'
```

##### Upload Document BACK
```
curl --location --request POST 'https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-bbbbbbbbbbbb/parts/BACK' \
--header 'Authorization: Bearer xxx' \
--form 'file=@"/J:/User/2020-03-01/faces/back.jpg"'
```

##### Upload Document FACE
```
curl --location --request POST 'https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-bbbbbbbbbbbb/parts/FACE' \
--header 'Authorization: Bearer xxx' \
--form 'file=@"/J:/User/2020-03-01/faces/face.jpg"'
```

#### Finalize
HTTP Request Method: __PUT__
* US: `https://api.amer-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>`
* EU: `https://api.emea-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>`
* SG: `https://api.apac-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>`

__Request:__
```
curl --location --request PUT 'https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa' \
--header 'Authorization: Bearer xxx'
```

__Response:__
```
{
    "timestamp": "2021-05-24T08:20:31.344Z",
    "account": {
        "id": "11111111-1111-1111-1111-aaaaaaaaaaaa"
    },
    "workflowExecution": {
        "id": "22222222-2222-2222-2222-aaaaaaaaaaaa"
    }
}
```

## Workflow 10013: ID and Identity Verification + Screening + Proof Of Residency + Address Validation + US DL Verification
This workflow verifies a photo ID document and returns a) whether that document is valid, and b) data extracted from that document. It also compares the user's face with the photo on the ID and performs a liveness check to ensure the person is physically present. Performs a proof of residency on the users address, as well as address validation. Checks if user is part of any sanctions list and also performs US Driving License Verification.

### Required Credentials
* ID
* Selfie
* Facemap

### Stored Credentials
* ID
* Selfie
* Facemap

### Prerequisites
* Identity Verification must be enabled for your account. (Contact your Jumio account manager for activation.)
* Screening must be enabled for your account. (Contact your Jumio account manager for activation.)
* Screening must be set up for your Customer Portal (see [Portal Setup](https://github.com/Jumio/implementation-guides/blob/master/netverify/netverify-screening.md))
* Proof Of Residency must be enabled for your account. (Contact your Jumio account manager for activation.)
* Address Validation must be enabled for your account. (Contact your Jumio account manager for activation.)
* US DL Verification must be enabled for your account. (Contact your Jumio account manager for activation.)
* Allowed channels: API, SDK, WEB

### Capabilities
In order of dependency:
* [usability](kyx%2Bapi_guide.md#capabilitiesusability) --> [imageChecks](kyx%2Bapi_guide.md#capabilitiesimagechecks) --> [extraction](kyx%2Bapi_guide.md#capabilitiesextraction) --> [dataChecks](kyx%2Bapi_guide.md#capabilitiesdatachecks)
* [usability](kyx%2Bapi_guide.md#capabilitiesusability) --> [liveness](kyx%2Bapi_guide.md#capabilitiesliveness)
* [usability](kyx%2Bapi_guide.md#capabilitiesusability) --> [imageChecks](kyx%2Bapi_guide.md#capabilitiesimagechecks) --> [extraction](kyx%2Bapi_guide.md#capabilitiesextraction) --> [watchlistScreening](kyx%2Bapi_guide.md#watchlistScreening)
* [usability](kyx%2Bapi_guide.md#capabilitiesusability) --> [imageChecks](kyx%2Bapi_guide.md#capabilitiesimagechecks) --> [extraction](kyx%2Bapi_guide.md#capabilitiesextraction) --> [addressValidation](kyx%2Bapi_guide.md#addressValidation)
* [usability](kyx%2Bapi_guide.md#capabilitiesusability) --> [imageChecks](kyx%2Bapi_guide.md#capabilitiesimagechecks) --> [extraction](kyx%2Bapi_guide.md#capabilitiesextraction) --> [proofOfResidency](kyx%2Bapi_guide.md#proofOfResidency)
* [usability](kyx%2Bapi_guide.md#capabilitiesusability) --> [imageChecks](kyx%2Bapi_guide.md#capabilitiesimagechecks) --> [extraction](kyx%2Bapi_guide.md#capabilitiesextraction) --> [drivingLicenseVerification](kyx%2Bapi_guide.md#drivingLicenseVerification)

### Example

### Initiate Account Creation
HTTP Request Method: __POST__
* US: `https://account.amer-1.jumio.ai/api/v1/accounts`
* EU: `https://account.emea-1.jumio.ai/api/v1/accounts`
* SG: `https://account.apac-1.jumio.ai/api/v1/accounts`

__Request:__
```
curl --request POST --location 'https://account.amer-1.jumio.ai/api/v1/accounts' \
    --header 'Content-Type: application/json' \
    --header 'User-Agent: User Demo' \
    --header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
    --data-raw '{
        "customerInternalReference": "CUSTOMER_REFERENCE",
        "workflowDefinition": {
            "key": 10013,
            "credentials": [
                {
                    "category": "ID",
                    "type": {
                        "values": ["DRIVING_LICENSE", "ID_CARD", "PASSPORT"]
                    },
                    "country": {
                        "values": ["USA", "CAN", "AUT", "GBR"]
                    }
                }
            ]
        },
        "callbackUrl": "YOUR_CALLBACK_URL",
        "userReference": "YOUR_USER_REFERENCE"
    }'
```

__Response:__
```
{
    "timestamp": "2021-05-19T14:55:08.187Z",
    "account": {
        "id": "11111111-1111-1111-1111-aaaaaaaaaaaa"
    },
    "web": {
        "href": "https://mycompany.web.amer-1.jumio.ai/web/v4/app?authorizationToken=xxx&locale=es"
    },
    "sdk": {
        "token": "xxx"
    },
    "workflowExecution": {
        "id": "22222222-2222-2222-2222-aaaaaaaaaaaa",
        "credentials": [
            {
                "id": "33333333-3333-3333-aaaaaaaaaaaa",
                "category": "ID",
                "allowedChannels": [
                    "WEB",
                    "SDK"
                ]
            },
            {
                "id": "33333333-3333-3333-bbbbbbbbbbbb",
                "category": "FACEMAP",
                "allowedChannels": [
                    "WEB",
                    "SDK"
                ]
            },
            {
                "id": "33333333-3333-3333-cccccccccccc",
                "category": "SELFIE",
                "allowedChannels": [
                    "WEB",
                    "SDK"
                ]
            }
        ]
    }
}
```

## Retrieval
Refer to [Data Acquistion](kyx%2Bapi_guide.md#data-acquistion) section for more information on available channels (API, SDK, WEB).      
Use our [Retrieval API](kyx%2Bapi_guide.md#retrieval) to retrieve account data and/or data acquired during a workflow.

---
&copy; Jumio Corporation, 395 Page Mill Road, Suite 150 Palo Alto, CA 94306
