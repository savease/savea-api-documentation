# Savea public API documentation

NOTE: This document is under development and might change without any notice.

---

## Table of contents

* [Get started](#get-started)
    * [The client key](#the-client-key)
    * [The account login token](#the-account-login-token)
    * [Language support](#language-support)
    * [Error handling](#error-handling)
* [Requests](#requests)
    * [Retrieve a company](#-retrieve-a-company)
    * [Retrieve a customer account login](#-retrieve-a-customer-account-login)
    * [Retrieve a stop](#-retrieve-a-stop)
    * [Retrieve all stops](#-retrieve-all-stops)
    * [Retrieve a traveller type](#-retrieve-a-traveller-type)
    * [Retrieve all traveller types](#-retrieve-all-traveller-types)
    * [Check status](#-check-status)
    * [Log in to a customer account](#-log-in-to-a-customer-account)
    * [Log out from a customer account](#-log-out-from-a-customer-account)
* [Data structures](#data-structures)
    * [Account](#-account)
    * [Account login](#-account-login)
    * [Company](#-company)
    * [Coordinate](#-coordinate)
    * [Language](#-language)
    * [Phone number](#-phone-number)
    * [Ping response](#-ping-response)
    * [Stop](#-stop)
    * [Traveller type](#-traveller-type)

---

## Get started

The easiest way to get started is to use the [Ping](#-check-status) request. This request will show some useful information that can be used to debug the API client.

[Try it now](https://api.savea.se/v1/ping/).

The response will show the following information:

* ```message``` A friendly greeting message in the [requested language](#language-support).
* ```requestContent``` The raw data sent to the request.
* ```clientKey``` If a [client key](#the-client-key) was used in the request, its description is shown here.
* ```accountLogin``` If a [account login token](#the-account-login-token) was used in the request, the customer account login information is shown here.

### The client key

Some requests require a client key, which is an identifier for the application using the API.

To include the client key in the request, use the ```X-Client-Key``` header, e.g.

```X-Client-Key: f52f...```

To apply for a client key, please contact us at [support@savea.se](mailto:support@savea.se).

### The account login token

To process requests that supports or requires functionality when a customer is logged in to an account, an account login token can or must be supplied.

To include the account login token in the request, use the ```X-Account-Login-Token``` header, e.g.

```X-Account-Login-Token: 3b82...```

An account login token can be retrieved with the [Log in to a customer account](#-log-in-to-a-customer-account) request.

### Language support

Use the ```Accept-Language``` HTTP header in the requests to choose the desired language for the result.

### Error handling

If any of the documented requests fails, an HTTP Status code of 4XX is returned along with an error structure, containing these two fields:

* ```error``` A human-readable error message intended for debugging the client application. This content might change without any notice.
* ```errorId``` A text id identifying the error type. This id will never change for a major version of the API.

[Try it now](https://api.savea.se/v1/companies/foobar).

---

## Requests

### ⇄ Retrieve a company

Returns information about a company using Savea.

**Request**

|Method|Url|
|------|---|
|GET|/v1/companies/\<companyId>|

**Parameters**

|Name|Description|Required|
|----|-----------|--------|
|companyId|The unique id of the company.|yes|

**Response**

* [Company](#-company)

**Errors**

|Code|Error id|Reason|
|----|--------|-----|
|404|ERROR_INVALID_COMPANY_ID|The company id is invalid.|

### ⇄ Retrieve a customer account login

Returns information about a customer account login session.

🛈 This request requires a [client key](#the-client-key)\
🛈 This request requires an [account login token](#the-account-login-token).

**Request**

|Method|Url|
|------|---|
|GET|/v1/accountlogins/\<companyId>/\<customerAccountLoginId>|

**Parameters**

|Name|Description|Required|
|----|-----------|--------|
|companyId|The unique id of the company.|yes|
|customerAccountLoginId|The unique id of the customer account login.|yes|

**Response**

* [Account login](#-account-login)

**Errors**

|Code|Error id|Reason|
|----|--------|-----|
|403|ERROR_MISSING_OR_INVALID_CLIENT_KEY|The [client key](#the-client-key) is missing or invalid.|
|403|ERROR_MISSING_OR_INVALID_ACCOUNT_LOGIN_TOKEN|The [account login token](#the-account-login-token) is missing or invalid.|
|404|ERROR_INVALID_COMPANY_ID|The company id is invalid.|
|404|ERROR_RESOURCE_NOT_FOUND_OR_INACCESSIBLE|The customer account login id is invalid or not accessible for the login token used.

### ⇄ Retrieve a stop

Returns information about a stop.

**Request**

|Method|Url|
|------|---|
|GET|/v1/stops/\<companyId>/\<stopId>|

**Parameters**

|Name|Description|Required|
|----|-----------|--------|
|companyId|The id of the company.|yes|
|stopId|The id of the stop.|yes|

**Response**

* [Stop](#-stop)

**Errors**

|Code|Error id|Reason|
|----|--------|-----|
|404|ERROR_INVALID_COMPANY_ID|The company id is invalid.|
|404|ERROR_RESOURCE_NOT_FOUND_OR_INACCESSIBLE|The stop id is invalid.|

### ⇄ Retrieve all stops

Returns the public stops for a company.

**Request**

|Method|Url|
|------|---|
|GET|/v1/stops/\<companyId>/|

**Parameters**

|Name|Description|Required|
|----|-----------|--------|
|companyId|The id of the company.|yes|

**Response**

* array of [Stop](#-stop)

**Errors**

|Code|Error id|Reason|
|----|--------|-----|
|404|ERROR_INVALID_COMPANY_ID|The company id is invalid.|

### ⇄ Retrieve a traveller type

Returns information about a traveller type.

**Request**

|Method|Url|
|------|---|
|GET|/v1/travellertypes/\<companyId>/\<travellerTypeId>|

**Parameters**

|Name|Description|Required|
|----|-----------|--------|
|companyId|The id of the company.|yes|
|travellerTypeId|The id of the traveller type.|yes|

**Response**

* [Traveller type](#-traveller-type)

**Errors**

|Code|Error id|Reason|
|----|--------|-----|
|404|ERROR_INVALID_COMPANY_ID|The company id is invalid.|
|404|ERROR_RESOURCE_NOT_FOUND_OR_INACCESSIBLE|The traveller type id is invalid.|

### ⇄ Retrieve all traveller types

Returns the public traveller types for a company.

**Request**

|Method|Url|
|------|---|
|GET|/v1/travellertypes/\<companyId>/|

**Parameters**

|Name|Description|Required|
|----|-----------|--------|
|companyId|The id of the company.|yes|

**Response**

* array of [Traveller type](#-traveller-type)

**Errors**

|Code|Error id|Reason|
|----|--------|-----|
|404|ERROR_INVALID_COMPANY_ID|The company id is invalid.|

### ⇄ Check status

Returns various information for the request.

**Request**

|Method|Url|
|------|---|
|GET or POST|/v1/ping/|

**Response**

* [Ping response](#-ping-response)

### ⇄ Log in to a customer account

Log in to a customer account.

🛈 This request requires a [client key](#the-client-key).

**Request**

|Method|Url|
|------|---|
|POST|/v1/accountlogins/\<companyId>/|

**Parameters**

|Name|Description|Required|
|----|-----------|--------|
|companyId|The unique id of the company.|yes|

**Request body**

```
{
   "emailAddressOrPhoneNumber": "...",
   "password": "...",
   "isPermanentLogin": ...
}
```

|Name|Type|Description|Required|
|----|----|-----------|--------|
|emailAddressOrPhoneNumber|string|A valid email address or phone number for the account owner. The phone number can be in any valid format.|yes|
|password|string|The password for the account.|yes|
|isPermanentLogin|boolean|If true, the login will be "permanent", i.e. with a very long expiry time. If false, the login will in most cases expire after 20 minutes of inactivity.|yes|

**Response**

* [Account login](#-account-login)

**Errors**

|Code|Error id|Reason|
|----|--------|------|
|400|ERROR_INVALID_PARAMETER|One or more of the request body parameter values are in invalid format.|
|400|ERROR_INVALID_CREDENTIALS|The login failed due to invalid login credentials.|
|403|ERROR_MISSING_OR_INVALID_CLIENT_KEY|The [client key](#the-client-key) is missing or invalid.|
|404|ERROR_INVALID_COMPANY_ID|The company id is invalid.|

### ⇄ Log out from a customer account

Log out from a customer account.

🛈 This request requires a [client key](#the-client-key).\
🛈 This request requires an [account login token](#the-account-login-token).

**Request**

|Method|Url|
|------|---|
|DELETE|/v1/accountlogins/\<companyId>/\<customerAccountLoginId>|

**Parameters**

|Name|Description|Required|
|----|-----------|--------|
|companyId|The unique id of the company.|yes|
|customerAccountLoginId|The unique id of the customer account login.|yes|

**Response**

* Empty response.

**Errors**

|Code|Error id|Reason|
|----|--------|-----|
|403|ERROR_MISSING_OR_INVALID_CLIENT_KEY|The [client key](#the-client-key) is missing or invalid.|
|403|ERROR_MISSING_OR_INVALID_ACCOUNT_LOGIN_TOKEN|The [account login token](#the-account-login-token) is missing or invalid.|
|404|ERROR_INVALID_COMPANY_ID|The company id is invalid.|
|404|ERROR_RESOURCE_NOT_FOUND_OR_INACCESSIBLE|The customer account login id is invalid or not accessible for the login token used.

**Notes**

* After a successful request, the corresponding [account login](#-account-login) and its token will be invalid.

---

## Data structures

### 🗏 Account

The account structure represents information about a customer account.

**Attributes**

|Name|Type|Description|
|----|----|-----------|
|id|string|The id of the account.|
|firstName|string|The first name of the account owner.|
|lastName|string|The last name of the account owner.|
|emailAddress|string or null|The email address of the account owner or null if no email address is registered for the account.|
|phoneNumber|[Phone number](#-phone-number) or null|The phone number of the account owner or null if no phone number is registered for the account.|
|balance|float|The balance in SEK for any pre-paid purchases for this account.

### 🗏 Account login

The account login structure represents information about a customer account login session.

**Attributes**

|Name|Type|Description|
|----|----|-----------|
|id|string|The id of the account login.|
|token|string|The [account login token](#the-account-login-token).|
|account|[Account](#-account)|The customer account.|

### 🗏 Company

The company structure represents information about a company using Savea.

**Attributes**

|Name|Type|Description|
|----|----|-----------|
|id|string|The id of the company.|
|name|string|The name of the company.|
|languages|array of [Language](#-language)|The languages supported by the company.|

### 🗏 Coordinate

The coordinate structure represents a WGS84 coordinate with latitude and longitude.

**Attributes**

|Name|Type|Description|
|----|----|-----------|
|latitude|float|The latitude in degrees.|
|longitude|float|The longitude in degrees.|

### 🗏 Language

The language structure represents a language.

**Attributes**

|Name|Type|Description|
|----|----|-----------|
|code|string|The ISO 639-1 two-letter code for the language.|
|name|string|The name of the language in the own language.|

### 🗏 Phone number

The phone number structure represents an international phone number.

**Attributes**

|Name|Type|Description|
|----|----|-----------|
|msisdn|string|The phone number as a [MSISDN](https://en.wikipedia.org/wiki/MSISDN).|
|internationalFormat|string|The phone number in a display-friendly international format, e.g. "+46 480 XXX XX"|
|nationalFormat|string|The phone number in a display-friendly national format, e.g. "0480-XXX XX"|

### 🗏 Ping response

The response from a [Ping](#-check-status) request.

**Attributes**

|Name|Type|Description|
|----|----|-----------|
|message|string|A friendly greeting message in the requested language.|
|requestContent|object or null|The content sent in the request echoed back or null if no content was sent.|
|clientKey|string or null|The description of the [client key](#the-client-key) in the request or null if no valid client key was used.| 
|accountLogin|[AccountLogin](#-account-login) or null|The customer account login if a valid [account login token](#the-account-login-token) was used or null if no account token was used.| 

### 🗏 Stop

The stop structure represents a stop, where a traveller might enter or exit a vehicle.

**Attributes**

|Name|Type|Description|
|----|----|-----------|
|id|string|The id of the stop. This id is unique for all companies.|
|name|string|The name of the stop.|
|extraInfo|string|Optional extra information for the stop or an empty string.|
|position|[Coordinate](#-coordinate) or null|Optional position for the stop or null.|

### 🗏 Traveller type

The traveller type structure represents the type of the traveller, e.g., Adult, Child etc.

**Attributes**

|Name|Type|Description|
|----|----|-----------|
|id|string|The id of the traveller type. This id is unique for all companies.|
|name|string|The name of the traveller type.|
|extraInfo|string|Optional extra information for the traveller type or an empty string.|
