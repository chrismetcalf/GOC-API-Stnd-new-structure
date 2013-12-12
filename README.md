GC Web API Standards
======================

<span color='red'>
Working requirements for Government of Canada (GoC) Web Application Programming Interfaces (API)s.

Presently a draft from the TBS Web Interoperability Working Group with the intent to deliver a first working draft by the next Web Managers Council.  RFC to [WET - GC Web API Standards](https://github.com/wet-boew/wet-boew-api-standards).
</span>

* [This Document](#this-document)
	* [Goal](#goal)
	* [Structure](#structure)
	* [Style Guide](#style-guide)
* [Web API Implementation](#web-api-implementation)
	* [Minimum Delivery](#minimum-delivery)
		* [HTTP Header](#http-header)
			* [Media Type](#media-type)
			* [Language](#language)
		* [HTTP Methods](#http-methods)
		* [URI Structure](#uri-structure)
		* [Output](#output)
			* [Metadata](#metadata)
			* [Minimum Formats](#minimum-formats)
			* [Official Languages](#official-languages)
		* [Documentation](#documentation)
		* [Registration](#registration)
	* [Optional Features](#optional-features)
		* [Dataset segmenting](#dataset-segmenting)
			* [Limits](#limits)
			* [Offsets](#offsets)
			* [Pages](#pages)
			* [Dataset segmenting metadata](#dataset-segmenting-metadata)
		* [Structured Error Handling](#structured-error-handling)
		* [URI structure filtering](#uri-structure-filtering)
		* [URI argument filtering](#uri-argument-filtering)
		* [Versioning](#versioning)
	* [Best Practices](#best-practices)
		* [Responses](#responses)
			* [Values in Keys](#values-in-keys)
			* [Internal Specific Keys](#internal-specific-keys)
			* [Metadata is dataset properties](#metadata-is-dataset-properties)
		* [Clean URLs](#clean-urls)
		* [Callbacks](#callbacks)
* [Examples](#examples)

## This document
### Goal
The goal of this document is to ensure API delivery across the Government of Canada ( GoC ) is consisten and up to the highest standards by defining a base level of delivery and describing expansion where possible.  The intent is to maintain a living document that adapts to changes in the landscape of Web API delivery but at the same time being mindful of established and mandated standards presently adopted inside and outside the GoC.

### Structure
This document describes API requierments by priority:

* Minimum delivery
* Optional features
* Best practices
* Examples

### Style Guide
For the remainder of this document code, arguments and other undefiend technical statements will be `code fenced` as to be easily distinguishable from standard text.

Arguments text will be used as follows:
* Arguments stated as a header will be followed by a colon ':' as `argument:`
* Arguments stated as a URI argument will be followed by an the equals sign '=' as `argument=`
* Arguments stated as a URI path element will be brackeded with forward slashes '/' as `/argument/`
* Arguments stated as a file format extension will be prefaced with a period '.' as `.arg`

Draft comments are to be bracked with the following.
`TODO:`, `NOTE:` and `ISSUE:`

## Web API Implementation
Web APIs in the GoC are to be RESTful as described by Roy Thomas Fielding's dissertation "Architectural Styles and the Design of Network-based Software Architectures" (2000) adapted to mandated minimums for the GoC and design choices in support of Web Interoperability.

The intent is not to limit development to the prescribed minimums but to ensure that GoC Web APIs behave consistently.  Any and all other requierments or options not described in this document may be implemented at the discression of the API owner so long as the minimums and delivery standard of optional features are met.

### Minimum delivery
#### HTTP Header
Headers variables are part of the request and response cycle in the Hypertext Transfer Protocol ( HTTP ).  Although not explicitly prescribed by RESTful design header negotation is a widely used component in defining state in format and/or language and as such need to be supported.  Supporting headers for format and language bridges, in part, a devide in the theory of proper implementation.

The minimum header variables to be supported are media type through `Accept:` and language through `Accept-Language:`.  Supplemental header variables in request ( e.g.: `Accept-Charset:`, `Accept-Encoding:` ) or response ( e.g.: `Content-Language:`, `Content-Length:`  ) often aid in delivery and efficiency where appropriate.

Supplemental methods of specifying output formats and language filters will be described later in this document `TODO: Reference url and extension format definition`.

##### Media Type
Output format, commonly known as media type, from a Web API are historically described my Multipurpose Internet Mail Extensions ( MIME ) types registered with the Internet Assigned Numbers Authority ( IANA )'s media type catalogue.  For most standard file types IANA's media type cataloge will provide the appropriate type definition.

The `Accept:` header is described by W3C RFC2616 Section 14.1 ( http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html )

The media types are defined by by IANA.org ( http://www.iana.org/assignments/media-types/media-types.xhtml )

##### Language
The `Accept-Language:` header is described by W3C RFC2616 Section 14.4 ( http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html )

Languages are to be defined by the W3C recomended BCP-47 ( http://www.w3.org/International/core/langtags/#bcp47 ).

#### HTTP Methods
HTTP Methods are often described in APIs as verbs.  Verbs refer to standard actions taken on data, those standard verbs are ( POST, GET, PUT and DELETE ).

Assuming dogs is a list of all dogs, dog id 1234 is an instance of dog named "Bogart" and instance 4321 is "Shreddies"

| HTTP METHOD | POST            | GET            | PUT              | DELETE           |
| ----------- | --------------- | -------------- | ---------------- | ---------------- |
| CRUD OP     | CREATE          | READ           | UPDATE           | DELETE           |
| /dogs       | Create new dogs | List dogs      | Bulk update      | Delete all dogs  |
| /dogs/1234  | Error           | Show Bogart    | update Bogart    | Delete Bogart    |
| /dogs/4321  | Error           | Show Shreddies | update Shreddies | Delete Shreddies |

HTTP Methods are described by W3C RFC2616 Sections 9.3, 9.4, 9.6 and 9.7 ( http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html )

### URI Structure

### Output
#### Metadata
#### Minimum Formats
#### Official Languages

```
TODO: Review the following segment and how it works in the "Best Practice" section.
Best practice: Implement one or both of the following methods for including multilingual data in responses.
```

```
TODO: Review the language in this block, it's close but a different author.  Revisions will fix this.
```

##### Single endpoint

Recommended for writable APIs, for APIs returning many non-language fields and for APIs supporting more than the two official languages.

All languages are returned in a nested manner with BCP-47 language codes used as keys.

```
{
    "title": {
         "fr": "Levé LiDAR aux environs du Réserve de biosphere",
         "en": "Biosphere Reserve LiDAR Survey"
    },
    "resourceCount": 5,
    "state": "active",
    ...
}
```

Fields with values chosen from a limited set, such as "state" above, are represented with a single value.

##### Multiple language endpoints

Recommended for APIs returning many fields containing language content.

Create two APIs with the language included in the API URL.

Example: the same resource with language fields returned in only a single language is available from:

* http://example.gc.ca/en/api/resource/[id]
* http://example.gc.ca/fr/api/resource/[id]

The domain and API URLs must match exactly so that callers can retrieve the corresponding results in the other language easily.  If French and English content is served from separate domains then both APIs must be available on both domains.

Language fields are returned as objects with their language as the only key:

```
{
    "title": {
         "fr": "Levé LiDAR aux environs du Réserve de biosphere",
    },
    "resourceCount": 5,
    "state": "active",
    ...
}
```

Non-language fields must not be different when the same resource is retrieved in both languages.

### Documentation

### Registration

## Optional features
### Dataset segmenting

Limits are nearly always mandatory, only limited size datasets are safe to to implement without a default and maximum limit.  These elements are strongly recomended where possible and relevant.  A second, but no less important, consideration for the client creates requierments such as moblie device limitations and dataset size.

#### Limits

* If no limit is specified, return results with a default limit.
    * Sanity check to a reasonable return size
    * Sanity check to a reaonsable execution time
    * For small datasets limits may exceed row count

* Limits are row / object limits
    * The organization decides what is to be in a row or object
    * Limits apply to the topmost logical row or object

Limits are to be defined as the singular `limit=` followed by an integer.

#### Offsets

Offsets apply to the structured data returned from the API distinct from internal indexing in the data.  For the purpose of explanation we'll assume rows/objects return are numbered 1, 2, 3, 4, 5 to the limit.

The general logic is to shift to what would be the subsequent entry by the offset ammount.  For a query that returns 1,2,3 an offset of 1 should return 2,3,4.

Offsets are to be defined as the singular `offset=` followed by an integer.

Example use:

* http://example.gc.ca/api/dataset?limit=25&offset=75
    * For row is base 1 rows 76 through 100 should be returned

#### Pages

Much like offets defined above `page=` is an offset incremtented by the `limit=` argument.  If `limit=` is set to 25 and `page=` is set to 2 the total offset is 50, if the `page=` is set to 3 the total offset is 150.

Example use:

* http://example.gc.ca/api/dataset?limit=25&page=3
    * For row is base 1 rows 76 through 100 should be returned

#### Cursor

`cursor=` is an offset with a value based on the sort order of results returned.

Use `cursor=` to reliably iterate over all results without risk of skipping or receiving duplicate rows/objects due to insertions/deletions happening at the same time.

The string value use with `cursor=` is returned in the metadata of each response when any rows/objects are returned.  Typically it is a single value copied from the last row/object, and could be a date, name, internal id or any other sortable type.

Example use:
* http://example.gc.ca/api/dataset?limit=25&cursor=20130101.010101
    * For 25 rows following the row containing the sort order value "20130101.010101"

#### Dataset segmenting metadata

Information relevant to record limits, offsets and indexes should also be included as described in the example resonse as a nested element.  Only relevant elements ( "count", "limit", "offset", "page" and "continueFrom" ) are required.

`TODO: Find a better word for ruleset than what we have below`

```
{
    "metadata": {
        "resultset": {
            "count": 25,
            "limit": 25,
            "page": 3,
            "offset": 75,
            "cursor": "sam100890032"
        }
    },
    ...
}

{
    "metadata": {
        "resultset": {
            "count": 100,
            "limit": 100,
            "page": 2,
            "offset": 200,
            "cursor": "Smith, John"
        }
    },
    ...
}


{
    "metadata": {
        "resultset": {
            "count": 18,
            "limit": 25,
            "cursor": "20130101.010101"
        }
    },
    ...
}
```

### Structured Error Handling

Although errors can be represented by HTTP status code alone structured error handling improves the ability to resolve issues for both consumer and maintainer.

Research into common practice provides the following error structure:
* Error responses are to be an HTTP status code
* The same code is to be included
* A message for the developer
* A message for the end-user ( when appropriate )
* An internal error code ( corresponding to an internal code if available )
* A link where developers can find more information ( if available )

In JSON format

```
{
    "status" : "400",
    "developerMessage" : "Verbose, plain language description of the problem. Provide developers
    suggestions about how to solve their problems here",
    "userMessage" : "This is a message that can be passed along to end-users, if needed.",
    "errorCode" : "444444",
    "more info" : "http://example.gc.ca/developer/path/to/help/for/444444",
}
```

The three base states to recognize are success, improper request ( client error ) and internal server error ( API error ).  These better defined by the following HTTP Status codes:

* 200 - OK
* 400 - Bad Request
* 500 - Internal Server Error

Where possible the following codes should be used in the following circumstances:

`TODO: Add the additional errors sugested including 404, 304 and others`

### URI structure filtering
### URI argument filtering
### Versioning

Versioning, from a RESTful approach, is an anti-pattern but often neccesary by development practices.  The only requierment in versioning if implemented, the version number must be included in the output metadata to any relevant versioning format.

```
{
    "metadata": {
    	"version": "3.1.23a"
    },
    ...
}
```

If an API is to be versioned interoperability and consistency is greatly aided by the following:

* Limit endpoint changes unless neccesary
* Create endpoint for needs by the need over numerical where appropraite ( e.g.: hardware dependance, client needs )
* Versions should be integers not decimal numbers to avoid galloping endpoints and prefixed with ‘v’ for intuitive identification
    * Good: `v1, v2, v3`
    * Bad: `v-1.1, v1.2, 1.3`
* If numerical major version numbers are required if a change can produce changes in logic
* If numerical maintain at least one version back

## Best Practices

### Human readble / intuitive keys

The easier your data is to read and understand the more likely the data is to be used and correctly.

* Good: `{ "name" : "Bogart", "breed": "Bulldog" }`
* Bad: `{ "nm": "Bogart", "brd": "Bulldog" }`

### camelCase vs snake_case

* One of the required formats, JSON, prefers camelCase.
* Standard delivery platforms and languages prefer camelCase ( PHP, JavaScript ).
* Camel case can be typed without use of a special character.

Neither is mandatory or recomended for any functional advantage.  If there is no reason to do otherwise camel case is prefered.

`TODO: This felt more rediculous the longer I typed, I prefer snake_case for readablility`

### Responses

Response design is heavily dictated by data structure but there are better practices and more pitfalls to avoid.

#### Values in Keys

* Good: `{ "name" : "Bogart", "breed": "Bulldog" }`
* Bad: `{ "Bogart": "bulldog" }`

#### Internal Specific Keys

Avoid "node" and "taxonomy term" in your data.

* Good: `{ "dogId": 12345 }`
* Bad: `{ "did": 12345 }`

#### Metadata is dataset properties

Metadata should only contain direct properties of the response set, not properties of the members of the response set

* Good: `metadata: { "count": 3, "nextDog": 1237 }`
* Bad: `metadata: { "count": 3, "dogs": "1234,1235,1236", "breeds": "bulldog,mixed,poodle" }`

### Clean URLs
### Callbacks

# Examples
