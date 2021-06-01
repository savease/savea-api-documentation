# Savea public API documentation

NOTE: This document is under development and might change without any notice.

## Data structures

### Company

The company structure represents information about a company using Savea.

#### Attributes

|Name|Type|Description|
|----|----|-----------|
|id|string|The unique id of the company.|
|name|string|The name of the company.|
|languages|[Language](#language)[]|The languages supported by the company.|

### Coordinate

The coordinate structure represents a WGS84 coordinate with latitude and longitude.

#### Attributes

|Name|Type|Description|
|----|----|-----------|
|latitude|float|The latitude in degrees.|
|longitude|float|The longitude in degrees.|

### Language

The language structure represents a language.

#### Attributes

|Name|Type|Description|
|----|----|-----------|
|code|string|The ISO 639-1 two-letter code for the language.|
|name|string|The name of the language in the own language.|

### Stop

The stop structure represents a stop, where a traveller might enter or exit a vehicle.

#### Attributes

|Name|Type|Description|
|----|----|-----------|
|id|string|The unique id of the stop. This id is unique for all companies.|
|name|string|The name of the stop.|
|extraInfo|string|Optional extra information for the stop or an empty string.|
|position|[Coordinate](#coordinate) or null|Optional position for the stop or null.|

### TravellerType

The traveller type structure represents the type of the traveller, e.g. Adult, Child etc.

#### Attributes

|Name|Type|Description|
|----|----|-----------|
|id|string|The unique id of the traveller type. This id is unique for all companies.|
|name|string|The name of the traveller type.|
|extraInfo|string|Optional extra information for the traveller type or an empty string.|