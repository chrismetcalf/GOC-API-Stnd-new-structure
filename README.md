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
		* [URI argument filtering](#uri-argument-filtering)
		* [URI structure filtering](#uri-structure-filtering)
		* [Versioning](#versioning)
	* [Best Practices](#best-practices)
        * [URI Structure](#uri-structure)
		* [Human readble intuitive keys](#human-readble-intuitive-keys)
		* [Responses](#responses)
			* [Values in Keys](#values-in-keys)
			* [Internal Specific Keys](#internal-specific-keys)
			* [Metadata is dataset properties](#metadata-is-dataset-properties)

## This document

### Goal
The goal of this document is to ensure API delivery across the Government of Canada ( GoC ) is consistent and up to the highest standards by defining a base level of delivery, describing expansion where possible.  The intent is to maintain a living document that adapts to changes in the landscape of Web API delivery but at the same time being mindful of established and mandated standards presently adopted inside and outside the GoC.

```
TODO: Merge the following

This document provides a standard along with examples for Government of Canada Web APIs, encouraging consistency, maintainability, and best practices across applications. Government of Canada APIs aim to balance a truly RESTful API interface with a positive developer experience (DX).
````

### Structure
This document describes API requirements by priority:

* Minimum delivery
* Optional features
* Best practices
* Examples

### Style Guide
For the remainder of this document code, arguments and other undefined technical statements will be `code fenced` as to be easily distinguishable from standard text.

Arguments text will be used as follows:
* Arguments stated as a header will be followed by a colon ':' as `argument:`
* Arguments stated as a URI argument will be followed by an the equals sign '=' as `argument=`
* Arguments stated as a URI path element will be brackeded with forward slashes '/' as `/argument/`
* Arguments stated as a file format extension will be prefaced with a period '.' as `.arg`

Draft comments are to be bracked with the following.
`TODO:`, `NOTE:` and `ISSUE:`

## Web API Implementation
Web APIs in the GoC are to be RESTful as described by Roy Thomas Fielding's dissertation "Architectural Styles and the Design of Network-based Software Architectures" (2000) adapted to mandated minimums for the GoC and design choices in support of Web Interoperability.

The intent is not to limit development to the prescribed minimums but to ensure that GoC Web APIs behave consistently.  Any and all other requirements or options not described in this document may be implemented at the discretion of the API owner so long as the minimums and delivery standard of optional features are met.

### Minimum delivery

Interoperability depends greatly on common implementation.  Elements in this section describe mandatory elements in input, output and mantenence that must be found in and behave as described.

#### HTTP Header
Headers variables are part of the request and response cycle in the Hypertext Transfer Protocol ( HTTP ).  Although not explicitly prescribed by RESTful design header negotiation is a widely used component in defining state in format and/or language and as such need to be supported.  Supporting headers for format and language bridges, in part, a divide in the theory of proper implementation.

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

### Output

#### Metadata

Representing data about the returned dataset will be required for proper operation of certain features and describing the data provided.

To avoid collision with the data and general interoperability metadata is to be described in a variable in the response.

One field is required in the metadata varialble, the date the response was created in ISO8601 date time format with timezone as described below.

{
    metadata:{ "response_timestamp": "1867-07-01T00:00:01+00:00" },
    ...
}

http://www.tbs-sct.gc.ca/pol/doc-eng.aspx?id=18909&section=text#sec9.4
http://en.wikipedia.org/wiki/ISO_8601#Time_zone_designators

#### Minimum Formats

Leading API framworks and present day implementation of APIs delivery two output formats, JSON and XML.  Trends may be leading to JSON only APIs but development tools and applications may still support only one of the two options.  To maximise the clienta base a GoC API must output both JSON and XML at a minimum.

https://www.google.com/trends/explore?q=xml+api#q=xml%20api%2C%20json%20api&cmpt=q

#### Official Languages

All English or French content returned as data are to be nested with BCP-47 language codes used as keys.  If the content is unilingual offer the alternate language in metadata.

Bilingual
```JSON
{
    "size": {
         "fr": "Gros",
         "en": "Large"
    },
    "type": {
         "fr": "Chien",
         "en": "Dog"
    },
    ...
}
```

French Only
```JSON
{
    metadata: { "en:" : "http://example.gc.ca/..."},
    "size": {
         "fr": "Gros"
    },
    "type": {
         "fr": "Chien"
    },
    ...
}
```

English Only
```JSON
{
    metadata: { "fr:" : "http://exemple.gc.ca/..."},
    "size": {
         "en": "Large"
    },
    "type": {
         "en": "Dog"
    },
    ...
}
```

Bilingual, English and French endpoints must all offer bilingual, French or English results if requested by `Accept-Language:`, `query-parameter=` or ther means.

The calculated language ( e.g.: `Accept-Language:` or query parameters ) should be overridden if the media-type is HTML and it conflicts with the official language of the public web page served from the API.

```
Accept: text/html
Accept-Language: en
URL: http://exemple.gc.ca/api/chiens
```

The scenario above could return English content on the French `http://exemple.gc.ca/api/chiens` page.

### Documentation

`TODO: Insert when sorted`

### Registration

`TODO: Must be described`

## Optional features

Beyond the base delivery of an API features can help clients better access and use the data.  Elements in this section describe mandatory implementation of the the optional elements.

### Dataset segmenting

Limits are nearly always mandatory, only limited size datasets are safe to to implement without a default and maximum limit.  These elements are strongly recomendeded where possible and relevant.  A second, but no less important, consideration for the client creates requirements such as mobile device limitations and dataset size.

#### Limits

* If no limit is specified, return results with a default limit.
    * Sanity check to a reasonable return size
    * Sanity check to a reasonable execution time
    * For small datasets limits may exceed row count

* Limits are row / object limits
    * The organization decides what is to be in a row or object
    * Limits apply to the topmost logical row or object

Limits are to be defined as the singular `limit=` followed by an integer.

#### Offsets

Offsets apply to the structured data returned from the API distinct from internal indexing in the data.  For the purpose of explanation we'll assume rows/objects return are numbered 1, 2, 3, 4, 5 to the limit.

The general logic is to shift to what would be the subsequent entry by the offset amount.  For a query that returns 1,2,3 an offset of 1 should return 2,3,4.

Offsets are to be defined as the singular `offset=` followed by an integer.

Example use:

* http://example.gc.ca/api/dataset?limit=25&offset=75
    * For row is base 1 rows 76 through 100 should be returned

#### Pages

Much like offsets defined above `page=` is an offset incremented by the `limit=` argument.  If `limit=` is set to 25 and `page=` is set to 2 the total offset is 50, if the `page=` is set to 3 the total offset is 150.

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

Information relevant to record limits, offsets and cursors should also be included as described in the example response as a nested element.  Only relevant elements ( "count", "limit", "offset", "page" and "cursor" ) are required.

`TODO: Find a better word for ruleset than what we have below`

```JSON
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
```

```JSON
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
```

```JSON
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

```JSON
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

Where possible the use the appropriate HTTP status code such as the following:

* "304 Not Modified" when the resource has not changed since the last reload or will not change
* "404 Not found" when a requested entity does not exist

### URI argument filtering

Although not mandatory arguments are likely in dynamic and large datasets and follow standard `query=` logic.

Where required for compatibility or to satisfy client requirements query arguments can be used to override header variables such as `Accept:` or `Accept-Language:`.

URL arguments are defined by IETF RFC2396 Section 3 defined, through the document, as "query" ( http://www.ietf.org/rfc/rfc2396.txt )

### URI structure filtering

URI based filtering, or Clean URLs, can be used to specify arguments to the API in the resource path.  Generally an aesthetic feature Clean URIs aid in caching and general adoption of an API.

Caching is best served when arguments, or URIs in general, are consistently represented.  When encoding arguments in the URI be consistent in the ordering and value representation preferably in data logical order.

Formats in Clean URL are defined as extensions to the virtual file, to define an XML or JSON file would be defined as /path/file.xml and /path/file.json.

An API defined with traditional URL arguments would be converted from:

`http://example.gc.ca/api/resource?argument_one=vale_one&argument_two=value_two&data_layout=listformat=json`

To:

`http://example.gc.ca/api/resource/value_one/value_two/list.list`

Clean URLs can be achieved by various means such as scripting language or http server redirections.

### Versioning

Versioning, from a RESTful approach, is an anti-pattern but often necessary by development practices.  The only requirement in versioning if implemented, the version number must be included in the output metadata to any relevant versioning format.

```JSON
{
    "metadata": {
    	"version": "3.1.23a"
    },
    ...
}
```

If an API is to be versioned interoperability and consistency is greatly aided by the following:

* Limit endpoint changes unless necessary
* Create endpoint for needs by the need over numerical where appropriate ( e.g.: hardware dependance, client needs )
* Versions should be integers not decimal numbers to avoid galloping endpoints and prefixed with ‘v’ for intuitive identification
    * Good: `v1, v2, v3`
    * Bad: `v-1.1, v1.2, 1.3`
* If numerical major version numbers are required if a change can produce changes in logic
* If numerical maintain at least one version back

## Best Practices

Elements in this section describe prefered implementation to be consistent with existing imeplementation or .


`TODO: Describe best practices`

### URI Structure

For consistency with norms APIs should be identified seperatly and consistently.  There are two prevailing methods of distinguising an api from standard content.

The first is to contain API endpoints in a set directory at the root of the resource identifier (RI).  This lowers the chances the API endpoint moves due to later needs for the same root RI.  Using the plural "apis" is consistent with RI based.

* http://example.gc.ca/apis/dogs
* http://example.gc.ca/apis/cats

The second metod is to define a distinct domain name for the API and appending APIs afer root.  To be consistent with RI strucutre the use of 'apis' would be advised.

* http://apis.example.gc.ca/dogs
* http://apis.example.gc.ca/cats

### Human readable intuitive keys

The easier your data is to read and understand the more likely the data is to be used and correctly.

* Good: `{ "name" : "Bogart", "breed": "Bulldog" }`
* Bad: `{ "nm": "Bogart", "brd": "Bulldog" }`

### Responses

Response design is heavily dictated by data structure but there are better practices and more pitfalls to avoid.

#### Values in Keys

* Good: `{ "name" : "Bogart", "breed": "Bulldog" }`
* Bad: `{ "Bogart": "bulldog" }`

#### Internal Specific Keys

Avoid "node" and "taxonomy term" in your data.

* Good: `{ "dog_id": 12345 }`
* Bad: `{ "did": 12345 }`

#### Metadata is dataset properties

Metadata should only contain direct properties of the response set, not properties of the members of the response set

* Good: `metadata: { "count": 3, "nextDog": 1237 }`
* Bad: `metadata: { "count": 3, "dogs": "1234,1235,1236", "breeds": "bulldog,mixed,poodle" }`
