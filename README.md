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
		* [Limits, Offsets and Cursors](#limits-offsets-and-cursors)
		* [Structured Error Handling](#structured-error-handling)
		* [URI structure filtering](#uri-structure-filtering)
		* [URI argument filtering](#uri-argument-filtering)
		* [Versioning](#versioning)
	* [Best Practices](#best-practices)
		* [Responses](#responses)
			* [Values in Keys](#values-in-keys)
			* [Internal Specific Keys](#internal-specific-keys)
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
#### Minimum Formats
#### Official Languages
#### Metadata

### Documentation

### Registration

## Optional features
### Limits, Offsets and Cursors
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

`
    {
        "status" : "400",
        "developerMessage" : "Verbose, plain language description of the problem. Provide developers
        suggestions about how to solve their problems here",
        "userMessage" : "This is a message that can be passed along to end-users, if needed.",
        "errorCode" : "444444",
        "more info" : "http://example.gc.ca/developer/path/to/help/for/444444",
    }
`

The three base states to recognize are success, improper request ( client error ) and internal server error ( API error ).  These better defined by the following HTTP Status codes:

* 200 - OK
* 400 - Bad Request
* 500 - Internal Server Error

Where possible the following codes should be used in the following circumstances:

`TODO: Add the additional errors sugested including 404, 304 and others`

### URI structure filtering
### URI argument filtering
### Versioning

## Best Practices
### Responses
#### Values in Keys
#### Internal Specific Keys

### Callbacks

# Examples
