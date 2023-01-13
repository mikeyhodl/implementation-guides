![Jumio](/images/Jumio-ID-Verification-Banner.png)

# performNetverify Implementation Guide

This is a reference manual and configuration guide for the ID Verification API product. It illustrates how to implement the ID Verification API.

---
## Revision history



Information about changes to features and improvements documented in each release is available in our [Revision history](/netverify/README.md).


---

## Table of Contents
- [Release notes](#release-notes)
- [Using performNetverify](#using-performnetverify)
  - [Authentication and encryption](#authentication-and-encryption)
  - [Request headers](#request-headers)
  - [Request body](#request-body)
  - [Response](#response)
  - [Examples](#examples)
    - [Sample request](#sample-request)
    - [Sample response](#sample-response)
- [Configuring settings in the Customer Portal](#configuring-settings-in-the-customer-portal)
	- [Application settings - General](#application-settings--general)
		- [Callback URL](#callback-url)



---
# Using performNetverify

**performNetverify** offers a Restful ID verification API without Jumio-hosted user interface. Simply send an HTTP POST including the customer's ID image to the URL below. You will immediately receive a JSON response with a Jumio scan reference and a timestamp. A callback will be sent after the verification is complete (see [Callback](/netverify/callback.md)).

|⚠️ NOTE: Passports (MRZ) have the highest success rate among ID types. It is recommended for best conversion rates and better data extraction to suggest that your users upload passports when using this submission channel.
|:----------|

**HTTP Request Method:** `POST`<br>
**REST URL (US)**: `https://netverify.com/api/netverify/v2/performNetverify`<br>
**REST URL (EU)**: `https://lon.netverify.com/api/netverify/v2/performNetverify`<br>
**REST URL (SGP)**: `https://core-sgp.jumio.com/api/netverify/v2/performNetverify`<br>

## Authentication and encryption

ID Verification API calls are protected using [HTTP Basic Authentication](https://tools.ietf.org/html/rfc7617). Your Basic Auth credentials are constructed using your API token as the user-id and your API secret as the password. You can view and manage your API token and secret in the Customer Portal under **Settings > API credentials**.
<br>

|⚠️ Never share your API token, API secret, or Basic Auth credentials with *anyone* — not even Jumio Support.
|:----------|

The [TLS Protocol](https://tools.ietf.org/html/rfc5246) is required to securely transmit your data, and we strongly recommend using the latest version. For information on cipher suites supported by Jumio during the TLS handshake see [supported cipher suites](/netverify/supported-cipher-suites.md).

<br>

## Request headers

The following fields are required in the header section of your request:<br>

`Accept: application/json`<br>
`Content-Type: application/json`<br>
`Content-Length:`  (see [RFC-7230](https://tools.ietf.org/html/rfc7230#section-3.3.2))<br>
`Authorization:` (see [RFC 7617](https://tools.ietf.org/html/rfc7617))<br>
`User-Agent: YourCompany YourApp/v1.0`<br>

|ℹ️ Jumio requires the `User-Agent` value to reflect your business or entity name for API troubleshooting.|
|:---|

**Note:** Calls with missing or suspicious headers, suspicious parameter values, or without HTTP Basic Authentication will result in HTTP status code 403 Forbidden.
<br>

## Request body

|ℹ️ Values set in your API request will override the corresponding settings configured in the Customer Portal.
|:----------|

**Required fields appear in bold type.**

|Parameter|Type|Max. length|Description|
|:---|:---|:---|:---|
|**merchantIdScanReference**|String|100|Your reference for each scan must not contain sensitive data like PII (Personally Identifiable Information) or account login|
|**frontsideImage**|String|Max. 15MB & <8000 pixels per side & longest side >300 pixels|Base64 encoded image of ID front side|
|**faceImage** *|String|Max. 15MB & <8000 pixels per side & longest side >300 pixels|Base64 encoded image of face<br> __* Mandatory if Face match enabled__|
|**country**|String|3|Possible countries:<br>• [ISO 3166-1 alpha-3](http://en.wikipedia.org/wiki/ISO_3166-1_alpha-3) country code<br>• XKX (Kosovo)|
|**idType**|String|255|PASSPORT, DRIVING\_LICENSE, ID\_CARD, VISA|
|frontsideImageMimeType|String||Mime type of front side image<br>Possible values: image/jpeg (default), image/png|
|faceImageMimeType|String||Mime type of face image<br>Possible values: image/jpeg (default), image/png|
|backsideImage|String|Max. 15MB & <8000 pixels per side & longest side >300 pixels|Base64 encoded image of ID back side |
|backsideImageMimeType|String||Mime type of back side image<br>Possible values: image/jpeg (default), image/png|
|enabledFields|String|100|Defines fields which will be extracted during the ID verification. If a field is not listed in this parameter, it will not be processed for this transaction, regardless of customer portal settings.<br> **Note:** Face match and Address extraction will not be processed unless enabled for your account. If you want to enable them, please contact your Customer Success Manager, or reach out to Jumio Support.<br>See [supported documents for address extraction](#supported-documents-for-address-extraction).<br>Possible values:<br>"idNumber,idFirstName,idLastName,idDob, idExpiry,idUsState,idPersonalNumber,idFaceMatch,idAddress"|
|merchantReportingCriteria|String|100|Your reporting criteria for each scan|
|**customerId** \*|String|100|Identification of the customer must not contain sensitive data like PII (Personally Identificable Information) or account login <br>__* Mandatory if using Jumio Screening__|
|callbackUrl|String|255|Callback URL for the confirmation after the verification is completed (for constraints see [Callback URL](/netverify/portal-settings.md#callback-url))|
|firstName|String|100|First name of the customer|
|lastName|String|100|Last name of the customer|
|usState|String|100|Possible values if idType = PASSPORT or ID\_CARD:<br>• Last two characters of [ISO 3166-2: US](http://en.wikipedia.org/wiki/ISO_3166-2:US) state code<br>• [ISO 3166-1](http://en.wikipedia.org/wiki/ISO_3166-1) country name<br>• Kosovo<br>• [ISO 3166-1 alpha-2](http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country code<br>• XK (Kosovo)<br>• [ISO 3166-1 alpha-3](http://en.wikipedia.org/wiki/ISO_3166-1_alpha-3) country code<br>• XKX (Kosovo)<br><br>If idType = DRIVING\_LICENSE:<br>• Last two characters of [ISO 3166-2: US state code](http://en.wikipedia.org/wiki/ISO_3166-2:US)|
|expiry|String||Date of expiry in the format YYYY-MM-DD|
|number|String|100|Identification number of the document|
|dob|String||Date of birth in the format YYYY-MM-DD|
|callbackGranularity|String|255|Possible values:<br>• onFinish (default): Callback is only sent after the whole verification<br>• onAllSteps: Additional callback is sent when the images are received|
|personalNumber|String|14|Personal number of the document|
|**userConsent** *|Object| |_User consent needed where a product uses facial recognition technology or processes biometric data_<br>Possible values:<br>See [userConsent](#request-userconsent)<br>__* Mandatory for [End-User Consent](#end-user-consent-for-biometric-data-in-api)__ even if the user is not based in USA|

<br>

### Request userConsent

|Parameter|Type|Max. length|Description|
|:---|:---|:---|:---|
|**userIp** *|String| |Current IP address of the end-user used during the verification<br>__* Mandatory for [End-User Consent](#end-user-consent-for-biometric-data-in-api)__ even if the user is not based in USA|
|**userLocation** *|Object| |Possible values:<br>• userLocation.country <br>• userLocation.state<br>__* Mandatory for [End-User Consent](#end-user-consent-for-biometric-data-in-api)__ even if the user is not based in USA|
|**userLocation.country** *|String|3|Current country as per end-user location during the verification<br>Possible values: <br>•	[ISO 3166-1 alpha-3 country code](http://en.wikipedia.org/wiki/ISO_3166-1_alpha-3)<br>__* Mandatory for [End-User Consent](#end-user-consent-for-biometric-data-in-api)__ even if the user is not based in USA|
|userLocation.state|String|100|Current State as per end-user location during the verification<br>Applicable only in countries where state exists, including USA<br>Possible values: <br>•	For [USA](https://www.iso.org/obp/ui/#iso:code:3166:US), [CAN](https://www.iso.org/obp/ui/#iso:code:3166:CA), or [AUS](https://www.iso.org/obp/ui/#iso:code:3166:AU) alpha-2 code (state only - without country & hyphen), e.g. `IL` (Illinois), `NSW` (New South Wales)<br>•	For other countries, if applicable, any string|
|**consent** *|Object||Possible values:<br>• consent.obtained <br>• consent.obtainedAt<br>__* Mandatory for [End-User Consent](#end-user-consent-for-biometric-data-in-api) and if userLocation.country = USA / consent.obtained = yes__|
|**consent.obtained** *|String||If the end-user consent has been obtained<br>Possible values: <br>•	yes (the consent was given by the end-user)<br>•	no (the consent was not given by the end-user)<br>•	na (not applicable)<br>__* Mandatory for [End-User Consent](#end-user-consent-for-biometric-data-in-api) and if country = USA__|
|**consent.obtainedAt** *|String||Timestamp when the consent was obtained (UTC)<br>Format: YYYY-MM-DDThh:mm:ss.SSSZ<br>__* Mandatory for [End-User Consent](#end-user-consent-for-biometric-data-in-api) and if consent.obtained = yes__|

### Supported documents for address extraction

|Country    |ID card    |Driving license    |Passport    |Callback format|
|:------------|:-------|:--------------|:--------------|:-------|
|Australia|No|Yes|No|RAW|
|Bahrain|No|Yes|No|RAW|
|Canada|No|Yes|No|RAW|
|France|Yes|Yes|Yes|RAW|
|Germany|Yes|No|No|RAW|
|Indonesia|Yes|No|No|RAW|
|Ireland|No|Yes|No|RAW|
|Malaysia|Yes|No|No|RAW|
|Malta|Yes|Yes|No|RAW|
|Mexico|Yes|No|No|RAW|
|Peru|Yes|Yes|No|RAW|
|Romania|Yes|No|No|RAW|
|Singapore|Yes|No|No|RAW|
|Spain|Yes|No|No|RAW|
|United Kingdom|No|Yes|No|RAW|
|United States|Yes|Yes|No|RAW|

### End-User Consent for Biometric Data in API

To collect consent on Jumio’s behalf, you will need to incorporate certain explicit consent collection language and a link to Jumio’s Privacy Notice within your user consent flow before collection of end-user biometric data, if the end-user is located inside the United States.

The following is an example of how consent may be presented to the end-user, but you may use your own custom language as long as the required elements are present:

> “By clicking “Start” you consent to Jumio collecting, processing, and sharing your personal information, which may include biometric data, pursuant to its [Privacy Notice](https://www.jumio.com/legal-information/privacy-policy/jumio-corp-privacy-policy-for-online-services/).”

#### When to share consent parameters?

Every transaction which includes any product/workflow step in which biometrics are collected needs to have consent parameters adapted and populated. This means if Face match is enabled for your account you will need to share consent parameters for performNetverify.

If consent is not provided or other scenarios where consent needs to be resubmitted use our [Account Update](#account-update) API to add the needed consent parameters.

If your Privacy Policy and Terms & Conditions allowing continuous use of consent obtained from end-user on biometric data collection and processing the timestamp (`consent.obtainedAt`) should be when the original/updated consent was obtained.

|⚠️ If the continuous consent applies, the consent is valid for a period of maximum 3 years (between the consent timestamp and current timestamp). A new consent needs to be obtained from the end-user before continuous consent expires. The consent timestamp (`consent.obtainedAt`) has to be updated accordingly.
|:----------|

Note: Until further notice Jumio will not perform any treatment based on the consent parameters. After a notice period business validation will be performed, which could also lead to a rejection of the transaction also providing additional consent specific error codes. We will inform you separately on a specific date.

#### Where to share consent parameters?

Within the consent area in the [API request body](#request-body).

- Always mandatory: Populate the end-user IP address and end-user current location (`userConsent.userIp`,`userLocation.country`)
- Mandatory if userLocation.country = USA: Populate the consent parameters (i.e., status, timestamp)

See [examples](#examples) for a sample request including the user consent parameters.

---
## Response

|Parameter|Type|Max Length|Description|
|:----|:----|:----|:----|
|timestamp|String||Timestamp (UTC) of the response <br>format: YYYY-MM-DDThh:mm:ss.SSSZ|
|jumioIdScanReference|String|36|Jumio's reference number for each scan|

---
## Examples
### Sample request
#### Including End-User Consent (USA)

```
POST https://netverify.com/api/netverify/v2/performNetverify HTTP/1.1
Accept: application/json
Content-Type: application/json
Content-Length: 1234
User-Agent: Example Corp SampleApp/1.0.1
Authorization: Basic xxxxxxxxxxxxxxxxxxxxxxxxxxxxx

{
  "merchantIdScanReference": "YOURSCANREFERENCE",
  "frontsideImage": "YOURBASE64STRING",
  "country": "XKX",
  "idType": "PASSPORT",
  "customerId":"customerId",
  "userConsent": {
      "userIp": "1.2.3.4",                    
      "userLocation": {
          "country": "USA",                       
          "state": "IL"                           
      },
       "consent": {
        "obtained": "yes",                        
        "obtainedAt": "2022-07-20T17:20:35.000Z"  

      }
  }
}
```
#### Including End-User Consent (AUT)
```
POST https://netverify.com/api/netverify/v2/performNetverify HTTP/1.1
Accept: application/json
Content-Type: application/json
Content-Length: 1234
User-Agent: Example Corp SampleApp/1.0.1
Authorization: Basic xxxxxxxxxxxxxxxxxxxxxxxxxxxxx

{
  "merchantIdScanReference": "YOURSCANREFERENCE",
  "frontsideImage": "YOURBASE64STRING",
  "country": "XKX",
  "idType": "PASSPORT",
  "customerId":"customerId",
  "userConsent": {
      "userIp": "192.168.0.1",                    
      "userLocation": {
          "country": "AUT"                                                
      }
  }
}
```
|⚠️ Sample requests cannot be run as-is. Replace example data with your own parameter values.
|:----------|
<br>

### Sample Response
```
{
  "timestamp": "2017-08-16T10:37:44.623Z",
  "jumioIdScanReference": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```

---
# Configuring settings in the Customer Portal

In the **Settings** screen of the Customer Portal you can customize your settings and brand your ID Verification page. <br>Save changes using your Customer Portal password to activate them.
<br>

|ℹ️ Values set in your API request will override the corresponding settings configured in the Customer Portal.
|:----------|

<br>

## Application settings — General

### Callback URL

Define a **Callback URL** to receive verification results and extracted user data from Jumio when a transaction is completed. For more information, see our [Callback](/netverify/callback.md) documentation.

#### URL requirements:

* HTTPS using the [TLS Protocol](https://tools.ietf.org/html/rfc5246) (most recent version recommended)
* Valid [URL](https://tools.ietf.org/html/rfc3986) using [ASCII characters](https://en.wikipedia.org/wiki/ASCII) or [IDNA Punycode](https://tools.ietf.org/html/rfc3492)

#### URL restrictions:

* IP addresses, ports, query parameters and fragment identifiers are not allowed.
* Personally identifiable information (PII) is not allowed in any form.


---
&copy; Jumio Corporation, 395 Page Mill Road, Suite 150 Palo Alto, CA 94306
