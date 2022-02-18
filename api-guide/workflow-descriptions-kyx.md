# Workflow Descriptions

## Table of Contents
- [Workflow 10005: Standalone CPF Validation](#workflow-10005-standalone-cpf-validation)
- [Workflow 10006: Standalone Serpro Biometric](#workflow-1000-standalone-serpro-biometric)
- [Workflow 10010: Standalone Watchlist Screening](#workflow-10010-standalone-watchlist-screening)
- [Workflow 10011: Standalone ID and Identity Verification](#workflow-10011-standalone-id-and-identity-verification)
- [Workflow 10013: ID and Identity Verification + Screening + Proof Of Residency + Address Validation + US DL Verification](#workflow-10013-id-and-identity-verification--screening--proof-of-residency--address-validation--us-dl-verification)
- [Workflow 10014: Standalone Authentication ](#workflow-10014-standalone-authentication)
- [Retrieval](#retrieval-2)

Workflows are specified using the `key` attribute in the `workflowDefinition` object:
```
"workflowDefinition": {
    "key": DEFINITION_KEY,
    "credentials": []
}
```

## Workflow 10005: Standalone CPF Validation
This workflow validates the details provided by end-user against the Brazilian CPF database.

### Required Credentials XXX
* Data

### Stored Credentials XXX
* Data

### Prerequisites
* CPF number extraction must be enabled for your account. (Contact your Jumio account manager for activation.)
* Prepared data: personalNumber, firstName, lastName, address.country
* Allowed channels: API

### Example

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
    --header 'Authorization: Bearer
    YOUR_ACCESS_TOKEN' \
    --data-raw '{
        "customerInternalReference": "CUSTOMER_REFERENCE",
        "workflowDefinition": {
            "key": 10005
        }
    }'
```

__Response:__
```
{
    "timestamp": "2021-08-20T11:36:34.005Z",
    "account": {
        "id": "11111111-1111-1111-1111-aaaaaaaaaaaa"
    },
    "workflowExecution": {
        "id": "22222222-2222-2222-2222-aaaaaaaaaaaa",
        "credentials": [
            {
                "id": "33333333-3333-3333-aaaaaaaaaaaa",
                "category": "DATA",
                "allowedChannels": [
                    "API"
                ],
                "api": {
                    "token": "xxx",
                    "parts": {
                      "prepared_data": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/PREPARED_DATA",
                    },
                    "workflowExecution": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa"
                }
            }
        ]
    }
}
```

#### Upload Data
HTTP Request Method: __POST__
* US: `https://account.amer-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>/credentials/<credentialsId>/parts/PREPARED_DATA`
* EU: `https://account.emea-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>/credentials/<credentialsId>/parts/PREPARED_DATA`
* SG: `https://account.apac-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>/credentials/<credentialsId>/parts/PREPARED_DATA`

__Request Path Parameters:__

| Parameter           | Type   | Note          |
|---------------------|--------|---------------|
| accountId           | string | UUID of the account        |
| workflowExecutionId | string | UUID of the workflow       |
| credentialsId       | string | UUID of the credentials    |

__Request Body:__

| Parameter                      | Type           |  Notes          |
|--------------------------------|----------------|----------------|
| suffix  | string         | Name suffix of the user             |
| firstName  | string         | First name of the user              |
| middleName  | string         | Middle name of the user              |
| lastName  | string         | Last name of the user              |
| sex  | string         | Sex of the user              |
| dateOfBirth  | string         | Date of birth of the user<br>Format: YYYY-MM-DD              |
| socialSecurityNumber  | string         | Social security number              |
| nationality  | string         | Nationality of the user              |
| email  | string         | Email of the user<br>Format: abc@test.com              |
| phoneNumber  | string         | Phone number of the user              |
| phoneNumber2  | string         | Additional phone number of the user              |
| personalNumber  | string         | Personal number of the user              |
| address  | object         |               |
| address.line1  | string         | Line item 1              |
| address.line2  | string         | Line item 2              |
| address.line3  | string         | Line item 3              |
| address.line4  | string         | Line item 4              |
| address.line5  | string         | Line item 5              |
| address.postalCode  | string         | Postal code              |
| address.city  | string         | City              |
| address.subdivision  | string         | Subdivision (Region, State, Province, Emirate, Department, ...)              |
| address.country  | string         | Country              |
| id  | object         |               |
| id.issuingDate  | string         | Issuing date of the document<br>Format: YYYY-MM-DD              |
| id.expiryDate  | string         | Expiry date of the document<br>Format: YYYY-MM-DD              |
| id.idNumber  | string         | ID number of the document              |
| id.type  | string         | ID type of the document<br>Possible values:<br>• PASSPORT<br>• DRIVING_LICENSE<br>• ID_CARD              |

<!--
| kyc  | string         | TBD              |
-->

__Request:__
```
curl --request POST --location 'https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/PREPARED_DATA' \
    --header 'Content-Type: application/json' \
    --header 'User-Agent: User Demo' \
    --header 'Authorization: Bearer
    YOUR_ACCESS_TOKEN' \
    --data-raw '{
        "suffix": "Mr.",
        "firstName": "FIRSTNAME",
        "middleName": "MIDDLENAME",
        "lastName": "LASTNAME",
        "sex": "M",
        "dateOfBirth": "1985-01-01",
        "socialSecurityNumber": "111-11-1120",
        "email": "tmp@tmp.com",
        "phoneNumber": "+231-375-9663",
        "personalNumber": "1234567890123456",
        "address": {
            "line1": "123 HIGHLAND DRIVE",
            "line2": "Line 2",
            "line3": "Line 3",
            "line4": "Line 4",
            "line5": "Line 5",
            "postalCode": "12345",
            "city": "CITY",
            "subdivision": "Indiana",
            "country": "BRA"
        },
        "id": {
            "idNumber": "ABCD12345",  
            "issuingDate": "2014-02-20",
            "expiryDate": "2034-11-30",
            "type": "DRIVING_LICENSE"
      }
  }'
```

__Response:__
```
{
    "timestamp": "2021-08-20T11:36:34.005Z",
    "account": {
        "id": "11111111-1111-1111-1111-aaaaaaaaaaaa"
    },
    "workflowExecution": {
        "id": "22222222-2222-2222-2222-aaaaaaaaaaaa"
    },
    "api": {
        "parts": {
            "prepared_data": "https://api.amer-1.jumio.ai/api/v1/account/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/PREPARED_DATA"
        },
        "workflowExecution": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa"
    }
}
```

#### Finalize
HTTP Request Method: __PUT__
* US: `https://api.amer-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>`
* EU: `https://api.emea-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>`
* SG: `https://api.apac-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>`

__Request:__
```
curl --location --request PUT 'https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa' \
--header 'User-Agent: User Demo' \
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

#### Retrieve Data

Use our [Retrieval API](https://github.com/Jumio/implementation-guides/blob/master/api-guide/api-guide-kyx.md#retrieval) to retrieve account data and/or data acquired during a workflow.

<!--

TBD

https://retrieval.amer-1.jumio.ai/api/v1/accounts/14bc528c-f858-4029-87a1-d2ffdff2fa32/credentials/5f35a0e4-1719-491b-aeb0-169cbec56f89/parts/PREPARED_DATA

```
10005 Steps retrieval output
```
{
    "steps": [
        {
            "id": "7b4d5798-4c3c-4afc-bc9b-5b0af0e15f2a",
            "name": "BRA_CPF_VALIDATION",
            "decision": {
                "type": "REJECTED",
                "details": {
                    "label": "REJECTED"
                }
            },
            "capabilityIds": [
                "c6b2db51-fc84-4ca1-a0ba-ce25b07648cc"
            ]
        }
    ]
}
```

-->





## Workflow 10006: Standalone Serpro Biometric
This workflow uses the provided Selfie and the CPF ID of end-user and validates both against the database.

### Required Credentials XXX
* Selfie
* Data

### Stored Credentials XXX
* Selfie
* Data

### Prerequisites
<!-- * CPF number extraction must be enabled for your account. (Contact your Jumio account manager for activation.) TODO: activation needed??-->
* Prepared data: personalNumber
* Allowed channels: API

### Example

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
    --header 'Authorization: Bearer
    YOUR_ACCESS_TOKEN' \
    --data-raw '{
        "customerInternalReference": "CUSTOMER_REFERENCE",
        "workflowDefinition": {
            "key": 10006
        }
    }'
```

__Response:__
```
{
    "timestamp": "2021-08-20T11:36:34.005Z",
    "account": {
        "id": "11111111-1111-1111-1111-aaaaaaaaaaaa"
    },
    "workflowExecution": {
        "id": "22222222-2222-2222-2222-aaaaaaaaaaaa",
        "credentials": [
            {
                "id": "33333333-3333-3333-aaaaaaaaaaaa",
                "category": "DATA",
                "allowedChannels": [
                    "API"
                ],
                "api": {
                    "token": "xxx",
                    "parts": {
                      "prepared_data": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/PREPARED_DATA",
                    },
                    "workflowExecution": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa"
                }
            },
            {
                "id": "33333333-3333-3333-bbbbbbbbbbbb",
                "category": "SELFIE",
                "allowedChannels": [
                    "API"
                ],
                "api": {
                    "token": "xxx",
                    "parts": {
                      "prepared_data": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-bbbbbbbbbbbb/parts/FACE",
                    },
                    "workflowExecution": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-bbbbbbbbbbbb"
                }
            }
        ]
    }
}
```

#### Upload Data
HTTP Request Method: __POST__
* US: `https://account.amer-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>/credentials/<credentialsId>/parts/PREPARED_DATA`
* EU: `https://account.emea-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>/credentials/<credentialsId>/parts/PREPARED_DATA`
* SG: `https://account.apac-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>/credentials/<credentialsId>/parts/PREPARED_DATA`

__Request Path Parameters:__

| Parameter           | Type   | Note          |
|---------------------|--------|---------------|
| accountId           | string | UUID of the account        |
| workflowExecutionId | string | UUID of the workflow       |
| credentialsId       | string | UUID of the credentials    |

__Request Body:__

| Parameter                      | Type           |  Notes          |
|--------------------------------|----------------|----------------|
| suffix  | string         | Name suffix of the user             |
| firstName  | string         | First name of the user              |
| middleName  | string         | Middle name of the user              |
| lastName  | string         | Last name of the user              |
| sex  | string         | Sex of the user              |
| dateOfBirth  | string         | Date of birth of the user<br>Format: YYYY-MM-DD              |
| socialSecurityNumber  | string         | Social security number              |
| nationality  | string         | Nationality of the user              |
| email  | string         | Email of the user<br>Format: abc@test.com              |
| phoneNumber  | string         | Phone number of the user              |
| phoneNumber2  | string         | Additional phone number of the user              |
| personalNumber  | string         | Personal number of the user              |
| address  | object         |               |
| address.line1  | string         | Line item 1              |
| address.line2  | string         | Line item 2              |
| address.line3  | string         | Line item 3              |
| address.line4  | string         | Line item 4              |
| address.line5  | string         | Line item 5              |
| address.postalCode  | string         | Postal code              |
| address.city  | string         | City              |
| address.subdivision  | string         | Subdivision (Region, State, Province, Emirate, Department, ...)              |
| address.country  | string         | Country              |
| id  | object         |               |
| id.issuingDate  | string         | Issuing date of the document<br>Format: YYYY-MM-DD              |
| id.expiryDate  | string         | Expiry date of the document<br>Format: YYYY-MM-DD              |
| id.idNumber  | string         | ID number of the document              |
| id.type  | string         | ID type of the document<br>Possible values:<br>• PASSPORT<br>• DRIVING_LICENSE<br>• ID_CARD              |

<!--
| kyc  | string         | TBD              |
-->

__Request:__
```
curl --request POST --location 'https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/PREPARED_DATA' \
    --header 'Content-Type: application/json' \
    --header 'User-Agent: User Demo' \
    --header 'Authorization: Bearer
    YOUR_ACCESS_TOKEN' \
    --data-raw '{
        "suffix": "Mr.",
        "firstName": "FIRSTNAME",
        "middleName": "MIDDLENAME",
        "lastName": "LASTNAME",
        "sex": "M",
        "dateOfBirth": "1985-01-01",
        "socialSecurityNumber": "111-11-1120",
        "email": "tmp@tmp.com",
        "phoneNumber": "+231-375-9663",
        "personalNumber": "1234567890123456",
        "address": {
            "line1": "123 HIGHLAND DRIVE",
            "line2": "Line 2",
            "line3": "Line 3",
            "line4": "Line 4",
            "line5": "Line 5",
            "postalCode": "12345",
            "city": "CITY",
            "subdivision": "Indiana",
            "country": "BRA"
        },
        "id": {
            "idNumber": "ABCD12345",  
            "issuingDate": "2014-02-20",
            "expiryDate": "2034-11-30",
            "type": "DRIVING_LICENSE"
      }
  }'
```

__Response:__
```
{
    "timestamp": "2021-08-20T11:36:34.005Z",
    "account": {
        "id": "11111111-1111-1111-1111-aaaaaaaaaaaa"
    },
    "workflowExecution": {
        "id": "22222222-2222-2222-2222-aaaaaaaaaaaa"
    },
    "api": {
        "parts": {
            "prepared_data": "https://api.amer-1.jumio.ai/api/v1/account/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/PREPARED_DATA"
        },
        "workflowExecution": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa"
    }
}
```

#### Upload Face

HTTP Request Method: __POST__
* US: `https://api.amer-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>/credentials/<credentialsId>/parts/FACE`
* EU: `https://api.emea-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>/credentials/<credentialsId>/parts/FACE`
* SG: `https://api.apac-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>/credentials/<credentialsId>/parts/FACE`

__Request Path Parameters:__

| Parameter           | Type   | Note                                      |
|---------------------|--------|-------------------------------------------|
| accountId           | string | UUID of the account                       |
| workflowExecutionId | string | UUID of the workflow                      |
| credentialsId       | string | UUID of the credentials                   |

__Request Body:__

| Key  | Value                                                          |
|------|----------------------------------------------------------------|
| file | JPEG, PNG  (max. size 10 MB and max resolution of 8000 x 8000) |

__Request:__
```
curl --location --request POST 'https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-bbbbbbbbbbbb/parts/FACE' \
--header 'User-Agent: User Demo' \
--header 'Content-Type: multipart/form-data' \
--header 'Authorization: xxx' \
--form 'file=@"/J:/User/2020-03-01/faces/face.jpg"
```

__Response:__
```
{
    "timestamp": "2021-05-24T08:15:55.269Z",
    "account": {
        "id": "11111111-1111-1111-1111-aaaaaaaaaaaa"
    },
    "workflowExecution": {
        "id": "22222222-2222-2222-2222-aaaaaaaaaaaa"
    },
    "api": {
        "token": "xxx",
        "parts": {
            "face": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-bbbbbbbbbbbb/parts/FACE"
        },
        "workflowExecution": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa"
    }
}
```

#### Finalize
HTTP Request Method: __PUT__
* US: `https://api.amer-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>`
* EU: `https://api.emea-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>`
* SG: `https://api.apac-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>`

__Request:__
```
curl --location --request PUT 'https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa' \
--header 'User-Agent: User Demo' \
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

#### Retrieve Data

Use our [Retrieval API](https://github.com/Jumio/implementation-guides/blob/master/api-guide/api-guide-kyx.md#retrieval) to retrieve account data and/or data acquired during a workflow.



## Workflow 10010: Standalone Watchlist Screening
This workflow checks if the user is part of any sanctions list using Comply Advantage.

### Required Credentials
* Data

### Stored Credentials
* Data

### Prerequisites
* Screening must be enabled for your account. (Contact your Jumio account manager for activation.)
* Screening must be set up for your Customer Portal (see [Comply Advantantage Portal Setup](https://github.com/Jumio/implementation-guides/blob/master/netverify/netverify-screening.md))
* Allowed channels: API

### Example

#### Initiate Account Creation
HTTP Request Method: __POST__
* US: `https://account.amer-1.jumio.ai/api/v1/accounts`
* EU: `https://account.emea-1.jumio.ai/api/v1/accounts`
* SG: `https://account.apac-1.jumio.ai/api/v1/accounts`

| ⚠️&nbsp;&nbsp; `userReference` is required to be defined in the initiate for WF10010 in order for Screening to work.
|:----------|

__Request:__
```
curl --request POST --location 'https://account.amer-1.jumio.ai/api/v1/accounts' \
    --header 'Content-Type: application/json' \
    --header 'User-Agent: User Demo' \
    --header 'Authorization: Bearer
    YOUR_ACCESS_TOKEN' \
    --data-raw '{
        "customerInternalReference": "CUSTOMER_REFERENCE",
        "workflowDefinition": {
            "key": 10010
        },
        "callbackUrl": "YOUR_CALLBACK_URL",
        "userReference": "YOUR_USER_REFERENCE"
    }'
```

__Response:__
```
{
    "timestamp": "2021-08-20T11:36:34.005Z",
    "account": {
        "id": "11111111-1111-1111-1111-aaaaaaaaaaaa"
    },
    "workflowExecution": {
        "id": "22222222-2222-2222-2222-aaaaaaaaaaaa",
        "credentials": [
            {
                "id": "33333333-3333-3333-aaaaaaaaaaaa",
                "category": "DATA",
                "allowedChannels": [
                    "API"
                ],
                "api": {
                    "token": "xxx",
                    "parts": {
                      "prepared_data": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/PREPARED_DATA",
                    },
                    "workflowExecution": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa"
                }
            }
        ]
    }
}
```

#### Upload Data
HTTP Request Method: __POST__
* US: `https://account.amer-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>/credentials/<credentialsId>/parts/PREPARED_DATA`
* EU: `https://account.emea-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>/credentials/<credentialsId>/parts/PREPARED_DATA`
* SG: `https://account.apac-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>/credentials/<credentialsId>/parts/PREPARED_DATA`

__Request Path Parameters:__

| Parameter           | Type   | Note          |
|---------------------|--------|---------------|
| accountId           | string | UUID of the account        |
| workflowExecutionId | string | UUID of the workflow       |
| credentialsId       | string | UUID of the credentials    |

__Request Body:__

| Parameter                      | Type           |  Notes          |
|--------------------------------|----------------|----------------|
| suffix  | string         | Name suffix of the user             |
| firstName  | string         | First name of the user              |
| middleName  | string         | Middle name of the user              |
| lastName  | string         | Last name of the user              |
| sex  | string         | Sex of the user              |
| dateOfBirth  | string         | Date of birth of the user<br>Format: YYYY-MM-DD              |
| socialSecurityNumber  | string         | Social security number              |
| nationality  | string         | Nationality of the user              |
| email  | string         | Email of the user<br>Format: abc@test.com              |
| phoneNumber  | string         | Phone number of the user              |
| phoneNumber2  | string         | Additional phone number of the user              |
| personalNumber  | string         | Personal number of the user              |
| address  | object         |               |
| address.line1  | string         | Line item 1              |
| address.line2  | string         | Line item 2              |
| address.line3  | string         | Line item 3              |
| address.line4  | string         | Line item 4              |
| address.line5  | string         | Line item 5              |
| address.postalCode  | string         | Postal code              |
| address.city  | string         | City              |
| address.subdivision  | string         | Subdivision (Region, State, Province, Emirate, Department, ...)              |
| address.country  | string         | Country              |
| id  | object         |               |
| id.issuingDate  | string         | Issuing date of the document<br>Format: YYYY-MM-DD              |
| id.expiryDate  | string         | Expiry date of the document<br>Format: YYYY-MM-DD              |
| id.idNumber  | string         | ID number of the document              |
| id.type  | string         | ID type of the document<br>Possible values:<br>• PASSPORT<br>• DRIVING_LICENSE<br>• ID_CARD              |

<!--
| kyc  | string         | TBD              |
-->

__Request:__
```
curl --request POST --location 'https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/PREPARED_DATA' \
    --header 'Content-Type: application/json' \
    --header 'User-Agent: User Demo' \
    --header 'Authorization: Bearer
    YOUR_ACCESS_TOKEN' \
    --data-raw '{
        "suffix": "Mr.",
        "firstName": "FIRSTNAME",
        "middleName": "MIDDLENAME",
        "lastName": "LASTNAME",
        "sex": "M",
        "dateOfBirth": "1985-01-01",
        "socialSecurityNumber": "111-11-1120",
        "email": "tmp@tmp.com",
        "phoneNumber": "+231-375-9663",
        "personalNumber": "1234567890123456",
        "address": {
            "line1": "123 HIGHLAND DRIVE",
            "line2": "Line 2",
            "line3": "Line 3",
            "line4": "Line 4",
            "line5": "Line 5",
            "postalCode": "12345",
            "city": "CITY",
            "subdivision": "Indiana",
            "country": "BRA"
        },
        "id": {
            "idNumber": "ABCD12345",  
            "issuingDate": "2014-02-20",
            "expiryDate": "2034-11-30",
            "type": "DRIVING_LICENSE"
      }
  }'
```

__Response:__
```
{
    "timestamp": "2021-08-20T11:36:34.005Z",
    "account": {
        "id": "11111111-1111-1111-1111-aaaaaaaaaaaa"
    },
    "workflowExecution": {
        "id": "22222222-2222-2222-2222-aaaaaaaaaaaa"
    },
    "api": {
        "parts": {
            "prepared_data": "https://api.amer-1.jumio.ai/api/v1/account/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/PREPARED_DATA"
        },
        "workflowExecution": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa"
    }
}
```

#### Finalize
HTTP Request Method: __PUT__
* US: `https://api.amer-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>`
* EU: `https://api.emea-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>`
* SG: `https://api.apac-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>`

__Request:__
```
curl --location --request PUT 'https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa' \
--header 'User-Agent: User Demo' \
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
  --header 'User-Agent: User Demo' \
  --header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
  --data-raw '{
      "customerInternalReference": "CUSTOMER_REFERENCE",
      "workflowDefinition": {
          "key": 10011,
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
      "userReference": "YOUR_USER_REFERENCE",
      "tokenLifetime": "5m",
      "web":{
          "successUrl":"https://www.yourcompany.com/success",
          "errorUrl":"https://www.yourcompany.com/error",
          "locale":"es"
      }
  }'
```

__Response:__
```
{
    "timestamp": "2022-01-06T13:41:20.769Z",
    "account": {
        "id": "11111111-1111-1111-1111-aaaaaaaaaaaa"
    },
    "web": {
        "href": "https://mycompany.web.amer-1.jumio.ai/web/v4/app?authorizationTokenxxx&locale=es",
        "successUrl": "https://www.yourcompany.com/success",
        "errorUrl": "https://www.yourcompany.com/error"
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

__Request Headers__

The following fields are required in the header section of your request:

`Accept: application/json`    
`Content-Type: multipart/form-data`   
`Content-Length:` see [RFC-7230](https://tools.ietf.org/html/rfc7230#section-3.3.2)    
`Authorization:` see [RFC6749](https://tools.ietf.org/html/rfc6749)   
`User-Agent: YourCompany YourApp/v1.0`   

| ⚠️&nbsp;&nbsp; Jumio requires the __User-Agent__ value to reflect your business or entity name for API troubleshooting.
|:----------|

| ℹ️&nbsp;&nbsp; Calls with missing or suspicious headers, suspicious parameter values, or without OAuth2 will result in HTTP status code __403 Forbidden__
|:----------|

__Request URL__

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

__Response__
Unsuccessful requests will return HTTP status code __401 Unauthorized, 403 Forbidden__ or __404 Not Found__ if the scan is not available.

Successful requests will return HTTP status code __200 OK__ along with a JSON object containing the information described below.

##### Upload Document FRONT
__Request:__
```
curl --location --request POST 'https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/FRONT' \
--header 'User-Agent: User Demo' \
--header 'Authorization: Bearer xxx' \
--form 'file=@"/J:/User/2020-03-01/faces/front.jpg"'
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
    },
    "api": {
        "parts": {
            "front": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/FRONT",
            "back": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/BACK"
        },
        "workflowExecution": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa"
}
```

##### Upload Document BACK
__Request:__
```
curl --location --request POST 'https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-bbbbbbbbbbbb/parts/BACK' \
--header 'User-Agent: User Demo' \
--header 'Authorization: Bearer xxx' \
--form 'file=@"/J:/User/2020-03-01/faces/back.jpg"'
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
    },
    "api": {
        "parts": {
            "front": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/FRONT",
            "back": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/BACK"
        },
        "workflowExecution": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa"
}
```

##### Upload Document FACE
__Request:__
```
curl --location --request POST 'https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-bbbbbbbbbbbb/parts/FACE' \
--header 'User-Agent: User Demo' \
--header 'Authorization: Bearer xxx' \
--form 'file=@"/J:/User/2020-03-01/faces/face.jpg"'
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
    },
    "api": {
        "parts": {
            "front": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/FRONT",
            "back": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/BACK"
        },
        "workflowExecution": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa"
}
```

#### Finalize
HTTP Request Method: __PUT__
* US: `https://api.amer-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>`
* EU: `https://api.emea-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>`
* SG: `https://api.apac-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>`

__Request:__
```
curl --location --request PUT 'https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa' \
--header 'User-Agent: User Demo' \
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
                  "country": {
                    "predefinedType": "DEFINED",
                    "values": ["USA", "CAN", "AUT", "GBR"]
                  },
                  "type": {
                    "predefinedType": "DEFINED",
                    "values": ["DRIVING_LICENSE", "ID_CARD"]
                  }  
                }
            ],
            "capabilities": {
              "watchlistScreening": {
                "additionalProperties": "string"
              }
            }
        },
        "callbackUrl": "YOUR_CALLBACK_URL",
        "userReference": "YOUR_USER_REFERENCE",
        "tokenLifetime": "5m",
        "web":{
            "successUrl":"https://www.yourcompany.com/success",
            "errorUrl":"https://www.yourcompany.com/error",
            "locale":"es"
        }
    }'
```

__Response:__
```
{
    "timestamp": "2021-05-28T09:17:50.240Z",
    "account": {
        "id": "11111111-1111-1111-1111-aaaaaaaaaaaa"
    },
    "web": {
        "href": "https://mycompany.web.amer-1.jumio.ai/web/v4/app?authorizationTokenxxx&locale=es",
        "successUrl": "https://www.yourcompany.com/success",
        "errorUrl": "https://www.yourcompany.com/error"
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
                    "API",
                    "SDK"
                ],
                "api": {
                    "token": "xxx",
                    "parts": {
                        "front": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/FRONT",
                        "back": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/BACK"
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
                    "token": "xxx",
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

__Request Headers__

The following fields are required in the header section of your request:

`Accept: application/json`    
`Content-Type: multipart/form-data`   
`Content-Length:` see [RFC-7230](https://tools.ietf.org/html/rfc7230#section-3.3.2)    
`Authorization:` see [RFC6749](https://tools.ietf.org/html/rfc6749)   
`User-Agent: YourCompany YourApp/v1.0`   

| ⚠️&nbsp;&nbsp; Jumio requires the __User-Agent__ value to reflect your business or entity name for API troubleshooting.
|:----------|

| ℹ️&nbsp;&nbsp; Calls with missing or suspicious headers, suspicious parameter values, or without OAuth2 will result in HTTP status code __403 Forbidden__
|:----------|

__Request URL__

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

__Response__
Unsuccessful requests will return HTTP status code __401 Unauthorized, 403 Forbidden__ or __404 Not Found__ if the scan is not available.

Successful requests will return HTTP status code __200 OK__ along with a JSON object containing the information described below.


##### Upload Document FRONT
__Request:__
```
curl --location --request POST 'https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/FRONT' \
--header 'User-Agent: User Demo' \
--header 'Authorization: Bearer xxx' \
--form 'file=@"/J:/User/2020-03-01/faces/front.jpg"'
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
    },
    "api": {
        "parts": {
            "front": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/FRONT",
            "back": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/BACK"
        },
        "workflowExecution": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa"
}
```

##### Upload Document BACK
__Request:__
```
curl --location --request POST 'https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-bbbbbbbbbbbb/parts/BACK' \
--header 'User-Agent: User Demo' \
--header 'Authorization: Bearer xxx' \
--form 'file=@"/J:/User/2020-03-01/faces/back.jpg"'
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
    },
    "api": {
        "parts": {
            "front": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/FRONT",
            "back": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/BACK"
        },
        "workflowExecution": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa"
}
```

##### Upload Document FACE
__Request:__
```
curl --location --request POST 'https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-bbbbbbbbbbbb/parts/FACE' \
--header 'User-Agent: User Demo' \
--header 'Authorization: Bearer xxx' \
--form 'file=@"/J:/User/2020-03-01/faces/face.jpg"'
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
    },
    "api": {
        "parts": {
            "front": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/FRONT",
            "back": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa/credentials/33333333-3333-3333-aaaaaaaaaaaa/parts/BACK"
        },
        "workflowExecution": "https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa"
}
```

#### Finalize
HTTP Request Method: __PUT__
* US: `https://api.amer-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>`
* EU: `https://api.emea-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>`
* SG: `https://api.apac-1.jumio.ai/api/v1/accounts/<accountId>/workflow-executions/<workflowExecutionId>`

__Request:__
```
curl --location --request PUT 'https://api.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa/workflow-executions/22222222-2222-2222-2222-aaaaaaaaaaaa' \
--header 'User-Agent: User Demo' \
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

## Workflow 10014: Standalone Authentication
This workflow compares the facemap of a user to an already existing facemap that has already been captured.

The existing facemap must have been acquired during a previous workflow, e.g. [Workflow 10013](##workflow-10013-id-and-identity-verification--screening--proof-of-residency--address-validation--us-dl-verification).

### Required Credentials
* Selfie
* Facemap

### Stored Credentials
* Selfie
* Facemap

### Prerequisites
* Identity Verification must be enabled for your account. (Contact your Jumio account manager for activation.)
* Account already exists, `<accountId>` available
* Facemap already on file, acquired through previous workflow
* Allowed channels: SDK, WEB

### Capabilities
In order of dependency:
* [usability](api-guide-kyx.md#capabilitiesusability) --> [authentication](api-guide-kyx.md#capabilitiesauthentication)

### Example

### Initiate Account Creation
HTTP Request Method: __PUT__
* US: `https://account.amer-1.jumio.ai/api/v1/accounts/<accountId>`
* EU: `https://account.emea-1.jumio.ai/api/v1/accounts/<accountId>`
* SG: `https://account.apac-1.jumio.ai/api/v1/accounts/<accountId>`

__Request:__
```
curl --location --request PUT 'https://account.amer-1.jumio.ai/api/v1/accounts/11111111-1111-1111-1111-aaaaaaaaaaaa' \
--header 'Content-Type: application/json' \
--header 'User-Agent: User Demo' \
--header 'Authorization: Bearer xxx' \
--data-raw '{
    "customerInternalReference": "CUSTOMER_REFERENCE",
    "workflowDefinition": {
        "key": 10014
    }
}'
```

__Response:__
```
{
    "timestamp": "2021-05-19T15:28:15.711Z",
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
Refer to [Data Acquistion](https://github.com/Jumio/implementation-guides/blob/master/api-guide/api-guide-kyx.md#data-acquisition) section for more information on available channels (API, SDK, WEB).     
Use our [Retrieval API](https://github.com/Jumio/implementation-guides/blob/master/api-guide/api-guide-kyx.md#retrieval) to retrieve account data and/or data acquired during a workflow.

---
&copy; Jumio Corporation, 395 Page Mill Road, Suite 150 Palo Alto, CA 94306
