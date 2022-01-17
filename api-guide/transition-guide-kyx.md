# Introduction
This document is intended as a reference manual for the Jumio KYX API which is based on the new [Platform API](api.guide.md). It covers the APIs that are meant for the MVP workflow.

## Table of Contents
- [Authentication and Encryption](#authentication-and-encryption)
- [Account Creation](#account-creation)
- [Account Update](#account-update)
  - [Workflow Descriptions](#workflow-descriptions)
- [Callback](#callback)
- [Retrieval](#retrieval)
  - [Get Workflow Details](#get-workflow-details)
  - [Get Workflow Steps](#get-workflow-steps)
- [Deletion](#deletion)

# Authentication and Encryption
See [Platform API > Authentication and Encryption](api_guide.md#authentication-and-encryption)

# Account Creation
See [Platform API > Account Creation](api_guide.md#account-creation)

Please check out supported [Workflow Definition Keys](#workflow-definition-keys) for the KYX API.

# Account Update
See [Platform API > Account Update](api_guide.md#account-update)

Please check out supported [Workflow Definition Keys](#workflow-definition-keys) for the KYX API.

# Workflow Descriptions

## Workflow Definition Keys
| definitionKey | Name                         | Description  |
|---------------|------------------------------|--------------|
| 10013             | [ID and Identity Verification + Screening + Proof Of Residency + Address Validation + US DL Verification](#workflow-10013-id-and-identity-verification--screening--proof-of-residency--address-validation--us-dl-verification)  | This workflow verifies a photo ID document and returns a) whether that document is valid, and b) data extracted from that document. It also compares the user's face with the photo on the ID and performs a liveness check to ensure the person is physically present. Performs a proof of residency on the users address, as well as address validation. Checks if user is part of any sanctions list and also performs US Driving License Verification. |  

### Workflow 10013: ID and Identity Verification + Screening + Proof Of Residency + Address Validation + US DL Verification
This workflow verifies a photo ID document and returns a) whether that document is valid, and b) data extracted from that document. It also compares the user's face with the photo on the ID and performs a liveness check to ensure the person is physically present. Performs a proof of residency on the users address, as well as address validation. Checks if user is part of any sanctions list and also performs US Driving License Verification.

#### Required Credentials
* ID
* Selfie
* Facemap

#### Stored Credentials
* ID
* Selfie
* Facemap

#### Prerequisites
* Identity Verification must be enabled for your account. (Contact your Jumio account manager for activation.)
* Screening must be enabled for your account. (Contact your Jumio account manager for activation.)
* Screening must be set up for your Customer Portal (see [Portal Setup](https://github.com/Jumio/implementation-guides/blob/master/netverify/netverify-screening.md))
* Proof Of Residency must be enabled for your account. (Contact your Jumio account manager for activation.)
* Address Validation must be enabled for your account. (Contact your Jumio account manager for activation.)
* US DL Verification must be enabled for your account. (Contact your Jumio account manager for activation.)
* Allowed channels: API, SDK, WEB

#### Example

#### Initiate Account Creation
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

#### Initiate Account Update

HTTP Request Method: __PUT__
* US: `https://account.amer-1.jumio.ai/api/v1/accounts/<accountId>`
* EU: `https://account.emea-1.jumio.ai/api/v1/accounts/<accountId>`
* SG: `https://account.apac-1.jumio.ai/api/v1/accounts/<accountId>`

__Request:__
```
curl --request POST --location 'https://account.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa' \
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


# Callback
See [Platform API > Callback](api_guide.md#callback)

# Retrieval
See [Platform API > Retrieval](api_guide.md#retrieval) for a full list of parameters.

In addition to the listed parameters which can be returned for the Platform API, KYX will provide the following attributes when retrieving workflow details:
* [`workflow.decision`](#workflowdecision)
* [`workflow.steps`](#workflowsteps)

## Get Workflow Details
HTTP Request Method: __GET__
* US: `https://retrieval.amer-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>`
* EU: `https://retrieval.emea-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>`
* SG: `https://retrieval.apac-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>`

### Workflow Request Path Parameters

| Parameter           | Type   | Note                 |
|---------------------|--------|----------------------|
| accountId           | string | UUID of the account  |
| workflowExecutionId | string | UUID of the workflow |

### Workflow Execution Response
Unsuccessful requests will return HTTP status code __401 Unauthorized, 403 Forbidden__ or __404 Not Found__ if the scan is not available.

Successful requests will return HTTP status code __200 OK__ along with a JSON object containing the information described below.

| Parameter                          | Type   | Note                                                                   |
|------------------------------------|--------|------------------------------------------------------------------------|
| createdAt                          | string | Timestamp (UTC) of the creation.<br>Format: YYYY-MM-DDThh:mm:ss.SSSZ   |
| startedAt                          | string | Timestamp (UTC) of the start.<br>Format: YYYY-MM-DDThh:mm:ss.SSSZ      |
| completedAt                        | string | Timestamp (UTC) of the completion.<br>Format: YYYY-MM-DDThh:mm:ss.SSSZ |
| account                            | object | Possible values:<br>•	account.id                                       |
| account.id                         | string | UUID of the account                                                    |
| workflow                           | object | Possible values:<br>• workflow.id <br>• workflow.status <br>• workflow.definitionKey<br>• workflow.decision<br>• workflow.steps<br>• workflow.userReference<br>• workflow.customerInternalReference |
| workflow.id                        | string | UUID of the workflow                                                   |
| workflow.status                    | string | Possible values:<br>• INITIATED<br>• ACQUIRED<br>• PROCESSED<br>• SESSION_EXPIRED<br>•	TOKEN_EXPIRED   |
| workflow.definitionKey             | string | See [supported keys](#workflow-definition-keys)                        |
| workflow.decision                  | object | See [workflow.decision](#workflowdecision)                                                  |
| workflow.steps                     | object |See [workflow.steps](#workflowsteps)                        |
| workflow.userReference             | string | Customer internal reference for a request to link it in the customer backend (must not contain any PII) |
| workflow.customerInternalReference | string | Reference for the end user in the customer backend (must not contain any PII) |
| credentials                        | array (object)  | See [credentials](#credentials)                               |
| capabilities                       | object | See [capabilities](#capabilities)                                      |

#### workflow.decision
| Parameter        | Type   | Note                    |
|------------------|--------|-------------------------|
| risk               | object | Possible values:<br>• risk.score |
| risk.score         | double | Possible values:<br>• 0.00 - 100.00 (at max 2 decimal places) |
| type                    | string | Possible values:<br>• NOT_EXECUTED<br>• PASSED<br>• REJECTED<br>• WARNING  |
| details                 | object | Possible values:<br>• details.label        |
| details.label           | string | Possible values:<br>• NOT_EXECUTED<br>• PASSED<br>• REJECTED <br>• TECHNICAL_ERROR<br>• WARNING       |

#### workflow.step
| Parameter                 | Type   | Note                    |
|---------------------------|--------|-------------------------|
| href                      | string | href to manage steps for the workflow<br>see [Get Workflow Steps](#get-workflow-steps) |

#### credentials
| Parameter        | Type   | Note                    |
|------------------|--------|-------------------------|
| id               | string | UUID of the credentials |
| category         | string | ID                      |
| parts            | object | Possible values:<br>• parts.classifier<br>• parts.href  |
| parts.classifier | string | Possible values:<br>• FRONT<br>• BACK<br>•  FACE        |
| parts.href       | string | href to manage parts for the account credentials        |

#### capabilities
Since workflow execution consists of a chain of multiple capability executions (usability, extraction, liveness, ...), some have dependencies between them and need the result of previous executions.

This means that some capabilities should not be executed if any of the previous capabilities were not successful because they were REJECTED or NOT_EXECUTED. If, for example, __usability__ has passed, but __imageChecks__ got rejected with the reason DIGITAL_COPY, the consequent __extraction__ and __dataChecks__ cannot be executed because of PRECONDITION_NOT_FULFILLED. The precondition in this case would be to successfully pass __imageChecks__.

__Capability execution dependencies:__
 * usability --> imageChecks --> extraction --> dataChecks
 * usability --> liveness
 * usability --> similarity
 * usability --> authentication
 * usability --> imageChecks --> extraction --> watchlistScreening
 * usability --> imageChecks --> extraction --> addressValidation
 * usability --> imageChecks --> extraction --> proofOfResidency
 * usability --> imageChecks --> extraction --> drivingLicenseVerification

| Parameter              | Type  | Note            | Dependency     |
|------------------------|-------|-----------------|----------------|
| usability              | array (object) | See [usability](#capabilitiesusability)     | none      |
| liveness               | array (object) | See [liveness](#capabilitiesliveness)       | usability |
| similarity             | array (object) | See [similarity](#capabilitiessimilarity)   | usability |
| authentication         | array (object) | See [authentication](#capabilitiesauthentication)   | usability |
| imageChecks            | array (object) | See [imageChecks](#capabilitiesimageChecks) | usability |
| extraction             | array (object) | See [extraction](#capabilitiesextraction)   | usability, imageChecks |
| dataChecks             | array (object) | See [dataChecks](#capabilitiesdataChecks)   | usability, imageChecks, extraction |
| watchlistScreening     | array (object) | See [watchlistScreening](#capabilitieswatchlistScreening)   | usability, imageChecks, extraction |
| addressValidation      | array (object) | See [addressValidation](#capabilitiesaddressValidation)     | usability, imageChecks, extraction |
| proofOfResidency       | array (object) | See [proofOfResidency](#capabilitiesproofOfResidency)       | usability, imageChecks, extraction |
| drivingLicenseVerification | array (object) | See [drivingLicenseVerification](#capabilitiesdrivingLicenseVerification)   | usability, imageChecks, extraction |

#### capabilities.usability

__Dependency:__ none

| Parameter              | Type   | Note         |
|------------------------|--------|--------------|
| id                     | string | UUID of the capability     |
| credentials            | object |              |
| credentials.id         | string | UUID of the credentials             |
| credentials.category   | string | Possible values:<br>• ID<br>• FACEMAP<br>• DOCUMENT<br>• SELFIE           |
| decision               | object |              |
| decision.type          | string | Possible values:<br>• NOT_EXECUTED<br>• PASSED<br>• REJECTED<br>• WARNING |
| decision.details       | object |              |
| decision.details.label | string | if decision.type = NOT_EXECUTED:<br>• TECHNICAL_ERROR<br><br>if decision.type = PASSED:<br>• OK<br><br>if decision.type = REJECTED:<br>• BAD_QUALITY<br>• BLACK_WHITE<br>• MISSING_PAGE<br>• MISSING_SIGNATURE<br>• NOT_A_DOCUMENT<br>• PHOTOCOPY<br><br>if decision.type = WARNING:<br>• LIVENESS_UNDETERMINED<br>•  UNSUPPORTED_COUNTRY<br>• UNSUPPORTED_DOCUMENT_TYPE |

#### capabilities.liveness

__Dependency:__ [usability](#capabilitiesusability)

| Parameter              | Type   | Note                       |
|------------------------|--------|----------------------------|
| id                     | string | UUID of the capability     |
| credentials            | object |                            |
| credentials.id         | string | UUID of the credentials                           |
| credentials.category   | string | Possible values:<br>• ID<br>• FACEMAP<br>• DOCUMENT<br>• SELFIE                         |
| decision               | object |                            |
| decision.type          | string | Possible values:<br>• NOT_EXECUTED<br>• PASSED<br>• REJECTED<br>• WARNING |
| decision.details       | object |                            |
| decision.details.label | string | if decision.type = REJECTED:<br>• LIVENESS_UNDETERMINED<br>• ID_USED_AS_SELFIE<br>• MULTIPLE_PEOPLE<br>• DIGITAL_COPY<br>• PHOTOCOPY<br>• MANIPULATED<br>• NO_FACE_PRESENT<br>• FACE_NOT_FULLY_VISIBLE<br>• BLACK_WHITE<br><br>if decision.type = PASSED:<br>• OK<br><br>if decision.type = WARNING:<br>• AGE_DIFFERENCE<br>• BAD_QUALITY<br><br>if decision.type = NOT_EXECUTED:<br>• PRECONDITION_NOT_FULFILLED<br>• TECHNICAL_ERROR |
| data                   | object |              |
| data.type              | object | Possible values:<br>• IPROOV_STANDARD<br>• IPROOV_PREMIUM (SDK channel only)<br>• JUMIO_STANDARD |
| validFaceMapForAuthentication   | string | href to manage facemap   |

#### capabilities.similarity

__Dependency:__ [usability](#capabilitiesusability)

| Parameter              | Type   | Note         |
|------------------------|--------|--------------|
| id                     | string | UUID of the capability     |
| credentials            | object |              |
| credentials.id         | string | UUID of the credentials             |
| credentials.category   | string | Possible values:<br>• ID<br>• FACEMAP<br>• DOCUMENT<br>• SELFIE           |
| decision               | object |              |
| decision.type          | string | Possible values:<br>• NOT_EXECUTED<br>• PASSED<br>• REJECTED<br>• WARNING |
| decision.details       | object |              |
| decision.details.label | string | if decision.type = REJECTED:<br>• NO_MATCH<br><br>if decision.type = PASSED:<br>• MATCH<br><br>if decision.type = WARNING:<br>• NOT_POSSIBLE<br><br>if decision.type = NOT_EXECUTED:<br>• PRECONDITION_NOT_FULFILLED<br>• TECHNICAL_ERROR |
| data                   | object |              |
| data.similarity        | string | Possible values:<br>• MATCH<br>• NOT_MATCH<br>• NOT_POSSIBLE |

#### capabilities.authentication

__Dependency:__ [usability](#capabilitiesusability)

| Parameter              | Type   | Note                       |
|------------------------|--------|----------------------------|
| id                     | string | UUID of the capability     |
| credentials            | object |                            |
| credentials.id         | string | UUID of the credentials                           |
| credentials.category   | string | Possible values:<br>• ID<br>• FACEMAP<br>• DOCUMENT<br>• SELFIE                         |
| decision               | object |                            |
| decision.type          | string | Possible values:<br>• NOT_EXECUTED<br>• PASSED<br>• REJECTED    |
| decision.details       | object |                            |
| decision.details.label | string | if decision.type = PASSED:<br>• OK<br><br>if decision.type =REJECTED:<br>• FAILED<br><br>if decision.type = NOT_EXECUTED:<br>• PRECONDITION_NOT_FULFILLED<br>• TECHNICAL_ERROR |
| data                   | object |              |
| data.type              | object | Possible values:<br>• IPROOV_STANDARD<br>• IPROOV_PREMIUM (SDK channel only) |
| validFaceMapForAuthentication   | string | href to manage facemap                                 |

#### capabilities.imageChecks

__Dependency:__ [usability](#capabilitiesusability)

| Parameter              | Type   | Note                       |
|------------------------|--------|----------------------------|
| id                     | string | UUID of the capability     |
| credentials            | object |                            |
| credentials.id         | string | UUID of the credentials                           |
| credentials.category   | string | Possible values:<br>• ID<br>• FACEMAP<br>• DOCUMENT<br>• SELFIE                         |
| decision               | object |                            |
| decision.type          | string | Possible values:<br>• NOT_EXECUTED<br>• PASSED<br>• REJECTED<br>• WARNING |
| decision.details       | object |                            |
| decision.details.label | string | if decision.type = NOT_EXECUTED:<br>• PRECONDITION_NOT_FULFILLED<br>• TECHNICAL_ERROR<br><br>if decision.type = PASSED:<br>• OK<br><br>if decision.type = REJECTED:<br>• DIGITAL_COPY<br>• WATERMARK<br>• MANIPULATED_DOCUMENT<br>• OTHER_REJECTION<br>• GHOST_IMAGE_DIFFERENT<br>• PUNCHED<br>• SAMPLE<br>• CHIP_MISSING<br>• FAKE<br><br>if decision.type = WARNING:<br>• DIFFERENT_PERSON<br>• REPEATED_FACE |
| data                   | object | See [imageChecks.data](#capabilitiesimageChecksdata)             |

#### capabilities.imageChecks.data
| Parameter                         | Type   | Note                                                                                                        |
|-----------------------------------|--------|-------------------------------------------------------------------------------------------------------------|
| data.faceSearchFindings     | object | Result of 1:n face search on previous transactions |
| data.faceSearchFindings.status     | string | Possible values:<br>• DONE<br>• PENDING<br>• ERROR |
| data.faceSearchFindings.findings     | array (string) | A face (on the ID or Selfie) is matching with another face (on the ID or Selfie) from a previous transaction|
| data.faceSearchFindings.findings.items    | string | UUID of the workflow |

#### capabilities.extraction

__Dependencies:__ [usability](#capabilitiesusability), [imageChecks](#capabilitiesimageChecks)

| Parameter              | Type   | Note                       |
|------------------------|--------|----------------------------|
| id                     | string | UUID of the capability        |
| credentials            | object | Possible values:<br>• credentials.decision <br>• credentials.data        |
| decision               | object | Possible values:<br>• decision.type<br>• decision.details                |
| decision.type          | string | Possible values:<br>• NOT_EXECUTED <br>• PASSED                          |
| decision.details       | object | Possible values:<br>• decision.details.label                             |
| decision.details.label | string | if decision.type = NOT_EXECUTED:<br>• PRECONDITION_NOT_FULFILLED<br>• TECHNICAL_ERROR<br><br>if decision.type = PASSED:<br>• OK |
| data                   | object | See [extraction.data](#capabilitiesextractiondata)                       |

#### capabilities.extraction.data

| Parameter                         | Type   | Note                                                                                                        |
|-----------------------------------|--------|-------------------------------------------------------------------------------------------------------------|
| data.type                         | string | Possible values:<br>• PASSPORT<br>• DRIVING_LICENSE<br>• ID_CARD<br>• VISA<br>• UNSUPPORTED                 |
| data.subType                      | string | Possible values if data.type = ID_CARD:<br>• NATIONAL_ID<br>• CONSULAR_ID<br>• ELECTORAL_ID<br>• RESIDENT_PERMIT_ID <br>• TAX_ID <br>• STUDENT_ID <br>• PASSPORT_CARD_ID <br>• MILITARY_ID <br>• PUBLIC_SAFETY_ID <br>• HEALTH_ID <br>• OTHER_ID <br>• VISA <br>• UNKOWN<br> <br> Possible values if data.type = DRIVING_LICENSE:<br>• REGULAR_DRIVING_LICENSE <br>• LEARNING_DRIVING_LICENSE <br><br>Possible values if data.type = PASSPORT:<br>• E_PASSPORT (mobile only)   |
| data.issuingCountry               | string | [ISO 3166-1 alpha-3 country code](http://en.wikipedia.org/wiki/ISO_3166-1_alpha-3)                          |
| data.firstName                    | string | First name of the user as available on the ID if enabled, otherwise if provided                             |
| data.lastName                     | string | Last name of the customer as available on the ID if enabled, otherwise if provided                          |
| data.dateOfBirth                  | string |                                                                                                             |
| data.expiryDate                   | string |                                                                                                             |
| data.issuingDate                  | string |                                                                                                             |
| data.documentNumber               | string |                                                                                                             |
| data.state                        | string | Possible values:<br>• Last two characters of ISO 3166-2: US state code<br>• Last 2-3 characters of ISO 3166-2: AU state code<br>• Last two characters of ISO 3166-2: CA state code<br>• ISO 3166-1 country name<br>• XKX (Kosovo)<br>• Free text if it can't be mapped to a state/country code                                         |
| data.personalNumber               | string | Personal number of the document, if idType = PASSPORT and if data available on the document <br>(activation required)       |
| data.optionalMrzField1            | string | Optional field of MRZ line 1                                                                                |
| data.optionalMrzField2            | string | Optional field of MRZ line 2                                                                                |
| data.address                      | string | See [data.address](#capabilitiesextractiondataaddress) <br>(activation required)                            |
| data.issuingAuthority             | string | Issuing authority of the document <br>(activation required)                                                  |
| data.issuingPlace                 | string | Issuing authority of the document <br>(activation required)                                                  |
| data.curp                         | string | CURP for Mexican documents <br>(activation required)                                                         |
| data.gender                       | string | Possible values: <br>• M<br>• F                                                                             |
| data.nationality                  | string | Possible values:<br>• [ISO 3166-1 alpha-3 country code](http://en.wikipedia.org/wiki/ISO_3166-1_alpha-3)<br>(activation required) |
| data.placeOfBirth                 | string | Place of birth of document holder                                                                         |
| data.taxNumber                    | string | Tax number of the document <br>if country = ITA and type = HEALTH_ID, TAX_ID <br>(activation required)    |
| data.cpf                          | string | CPF number of the document (activation required)             |
| data.registrationNumber           | string | Registration number of the document (activation required)    |
| data.mothersName                  | string | Name of the document holder's mother (activation required)   |
| data.fathersName                  | string | Name of the document holder's father (activation required)   |
| data.personalIdentificationNumber | string | Personal identification number as available on the ID<br>• if idCountry = GEO and idSubtype = PASSPORT<br>• if idCountry = COL and idSubtype = ID_CARD<br>• if idCountry = LTU and idSubtype = DRIVING_LICENSE<br>• if idCountry = TUR and idSubtype = ID_CARD, DRIVING_LICENSE<br>• if idCountry = ROU and idSubtype = ID_CARD, DRIVING_LICENSE <br>(activation required) |
| data.rgNumber                     | string | "General Registration" number <br>if idCountry = BRA <br>(activation required)  |
| data.dlCategories                 | array  | Category of driving license |
| data.voterIdNumber                | string | Voter ID number <br>if idCountry = MEX <br>(activation required) |
| data.issuingNumber                | string | Issuing number <br>if idCountry = MEX <br>(activation required)  |
| data.passportNumber               | string | Passport number <br>if idType = VISA <br>(activation required)   |
| data.durationOfStay               | string | Duration of stay <br>if idType = VISA <br>(activation required)  |
| data.numberOfEntries              | string | Number of entries <br>if idType = VISA <br>(activation required) |
| data.visaCategory                 | string | Visa category <br>if idType = VISA <br>(activation required)     |
| data.dni                          | string | DNI ("Documento nacional de identidad") number as available on the ID <br>if idCountry = ESP and idSubType = NATIONA_ID <br>(activation required) |
| data.pesel                        | string | PESEL ("Powszechny Elektroniczny System Ewidencji Ludności") number as available on the ID <br>if idCountry = POL  <br>(activation required) |

#### capabilities.extraction.data.address

| Parameter              | Type   | Note                       |
|------------------------|--------|----------------------------|
| line1            | string | Line item 1                      |
| line2            | string | Line item 2                      |
| line3            | string | Line item 3                      |
| line4            | string | Line item 4                      |
| line5            | string | Line item 5                      |
| country          | string | Possible values: <br>• [ISO 3166-1 alpha-3 country code](http://en.wikipedia.org/wiki/ISO_3166-1_alpha-3) <br>• XKX (Kosovo)	 |
| postalCode       | string | Postal code                      |
| subdivision      | string | Subdivision (Region, State, Province, Emirate, Department, ...) |
| city             | string | City                             |
| formattedAddress | string | Complete address in a formatted way |

#### capabilities.dataChecks

__Dependencies:__ [usability](#capabilitiesusability), [imageChecks](#capabilitiesimageChecks), [extraction](#capabilitiesextraction)

| Parameter              | Type   | Note                       |
|------------------------|--------|----------------------------|
| id                     | string | UUID of the capability     |
| credentials            | object |                            |
| credentials.id         | string | UUID of the credentials                           |
| credentials.category   | string | Possible values:<br>• ID<br>• FACEMAP<br>• DOCUMENT<br>• SELFIE                         |
| decision               | object |                            |
| decision.type          | string | Possible values:<br>• NOT_EXECUTED<br>• PASSED<br>• REJECTED |
| decision.details       | object |                            |
| decision.details.label | string | if decision.type = NOT_EXECUTED:<br>• PRECONDITION_NOT_FULFILLED<br>• TECHNICAL_ERROR<br><br>if decision.type = PASSED:<br>• OK<br><br>if decision.type = REJECTED:<br>• NFC_CERTIFICATE<br>• MISMATCHING_DATAPOINTS<br>• MRZ_CHECKSUM<br>• MISMATCHING_DATA_REPEATED_FACE |

#### capabilities.watchlistScreening

__Dependencies:__ [usability](#capabilitiesusability), [imageChecks](#capabilitiesimageChecks), [extraction](#capabilitiesextraction)

| Parameter              | Type   | Note                       |
|------------------------|--------|----------------------------|
| id                     | string | UUID of the capability     |
| credentials            | object |                            |
| credentials.id         | string | UUID of the credentials                           |
| credentials.category   | string | Possible values:<br>• ID<br>• FACEMAP<br>• DOCUMENT<br>• SELFIE                         |
| decision               | object |                            |
| decision.type          | string | Possible values:<br>• NOT_EXECUTED<br>• PASSED<br>• WARNING |
| decision.details       | object |                            |
| decision.details.label | string | if decision.type = NOT_EXECUTED:<br>• NOT_ENOUGH_DATA<br>• VALIDATION_FAILED<br>• INVALID_MERCHANT_SETTINGS<br>• TECHNICAL_ERROR<br>• EXTRACTION_NOT_DONE<br>• NO_VALID_ID_CREDENTIAL<br><br>if decision.type = PASSED:<br>• OK<br><br>if decision.type = WARNING:<br>• ALERT |
| data                   | object | See [watchlistScreening.data](#capabilitieswatchlistScreeningdata)        |

#### capabilities.watchlistScreening.data

| Parameter              | Type   | Note                       |
|------------------------|--------|----------------------------|
| searchDate             | string | Timestamp (UTC) of the response.<br>Format: YYYY-MM-DDThh:mm:ss.SSSZ    |
| searchStatus           | string | Possible values:<br>• DONE<br>• NOT_DONE<br>• ERROR                      |
| searchId               | string | Only if searchStatus = DONE                      |
| searchReference        | string | Only if searchStatus = DONE                      |
| searchResultUrl        | string | Only if searchStatus = DONE                      |
| searchResults          | integer | Only if searchStatus = DONE                     |

#### capabilities.addressValidation

__Dependencies:__ [usability](#capabilitiesusability), [imageChecks](#capabilitiesimageChecks), [extraction](#capabilitiesextraction)

| Parameter              | Type   | Note                       |
|------------------------|--------|----------------------------|
| id                     | string | UUID of the capability     |
| credentials            | object |                            |
| credentials.id         | string | UUID of the credentials                           |
| credentials.category   | string | Possible values:<br>• ID<br>• FACEMAP<br>• DOCUMENT<br>• SELFIE                         |
| decision               | object |                            |
| decision.type          | string | Possible values:<br>• NOT_EXECUTED<br>• PASSED<br>• REJECTED<br>• WARNING |
| decision.details       | object |                            |
| decision.details.label | string | if decision.type = NOT_EXECUTED:<br>• NOT_ENOUGH_DATA<br>• TECHNICAL_ERROR<br>• UNSUPPORTED_COUNTRY<br><br>if decision.type = PASSED<br>• OK<br><br>if decision.type = REJECTED:<br>• DENY<br><br>if decision.type = WARNING:<br>• ALERT |

#### capabilities.proofOfResidency

__Dependencies:__ [usability](#capabilitiesusability), [imageChecks](#capabilitiesimageChecks), [extraction](#capabilitiesextraction)

| Parameter              | Type   | Note                       |
|------------------------|--------|----------------------------|
| id                     | string | UUID of the capability     |
| credentials            | object |                            |
| credentials.id         | string | UUID of the credentials                           |
| credentials.category   | string | Possible values:<br>• ID<br>• FACEMAP<br>• DOCUMENT<br>• SELFIE                         |
| decision               | object |                            |
| decision.type          | string | Possible values:<br>• NOT_EXECUTED<br>• PASSED<br>• REJECTED<br>• WARNING |
| decision.details       | object |                            |
| decision.details.label | string | if decision.type = NOT_EXECUTED:<br>• NOT_ENOUGH_DATA<br>• TECHNICAL_ERROR<br>• UNSUPPORTED_COUNTRY<br><br>if decision.type = PASSED:<br>• OK<br><br>if decision.type = REJECTED:<br>• DENY<br><br>if decision.type = WARNING:<br>• ALERT |

#### capabilities.drivingLicenseVerification

__Dependencies:__ [usability](#capabilitiesusability), [imageChecks](#capabilitiesimageChecks), [extraction](#capabilitiesextraction)

| Parameter              | Type   | Note                       |
|------------------------|--------|----------------------------|
| id                     | string | UUID of the capability     |
| credentials            | object |                            |
| credentials.id         | string | UUID of the credentials                           |
| credentials.category   | string | Possible values:<br>• ID<br>• FACEMAP<br>• DOCUMENT<br>• SELFIE                         |
| decision               | object |                            |
| decision.type          | string | Possible values:<br>• NOT_EXECUTED<br>• PASSED<br>• REJECTED<br>• WARNING |
| decision.details       | object |                            |
| decision.details.label | string | if decision.type = NOT_EXECUTED:<br>• TECHNICAL_ERROR<br>• UNSUPPORTED_COUNTRY<br>• UNSUPPORTED_STATE<br>• VALIDATION_FAILED<br><br>if decision.type = PASSED:<br>• OK<br><br>if decision.type = REJECTED:<br>• DENY<br><br>if decision.type = WARNING:<br>• ALERT |

### Examples

#### Request
```
GET
/api/v1/accounts/11111111-xxxx-xxxx-xxxx-xxxxxxxxxxxx/workflow-executions/22222222-xxxx-xxxx-xxxx-xxxxxxxxxxxx HTTP/1.1
Host: retrieval.apac-1.jumio.ai
User-Agent: User Demo
Authorization: Bearer xxx
```

#### Response
```
{
    "workflow": {
        "id": "22222222-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "status": "PROCESSED",
        "definitionKey": "10013"
    },
    "account": {
        "id": "11111111-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    },
    "createdAt": "2021-03-12T15:47:17.234Z",
    "startedAt": "2021-03-12T15:49:32.202Z",
    "completedAt": "2021-03-12T15:49:33.421Z",
    "credentials": [
        {
            "id": "33333333-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "category": "ID",
            "parts": [
                {
                    "classifier": "FRONT",
                    "href": "https://retrieval.amer-1.jumio.ai/api/v1/accounts/11111111-xxxx-xxxx-xxxx-xxxxxxxxxxxx/credentials/33333333-xxxx-xxxx-xxxx-xxxxxxxxxxxx/parts/FRONT"
                }
            ]
        }
    ],
    "capabilities": {
        "extraction": [
            {
                "id": "1a11111-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                "credentials": [
                    {
                        "id": "33333333-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                        "category": "ID"
                    }
                ],
                "decision": {
                    "type": "PASSED",
                    "details": {
                        "label": "OK"
                    }
                },
                "data": {
                    "type": "PASSPORT",
                    "subType": "E_PASSPORT",
                    "firstName": "JANE",
                    "lastName": "DOE",
                    "dateOfBirth": "1990-01-01",
                    "expiryDate": "2023-12-01",
                    "issuingDate": "2014-01-01",
                    "documentNumber": "xxxxxxxx",
                    "personalNumber": "<<<<<<<<<<<<<<",
                    "address": {
                        "country": "AUT",
                        "formattedAddress": "AUT"
                    }
                }
            }
        ],
        "dataChecks": [
            {
                "id": "1b11111-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                "credentials": [
                    {
                        "id": "33333333-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                        "category": "ID"
                    }
                ],
                "decision": {
                    "type": "PASSED",
                    "details": {
                        "label": "OK"
                    }
                }
            }
        ],
        "imageChecks": [
            {
                "id": "1c11111-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                "credentials": [
                    {
                        "id": "33333333-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                        "category": "ID"
                    }
                ],
                "decision": {
                    "type": "PASSED",
                    "details": {
                        "label": "OK"
                    }
                }
            }
        ],
        "usability": [
            {
                "id": "1d11111-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                "credentials": [
                    {
                        "id": "33333333-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                        "category": "ID"
                    }
                ],
                "decision": {
                    "type": "PASSED",
                    "details": {
                        "label": "OK"
                    }
                }
            }
            {
                "id": "1f11111-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                "credentials": [
                    {
                        "id": "44444444-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                        "category": "SELFIE"
                    }
                ],
                "decision": {
                    "type": "PASSED",
                    "details": {
                        "label": "OK"
                    }
                }
            },
            {
                "id": "1g11111-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                "credentials": [
                    {
                        "id": "55555555-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                        "category": "FACEMAP"
                    }
                ],
                "decision": {
                    "type": "PASSED",
                    "details": {
                        "label": "OK"
                    }
                }
            }
        ]
        "watchlistScreening": [
             {
                     "id": "1411111-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                     "credentials": [
                         {
                             "id": "33333333-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                             "category": "ID"
                         }
                     ],
                     "decision": {
                         "type": "PASSED",
                         "details": {
                             "label": "OK"
                         }
                     },
                     "data": {
                      "searchDate": "2021-07-15T10:44:11.000Z",
                      "searchId": "12345678",
                      "searchReference": "1626345851-xxxxxx",
                      "searchResultUrl": "https://app.xxx.com/public/search/123456XXC-xxxx/xxxccc",
                      "searchResults": 0,
                      "searchStatus": "SUCCESS"
                    }
                 }
             ],
          “addressValidation”: [
              {
                     "id": "1511111-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                     "credentials": [
                         {
                             "id": "33333333-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                             "category": "ID"
                         }
                     ],
                     "decision": {
                         "type": "PASSED",
                         "details": {
                             "label": "OK"
                         }
                     },
            }
          ],
          “proofOfResidency”: [
           {
                     "id": "1611111-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                     "credentials": [
                         {
                             "id": "33333333-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                             "category": "ID"
                         }
                     ],
                     "decision": {
                         "type": "PASSED",
                         "details": {
                             "label": "OK"
                         }
                     },
            }
          ],
          "drivingLicenseVerification": [
          {
                     "id": "1711111-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                     "credentials": [
                         {
                             "id": "33333333-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                             "category": "ID"
                         }
                     ],
                     "decision": {
                         "type": "PASSED",
                         "details": {
                             "label": "OK"
                         }
                     },
            }
         ]
         }
     }
```

## Get Workflow Steps
HTTP Request Method: __GET__
* US: `https://retrieval.amer-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>/steps`
* EU: `https://retrieval.emea-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>/steps`
* SG: `https://retrieval.apac-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>/steps`


#### Workflow Steps Request Path Parameters
| Parameter           | Type   | Note                                               |
|---------------------|--------|----------------------------------------------------|
| accountId           | string | UUID of the account                                |
| workflowExecutionId | string | UUID of the workflow                               |

#### Workflow Steps Execution Response
Unsuccessful requests will return the relevant [HTTP status code](https://tools.ietf.org/html/rfc7231#section-6) and information about the cause of the error. HTTP status code __404 Not Found__ will be returned if the transaction is not available, has been deleted, or is not a KYX workflow.

Successful requests will return HTTP status code __200 OK__ along with a JSON object containing the information described below.

| Parameter                 | Type   | Note                    |
|---------------------------|--------|-------------------------|
| steps                        | array (object) | See [steps](#steps) |

#### steps
| Parameter                 | Type   | Note                    |
|---------------------------|--------|-------------------------|
| id                        | string | UUID of the workflow step |
| name                      | string | Workflow step name<br>Possible values:<br>• ADDRESS_VERIFICATION<br>• DL_VERIFICATION<br>• ID_IV<br>• SCREENING |
| capabilityIds             | array (string) | List of the capabilityIds executed for this step |
| decision                  | object | Possible values:<br>• decision.risk<br>• decision.risk.score<br>• decision.type<br>• decision.details<br>• decision.details.label |
| decision.risk        | object | Possible values:<br>• decision.risk.score |
| decision.risk.score  | double | Possible values:<br>• 0.00 - 100.00 (max 2 decimal places)                      |
| decision.type             | string | Possible values:<br>• NOT_EXECUTED<br>• PASSED<br>• REJECTED<br>• WARNING  |
| decision.details          | object | Possible values:<br>• decision.details.label        |
| decision.details.label             | string | Possible values:<br>• NOT_EXECUTED<br>• PASSED<br>• REJECTED<br>• WARNING        |

### Examples

#### Request
```
GET
/api/v1/accounts/11111111-xxxx-xxxx-xxxx-xxxxxxxxxxxx/workflow-executions/22222222-xxxx-xxxx-xxxx-xxxxxxxxxxxx/steps HTTP/1.1
Host: retrieval.apac-1.jumio.ai
User-Agent: User Demo
Authorization: Bearer xxx
```

#### Response
```
{
 "steps": [
   {
     "id": "44111111-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
     "name": "ID_IV",
     "capabilityIds": [
       "1111111-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
       "1211111-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
       "1333111-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
       "1311111-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
     ],
     "decision": {
       "type": "PASSED",
       "details": {
         "label": "PASSED"
       }
     }
   },
   {
     "id": "44211111-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
     "name": "SCREENING",
     "capabilityIds": [
       "1411111-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
     ],
     "decision": {
       "type": "PASSED",
       "details": {
         "label": "PASSED"
       }
     }
   },
   {
     "id": "44311111-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
     "name": "ADDRESS_VERIFICATION",
     "capabilityIds": [
       "1511111-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
       “1611111-xxxx-xxxx-xxxx-xxxxxxxxxxxx”

     ],
     "decision": {
       "type": "PASSED",
       "details": {
         "label": "PASSED"
       }
     }
   },
{
     "id": "44411111-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
     "name": "DL_VERIFICATION",
     "capabilityIds": [
       "1711111-xxxx-xxxx-xxxx-xxxxxxxxxxxx”
     ],
     "decision": {
       "type": "PASSED",
       "details": {
         "label": "PASSED"
       }
     }
   }
 ]
}
```

# Deletion
See [Platform API > Deletion](api_guide.md#deletion)

---
&copy; Jumio Corporation, 395 Page Mill Road, Suite 150 Palo Alto, CA 94306
