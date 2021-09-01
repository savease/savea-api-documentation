# Savea public API documentation

NOTE: This document is under development and might change without any notice.

---

## Table of contents

* [Get started](#get-started)
    * [The client key](#the-client-key)
    * [The account login token](#the-account-login-token)
    * [Language support](#language-support)
* [Error handling](#error-handling)
    * [Request body parameters errors](#request-body-parameters-errors)
* [Requests](#requests)
    * [Retrieve a company](#-retrieve-a-company)
    * [Retrieve a customer account login](#-retrieve-a-customer-account-login)
    * [Retrieve a stop](#-retrieve-a-stop)
    * [Retrieve all stops](#-retrieve-all-stops)
    * [Retrieve a traveller type](#-retrieve-a-traveller-type)
    * [Retrieve all traveller types](#-retrieve-all-traveller-types)
    * [Search for travel plans](#-search-for-travel-plans)
    * [Search for multiple sets of travel plans](#-search-for-multiple-sets-of-travel-plans)
    * [Check status](#-check-status)
    * [Create an account draft](#-create-an-account-draft)
    * [Log in to a customer account](#-log-in-to-a-customer-account)
    * [Log out from a customer account](#-log-out-from-a-customer-account)
* [Data structures](#data-structures)
    * [Account](#-account)
    * [Account draft](#-account-draft)
    * [Account login](#-account-login)
    * [Company](#-company)
    * [Coordinate](#-coordinate)
    * [Image](#-image)
    * [Language](#-language)
    * [Phone number](#-phone-number)
    * [Ping response](#-ping-response)
    * [Price](#-price)
    * [Shop design](#-shop-design)
    * [Stop](#-stop)
    * [Travel plan](#-travel-plan)
    * [Travel plan part](#-travel-plan-part)
    * [Travel plan set](#-travel-plan-set)
    * [Traveller type](#-traveller-type)

---

## Get started

The easiest way to get started is to use the [Ping](#-check-status) request. This request will show some useful information that can be used to debug the API client.

[Try it now](https://api.savea.se/v1/ping/).

The response will show the following information:

* ```message``` A friendly greeting message in the [requested language](#language-support).
* ```requestContent``` The raw data sent to the request.
* ```clientKey``` If a [client key](#the-client-key) was used in the request, its description is shown here.
* ```accountLogin``` If an [account login token](#the-account-login-token) was used in the request, the customer account login information is shown here.

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

---

## Error handling

If any of the documented requests fails, an HTTP Status code of 4XX is returned along with an error structure, containing these two fields:

* ```error``` A human-readable error message intended for debugging the client application. This content might change without any notice. This message should not be displayed to the end user.
* ```errorId``` A text id identifying the error type. This id will never change for a major version of the API.

[Try it now](https://api.savea.se/v1/companies/foobar).

### Request body parameters errors

In the special case when ```errorId``` is ```ERROR_INVALID_REQUEST_BODY_PARAMETERS``` (with an HTTP status code of 400), the validation failed for the parameters sent via the request body.

When this happens, an extra field is appended to the result:

* ```parameterErrors``` An array of error structures, one for each parameter that failed validation.

Each error structure contains the following fields:

* ```parameterName``` The name of the parameter sent in the request.
* ```errorMessage``` A human-readable error message in the requested language. This text is suitable for being displayed to the end user.
* ```errorDetails``` A technical detailed description of the error. This text should only be used for debugging purposes.
* ```errorId``` A text id identifying the error type. This id can be used to create a custom error message and will be one of the following:

|Value|Description|Example|
|-----|-----------|-------|
|ERROR_MISSING_PARAMETER|The required parameter is missing in the request.||
|ERROR_EMPTY_PARAMETER_VALUE|The required parameter is present in the request, but its value is empty.||
|ERROR_INVALID_PARAMETER_VALUE|The parameter value is of invalid type or malformed|The parameter is an integer when a string was expected or an email address or phone number is not valid.
|ERROR_REQUIREMENTS_FAILED_FOR_PARAMETER_VALUE|The parameter value is syntactically correct, but further requirements failed.|A passwords is set that contains to few characters.|
|ERROR_RESOURCE_EXISTS_FOR_PARAMETER_VALUE|The resource specified for the parameter value could not be created, because it already exists.|An account is created with an email address that already is set for an existing account.| 
|ERROR_INVALID_CREDENTIALS_IN_PARAMETER_VALUE|The credentials specified in the parameter value is not valid.|A password is not valid for a login to an account.|

Note: Additional entries might be added to this list without any notice.

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

**Optional extra content**

Optional extra content can be embedded in the result by adding a query parameter with the name ```embed``` set to a comma-separated list of one or more of the following values:

|Value|Description|
|-----|-----------|
|shopDesign|Colors, logos etc. used in web shop and app.
|stops|All stops for the company. This is the same result as returned from the [Retrieve all stops](#-retrieve-all-stops) request.
|travellerTypes|All traveller types for the company. This is the same result as returned from the [Retrieve all traveller types](#-retrieve-all-traveller-types) request.

**Response**

* [Company](#-company)

**Errors**

|Code|Error id|Reason|
|----|--------|-----|
|404|ERROR_INVALID_COMPANY_ID|The company id is invalid.|
|404|ERROR_EMBEDDED_CONTENT_NOT_FOUND_OR_INACCESSIBLE|The requested embedded content flag is invalid or the embedded content is not accessible.|

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

### ⇄ Search for travel plans

Returns a list of travel plans between two stops for a specific date.

⚠ This request should only be used to search for only one set of travel plans, i.e. a one-way journey. To search for travel plans that includes a return journey or to search for multiple journeys, use the [Search for multiple sets of travel plans](#-search-for-multiple-sets-of-travel-plans) request instead.

**Request**

|Method|Url|
|------|---|
|GET|/v1/travelplans/\<companyId>/?\<query>

**Query Parameters**

|Name|Description|Required|
|----|-----------|--------|
|departureDate|The date of the departure in ISO 8601 YYYY-MM-DD format.|yes|
|departureStopId|The id of the departure stop.|yes|
|arrivalStopId|The id of the arrival stop.|yes|
|travellerCount|The number of travellers for each traveller type. This string must be in the format ```travellerTypeId1:count1;travellerTypeId2:count2;...```, e.g. ```foo:1;bar:2``` means one traveller of a type with id "foo" and two travellers of a type with id "bar".|yes|

**Response**

* array of [Travel plan](#-travel-plan)

**Errors**

|Code|Error id|Reason|
|----|--------|------|
|404|ERROR_INVALID_COMPANY_ID|The company id is invalid.|
|404|ERROR_MISSING_PARAMETER|One or more required query parameters are missing.|
|404|ERROR_EMPTY_PARAMETER_VALUE|The values of one or more required query parameters are empty.|
|404|ERROR_INVALID_PARAMETER_VALUE|The values of one or more required query parameters are invalid.|
|404|ERROR_RESOURCE_NOT_FOUND_OR_INACCESSIBLE|One or more of the stops or traveller types referred to by the query parameters was not found.|

### ⇄ Search for multiple sets of travel plans

Returns a list of travel plans sets for multiple stops and dates.

**Request**

|Method|Url|
|------|---|
|GET|/v1/travelplansets/\<companyId>/?\<query>

**Query Parameters**

|Name|Description|Required|
|----|-----------|--------|
|setCount|The number of travel plan sets to search for, e.g. "1" for a one-way journey or "2" for an outbound and return journey.|yes| 
|departureDate1 ... departureDateN|The dates of the departure in ISO 8601 YYYY-MM-DD format for each set where N is equal to the value of the setCount parameter.|yes|
|departureStopId1 ... departureStopIdN|The ids of the departure stops for each set where N is equal to the value of the setCount parameter.|yes|
|arrivalStopId1 ... arrivalStopIdN|The ids of the arrival stops for each set where N is equal to the value of the setCount parameter.|yes|
|travellerCount|The number of travellers for each traveller type. This string must be in the format ```travellerTypeId1:count1;travellerTypeId2:count2;...```, e.g. ```foo:1;bar:2``` means one traveller of a type with id "foo" and two travellers of a type with id "bar".|yes|

**Response**

* array of [Travel plan set](#-travel-plan-set)

|Code|Error id|Reason|
|----|--------|------|
|404|ERROR_INVALID_COMPANY_ID|The company id is invalid.|
|404|ERROR_MISSING_PARAMETER|One or more required query parameters are missing.|
|404|ERROR_EMPTY_PARAMETER_VALUE|The values of one or more required query parameters are empty.|
|404|ERROR_INVALID_PARAMETER_VALUE|The values of one or more required query parameters are invalid.|
|404|ERROR_RESOURCE_NOT_FOUND_OR_INACCESSIBLE|One or more of the stops or traveller types referred to by the query parameters was not found.|

### ⇄ Check status

Returns various information for the request.

**Request**

|Method|Url|
|------|---|
|GET or POST|/v1/ping/|

**Response**

* [Ping response](#-ping-response)

### ⇄ Create an account draft

Create an account draft for a customer account.

🛈 This request requires a [client key](#the-client-key).

**Request**

|Method|Url|
|------|---|
|POST|/v1/accountdrafts/\<companyId>/|

**Parameters**

|Name|Description|Required|
|----|-----------|--------|
|companyId|The unique id of the company.|yes|

**Request body**

```
{
   "firstName": "...",
   "lastName": "...",
   "password": "...",
   "emailAddress": "...",
   "phoneNumber": "...",
}
```

|Name|Type|Description|Required|
|----|----|-----------|--------|
|firstName|string|The first name of the account owner.|yes|
|lastName|string|The last name of the account owner.|yes|
|password|string|The password that will be used to log in to the account.|yes|
|emailAddress|string or null|The email address for the account owner.|Either an email address or a phone number is required.|
|phoneNumber|string or null|The phone number, in any valid format, for the account owner.|Either an email address or a phone number is required.|

**Response**

* [Account draft](#-account-draft)

**Errors**

|Code|Error id|Reason|
|----|--------|------|
|400|ERROR_INVALID_REQUEST_BODY_PARAMETERS|One or more of the request body parameter are invalid.|
|403|ERROR_MISSING_OR_INVALID_CLIENT_KEY|The [client key](#the-client-key) is missing or invalid.|
|404|ERROR_INVALID_COMPANY_ID|The company id is invalid.|

**Notes**

* After a successful request, a confirmation message is sent to the specified email address if set, otherwise by a sms to the phone number. This message contains a link that the receiver must click on to create the account. The account draft will then be deleted.

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
|400|ERROR_INVALID_REQUEST_BODY_PARAMETERS|One or more of the request body parameter are invalid.|
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

### 🗏 Account draft

The account login structure represents an account draft that is used to create a customer account.

**Attributes**

|Name|Type|Description|
|----|----|-----------|
|id|string|The id of the account draft.|
|firstName|string|The first name of the account owner.|
|lastName|string|The last name of the account owner.|
|emailAddress|string or null|Optional email address for the account owner or null.
|phoneNumber|[Phone number](#-phone-number) or null|Optional phone number for the account owner or null.
|verificationSentBy|array of string|How the verification message was sent to the account owner. The array contains at least one of "email" (message was sent via e-mail to the value of the parameter "emailAddress") or "sms" (message was sent via sms to the value of the parameter "phoneNumber").|

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

**Optional attributes**

|Name|Type|Description|
|----|----|-----------|
|shopDesign|[Shop design](#-shop-design)|The shop design settings if requested as embedded content.|
|stops|array of [Stop](#-stop)|All stops for the company if requested as embedded content.|
|travellerTypes|array of [Traveller type](#-traveller-type)|All traveller types for the company if requested as embedded content.|

### 🗏 Coordinate

The coordinate structure represents a WGS84 coordinate with latitude and longitude.

**Attributes**

|Name|Type|Description|
|----|----|-----------|
|latitude|float|The latitude in degrees.|
|longitude|float|The longitude in degrees.|

### 🗏 Image

The image structure represents an image, either as an image file available online, or as inline data.

**Attributes**

|Name|Type|Description|
|----|----|-----------|
|url|string or null|The url where the image is located or null if this image is only available as inline data via the "data" attribute.|
|data|string or null|The image data in Base64 encoded format or null if this image is only available via the "url" attribute.|
|mimeType|string|The mime type of the image.|

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

### 🗏 Price

The price structure represents a price with an amount and currency.

**Attributes**

|Name|Type|Description|
|----|----|-----------|
|amount|float|The price amount.|
|currency|string|The currency as a 3 character ISO 4217 code, e.g. "SEK".|
|localFormat|string|The price in a display-friendly shorter format for users in a country using the specified currency, e.g. "215:-".|
|globalFormat|string|The price in a display-friendly format to use globally, e.g. "215.00 SEK".|

### 🗏 Shop design

The shop design structure represents the logos, colors etc. used in web shop and app.

**Attributes**

|Name|Type|Description|
|----|----|-----------|
|backgroundColor|string|The background color in #RRGGBB format.|
|textColor|string|The text color in #RRGGBB format.|
|logo|[Image](#-image) or null|The main logo image or null if no main logo is present.|

### 🗏 Stop

The stop structure represents a stop, where a traveller might enter or exit a vehicle.

**Attributes**

|Name|Type|Description|
|----|----|-----------|
|id|string|The id of the stop. This id is unique for all companies.|
|name|string|The name of the stop.|
|extraInfo|string|Optional extra information for the stop or an empty string.|
|position|[Coordinate](#-coordinate) or null|Optional position for the stop or null.|

### 🗏 Travel plan

The travel plan structure represents a complete travel plan from one stop to another.

**Attributes**

|Name|Type|Description|
|----|----|-----------|
|id|string|The id of the travel plan. This id is unique for all companies.|
|departureStop|[Stop](#-stop)|The departure stop.|
|departureDateTime|string|The date and time of departure in RFC 3339 format.|
|arrivalStop|[Stop](#-stop)|The arrival stop.|
|arrivalDateTime|string|The date and time of arrival in RFC 3339 format.|
|isBookable|boolean|If true, it is possible to place a booking for this travel plan. If false, this travel plan can not be booked.|
|price|[Price](#-price) or null|The total price for the travel plan. This attribute can be null in the rare case where the prices are not setup for the travel plan.|    
|parts|array of [Travel plan part](#-travel-plan-part)|The parts of the travel plan. Each part is a separate trip without transfers. The number of transfers required is equal to the size of this array minus 1.|

### 🗏 Travel plan part

The travel plan part structure represent a part of a travel plan that can be travelled without a transfer.

**Attributes**

|Name|Type|Description|
|----|----|-----------|
|id|string|The id of the travel plan part. This id is unique for all companies.|
|departureStop|[Stop](#-stop)|The departure stop.|
|departureDateTime|string|The date and time of departure in RFC 3339 format.|
|arrivalStop|[Stop](#-stop)|The arrival stop.|
|arrivalDateTime|string|The date and time of arrival in RFC 3339 format.|

### # Travel plan set

The travel plan set structure represents a set of travel plans for a specific date and departure/arrival stops.

**Attributes**

|Name|Type|Description|
|----|----|-----------|
|departureStop|[Stop](#-stop)|The departure stop.|
|departureDate|string|The date of departure in ISO 8601 YYYY-MM-DD format.|
|arrivalStop|[Stop](#-stop)|The arrival stop.|
|arrivalDate|string|The date  of arrival in ISO 8601 YYYY-MM-DD format.|
|travelPlans|Array of [Travel plan](#-travel-plan)|The travel plans.|

### 🗏 Traveller type

The traveller type structure represents the type of the traveller, e.g., Adult, Child etc.

**Attributes**

|Name|Type|Description|
|----|----|-----------|
|id|string|The id of the traveller type. This id is unique for all companies.|
|name|string|The name of the traveller type.|
|extraInfo|string|Optional extra information for the traveller type or an empty string.|
