# Savea public API documentation

NOTE: This document is under development and might change without any notice.

---

## Table of contents

* [Get started](#get-started)
    * [The client key](#the-client-key)
    * [Language support](#language-support)
* [Requests](#requests)
    * [Retrieve a company](#-retrieve-a-company)
    * [Retrieve a stop](#-retrieve-a-stop)
    * [Retrieve all stops](#-retrieve-all-stops)
    * [Retrieve a traveller type](#-retrieve-a-traveller-type)
    * [Retrieve all traveller types](#-retrieve-all-traveller-types)
    * [Check status](#-check-status)
* [Data structures](#data-structures)
    * [Company](#-company)
    * [Coordinate](#-coordinate)
    * [Language](#-language)
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

### The client key

Some requests require a client key, which is an identifier for the application using the API.

To include the client key in the request, use the ```X-Client-Key``` header, e.g.

```X-Client-Key: f52f...```

To apply for a client key, please contact us at [support@savea.se](mailto:support@savea.se).

### Language support

Use the ```Accept-Language``` HTTP header in the requests to choose the desired language for the result.

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

### ⇄ Check status

Returns various information for the request.

**Request**

|Method|Url|
|------|---|
|GET or POST|/v1/ping/|

**Response**

* [Ping response](#-ping-response)

---

## Data structures

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

### 🗏 Ping response

The response from a [Ping](#-check-status) request.

**Attributes**

|Name|Type|Description|
|----|----|-----------|
|message|string|A friendly greeting message in the requested language.|
|requestContent|object or null|The content sent in the request echoed back or null if no content was sent.|
|clientKey|string or null|The description of the [client key](#the-client-key) in the request or null if no valid client key was used.| 

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
