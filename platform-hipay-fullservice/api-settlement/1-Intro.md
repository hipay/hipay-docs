![](images/media/image1.jpg){width="8.286897419072616in"
height="4.4375in"}![](images/media/image2.png){width="3.5388593613298336in"
height="0.4847222222222222in"}![](images/media/image3.png){width="8.334722222222222in"
height="1.9951388888888888in"}![](images/media/image2.png){width="3.5388593613298336in"
height="0.4847222222222222in"}

About this guide 3

How to read this guide 4

Chapter 1. API overview 7

1.1. Security considerations 7

Chapter 2. Integration guidelines 9

2.1. Submitting a request 9

2.2. Response handling 10

2.3 Error handling 12

Chapter 3. REST API resources 13

3.1. Request a list of settlements 13

3.2. Request general information about a settlement 15

3.3 Request for a settlement file 16

**List of tables**

[Table 1: Typographic conventions used throughout this guide
4](#_Toc453688143)

[Table 2: Description of data elements 5](#_Toc453688144)

[Table 3: Available formats of data elements 5](#_Toc453688145)

[Table 4: Acronyms and abbreviations 6](#_Toc453688146)

Table 5: Service endpoint 9

Table 6: HTTP status codes 10

Table 7: Properties of an error message 12

Table 8: Optional GET parameters 14

<span id="_Toc453598093" class="anchor"><span id="_Toc453688101"
class="anchor"></span></span>

About this guide

Purpose

This document is designed to give you details on how to integrate your
business to the HiPay Fullservice Financial Gateway. It provides
step-by-step instructions on how to simply and quickly get up and
running with our services, as well as detailed reference material.

Where applicable, this document refers to the related documentation with
further details.

Intended audience

The intended audience is the merchant’s technical staff or the
merchant’s system integrator.

Because almost every communication between the merchant’s system and the
REST API is done through predefined XML or JSON messages over the
Internet using standard protocols, basic XML/JSON programming skills and
a knowledge of HTTP(S) are required.

Furthermore, it is recommended to be familiar with the basics of
tokenization concepts.

Copyright

The information contained in this guide is proprietary and confidential
to HiPay and its members. This material may not be duplicated,
published, or disclosed, in whole or in part, without the prior written
permission of HiPay.

Legal notice

This document contains the proprietary and confidential information of
HiPay. Such information may not be used for any unauthorized purpose and
may not be published or disclosed to third parties, in whole or in part,
without the express written permission of HiPay. You acknowledge and
agree that between you and HiPay, this document and all portions
thereof, including, but not limited to, any copyright, trade secret and
other intellectual property rights relating thereto, are and at all
times shall remain the sole property of HiPay and that title and full
ownership rights in the information contained herein and all portions
thereof are reserved to and at all times shall remain with HiPay. You
agree to safeguard the confidentiality of the information contained
herein using the same standard you employ to safeguard your own
confidential information of like kind, but in no event less than a
commercially reasonable standard of care. If you do not agree with the
foregoing conditions, you are required to return this document
immediately to HiPay.

<span id="_Toc241310459" class="anchor"><span id="_Toc453598094"
class="anchor"></span></span>

<span id="_Toc453688102" class="anchor"></span>How to read this guide

Document conventions

Conventions will help you to locate and interpret information.

Please refer to the following table for details about the typographic
conventions used throughout this guide to highlight certain words or
phrases and point out specific pieces of information.

  Convention            Meaning
  --------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Mono-space            Source code, code examples, input to the command line, application output, code lines embedded in text, variables and code elements
  Italics               A term being or to be defined afterwards
  ...                   Horizontal ellipsis points in sample code indicate the omission of part of the code (i.e. when you would normally expect additional code to appear, but such code would not be related to the example)
  \$                    At the beginning of a command, indicates an operating system shell prompt
  &lt;placeholder&gt;   Indicates placeholders. Most often method or function parameters; these placeholders represent information that must be supplied by the implementation or the user. For command-line input, indicates parameter values.

<span id="_Toc453596277" class="anchor"><span id="_Toc453688143"
class="anchor"></span></span>Table 1: Typographic conventions used
throughout this guide

Abbreviations used in tables

The tables of this document describe data elements, which are equivalent
to parameters in a query or to fields of a response message. The
following table helps understanding the different attributes (columns)
that define a data element.

  ---------------------------------------------------------------------------------------------------------------------------
  Convention    Meaning
  ------------- -------------------------------------------------------------------------------------------------------------
  Field name    Name of the data element

  Format        Format of the element

  Length        Maximum size of the data element
                
                (i.e. a size of 6 means that the data value cannot exceed six characters)
                
                *Note: there is no limitation on the length of the size when no size is specified.*

  Req.          Specifies whether an element is required or not.
                
                **M** = Mandatory -&gt; must always be provided
                
                **C** = Conditional -&gt; requirement depends on value or appearance of other elements

  Description   Brief description of the data element and, where necessary, a list of valid values and element dependencies
  ---------------------------------------------------------------------------------------------------------------------------

<span id="_Toc453596278" class="anchor"><span id="_Toc453688144"
class="anchor"><span id="_Ref221954712"
class="anchor"></span></span></span>Table 2: Description of data
elements

  ---------------------------------------------------------------------------------------------------------------------------
  Format abbreviation   Description                                                 Example
  --------------------- ----------------------------------------------------------- -----------------------------------------
  AN                    Alphanumeric characters (a-z, A-Z, 0-9)                     

  A                     Alphabetic characters only (a-z, A-Z)                       

  N                     Numeric characters only                                     

  R                     Decimal number with explicit decimal point                  12.34

  DT                    Date in the format YYYY-MM-DD                               2016-12-31

  TM                    Time in the format HH:MM with optional seconds (HH:MM:SS)   15:30

  JSON                  JSON array                                                  {"shipping\_method":"clickandcollect",\
                                                                                    "first\_order":"0"}
  ---------------------------------------------------------------------------------------------------------------------------

<span id="_Toc453596279" class="anchor"><span id="_Toc453688145"
class="anchor"></span></span>Table 3: Available formats of data elements

Acronyms and abbreviations

The following acronyms and abbreviations are used in this guide.

  Acronym   Full name
  --------- ----------------------------------------------
  BIN       Bank Identification Number
  PAN       Primary Account Number
  PCI DSS   Payment Card Industry Data Security Standard
  REST      Representational State Transfer
  TPP       Third-Party Processing

<span id="_Toc453596280" class="anchor"><span id="_Toc453688146"
class="anchor"></span></span>Table 4: Acronyms and abbreviations

<span id="_Toc453598095" class="anchor"></span>

<span id="_Toc453688103" class="anchor"></span>Chapter 1. API overview

The API is based on REST principles. It is thus very easy to write and
test applications.

How to access the API:

-   Use your browser to access URLs.

-   Use almost any HTTP client in any programming language to interact
    with the API.

    1.  <span id="_Toc453598096" class="anchor"><span id="_Toc453688104"
        class="anchor"></span></span>Security considerations

The HiPay Fullservice REST API service is protected:

-   To ensure that only authorized merchants use it,

-   To prevent payment information from being compromised.

<span id="_Toc453598097" class="anchor"></span>PCI DSS requirements

  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Description                 The HiPay Fullservice REST API allows sending payment data, which means that the system will be transmitting, and possibly storing, card data.
  --------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Storage (SAD) information   The Card Schemes (American Express, Discover Financial Services, JCB International, MasterCard Worldwide and Visa Inc.) have never permitted the storage of sensitive data (track data and/or CVV2).
                              
                              It is prohibited under “Requirement 3” of the Payment Card Industry Data Security Standard (PCI DSS).

  Warning                     Merchants who store Sensitive Authentication Data (SAD) are exposed to fines from the Card Schemes.

  Security management data    If the Secure Vault API is used, merchants demonstrate that the system can handle this data securely and that they take full responsibility for your PCI DSS compliance.

  Contact                     For further information on PCI security standards, please visit [*www.pcisecuritystandards.org*](http://www.pcisecuritystandards.org).
  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<span id="_Toc453598098" class="anchor"></span>Encrypted communication

  Description   HiPay Fullservice provides all REST API methods over TLS (Transport Layer Security).
  ------------- -----------------------------------------------------------------------------------------------------------------------------------------
  Guarantees    All data transmitted between HiPay Fullservice and the Merchant system are encrypted (256-bit encryption using a DigiCert certificate).

<span id="_Toc453598099" class="anchor"></span>IP restriction

  ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Description   When a request is sent to the API, the IP address or IP address range from where the connection was made is verified.
                
                -   If it matches with the IP address provided by the Merchant at a previous stage (in the Merchant interface: Technical integration Section), the request will be processed.
                
                -   In case of missing or incorrect information, the server will respond with an appropriate error message, indicating the error in the request.
                
                
  ------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Important     When your IP address has changed, do not forget to ensure that all new IP addresses are configured for your account. If not, your server requests will be rejected.
  ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<span id="_Toc453598100" class="anchor"></span>Authentication

Only authenticated users and system components are allowed to access the
Financial Gateway API.

<span id="_Toc453598101" class="anchor"><span id="_Toc453688105"
class="anchor"></span></span>Chapter 2. Integration guidelines

This chapter outlines the basic integration requirements that your
application must meet.

<span id="_Toc453598102" class="anchor"><span id="_Toc453688106"
class="anchor"></span></span>2.1. Submitting a request

<span id="_Toc453598103" class="anchor"></span>Service endpoint

There is one endpoint (base URL) to which you can make your API calls.

-   The **Production** financial API only works in the production
    environment with live HiPay Fullservice accounts.

All URLs referenced in this guide must have the following base:

  Environment   Endpoint
  ------------- -------------------------------
  Production    https://api.hipay-tpp.com/v1/

<span id="_Toc453596281" class="anchor"><span id="_Toc453688147"
class="anchor"></span></span>Table 5: Service endpoint

<span id="_Toc453598104" class="anchor"></span>Authentication

  --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Overview         All requests to the HiPay Fullservice REST API require merchants to authenticate themselves using the HTTP Basic authentication.
  ---------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Authentication   Your API credentials can be found in the Merchant interface, in the Integration section.
                   
                   Most HTTP clients (including web browsers) have built-in support for HTTP Basic authentication. If not, the following header must be included in all HTTP requests.

  Authorization    Basic base64("&lt;API login&gt;:&lt;API password&gt;")

  Note             If the login and/or password are wrong, the **401 Unauthorized** HTTP Status code is returned.
  --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<span id="_Toc453598105" class="anchor"></span>

Character encoding

All parameters accept UTF-8 encoded text via the API. All other
encodings must be converted to UTF-8 before sending them to the HiPay
Fullservice API in order to guarantee that the data is not corrupted.

<span id="_Toc453598106" class="anchor"><span id="_Toc453688107"
class="anchor"></span></span>2.2. Response handling

The HTTP status code indicates whether a request succeeded or not.

I.e. a 2xx status code indicates a success, whereas a 4xx or a 5xx
status code indicates a failure.

<span id="_Toc453598107" class="anchor"></span>

Response status codes

The HiPay Fullservice API attempts to return appropriate HTTP status
codes for every request.

Please see below a list of HTTP status codes you may receive and their
meaning:

  --------------------------------------------------------------------------------------------------------------
  HTTP status               Description
  ------------------------- ------------------------------------------------------------------------------------
  200 OK                    The request was understood and successfully processed.

  400 Bad Request           The request was rejected due to a validation error.
                            
                            An error message is displayed with an explanation of the error situation.

  401 Unauthorized          Authentication error with your login and/or password.
                            
                            Your credentials are invalid.

  403 Forbidden             You are making a call to a resource for which you don’t have permission.
                            
                            -   E.g.: you are trying to retrieve information about a settlement you don’t own.
                            
                            An error message is displayed with an explanation of the error situation.

  404 Not Found             The resource requested, such as a token, does not exist.

  405 Method Not Allowed    The HTTP method you used is not allowed for the requested URL.
                            
                            -   E.g.: sending a POST request when a PUT is required.
                            
                            

  500 Server Error          HiPay Fullservice server error. Report it to HiPay’s Business IT Services.

  503 Service Unavailable   The HiPay Fullservice API is temporarily unable to process the request.
                            
                            Try again later.
  --------------------------------------------------------------------------------------------------------------

<span id="_Toc453596282" class="anchor"><span id="_Toc453688148"
class="anchor"></span></span>Table 6: HTTP status codes

<span id="_Toc453598108" class="anchor"></span>Response formats

+--------------------------------------+--------------------------------------+
| Overview                             | The HiPay Fullservice API can        |
|                                      | respond to your requests in various  |
|                                      | formats.                             |
|                                      |                                      |
|                                      | To specify your preferred response   |
|                                      | format, use the HTTP Accept request  |
|                                      | header.                              |
+======================================+======================================+
| Header examples                      | Here are examples of possible        |
|                                      | headers.                             |
|                                      |                                      |
|                                      | -   Accept: application/json         |
|                                      |                                      |
|                                      | -   Accept: application/xml          |
|                                      |                                      |
|                                      | -   Accept: application/json,        |
|                                      |     application/xml;q=0.8,           |
|                                      |     \*/\*;q=0.5                      |
|                                      |                                      |
|                                      | Please refer to [*RFC 2616 HTTP      |
|                                      | Accept                               |
|                                      | Header*](http://www.w3.org/Protocols |
|                                      | /rfc2616/rfc2616-sec14.html#sec14.1) |
|                                      | for details.                         |
+--------------------------------------+--------------------------------------+
| Responses in JSON Format             | By default, the HiPay Fullservice    |
|                                      | REST API returns JSON.               |
|                                      |                                      |
|                                      | Simply add the "Accept:              |
|                                      | application/json" header to any      |
|                                      | request.                             |
|                                      |                                      |
|                                      | POST/v1/settlement HTTP/1.1          |
|                                      |                                      |
|                                      | Host: api.hipay-tpp.com              |
|                                      |                                      |
|                                      | Accept: application/json             |
|                                      |                                      |
|                                      | Connection: close                    |
|                                      |                                      |
|                                      | Here is the response to the above    |
|                                      | request, represented as JSON.        |
|                                      |                                      |
|                                      | {\                                   |
|                                      | "settlements":\[\                    |
|                                      | {"amount":21452.92,"currency":"EUR", |
|                                      | "date\_value":"2014-10-16T00:09:25+0 |
|                                      | 200","settlementid":123457,"\_links" |
|                                      | :{"self":{"href":"https:\\/\\/api.hi |
|                                      | pay-tpp.com\\/settlement\\/123457"}, |
|                                      | "raw":{"href":"https:\\/\\/api.hipay |
|                                      | -tpp.com\\/settlement\\/123457\\/raw |
|                                      | "}}},\                               |
|                                      | {"amount":27877.64,"currency":"EUR", |
|                                      | "date\_value":"2014-10-17T00:10:02+0 |
|                                      | 200","settlementid":102607,"\_links" |
|                                      | :{"self":{"href":"https:\\/\\/api.hi |
|                                      | pay-tpp.com\\/settlement\\/102607"}, |
|                                      | "raw":{"href":"https:\\/\\/api.hipay |
|                                      | -tpp.com\\/settlement\\/102607\\/raw |
|                                      | "}}},\                               |
|                                      | …\                                   |
|                                      | \]\                                  |
|                                      | }                                    |
+--------------------------------------+--------------------------------------+
| Responses in XML Format              | The API also supports returning      |
|                                      | resource representations as XML.     |
|                                      |                                      |
|                                      | E.g.: here is the XML representation |
|                                      | of a token lookup result.            |
|                                      |                                      |
|                                      | &lt;?xml version="1.0"               |
|                                      | encoding="UTF-8"?&gt;                |
|                                      |                                      |
|                                      | &lt;settlement                       |
|                                      | settlementid="123456"&gt;            |
|                                      |                                      |
|                                      | &lt;recipient\_name&gt;&lt;!--\[CDAT |
|                                      | A\[MY                                |
|                                      | BUSINESS\]\]--&gt;&lt;/recipient\_na |
|                                      | me&gt;                               |
|                                      |                                      |
|                                      | &lt;recipient\_iban&gt;&lt;!--\[CDAT |
|                                      | A\[FR713….156368\]\]--&gt;&lt;/recip |
|                                      | ient\_iban&gt;                       |
|                                      |                                      |
|                                      | &lt;recipient\_bic&gt;&lt;!--\[CDATA |
|                                      | \[ABCAFRPXXXX\]\]--&gt;&lt;/recipien |
|                                      | t\_bic&gt;                           |
|                                      |                                      |
|                                      | ...                                  |
|                                      |                                      |
|                                      | &lt;/settlement&gt;                  |
+--------------------------------------+--------------------------------------+

<span id="_Toc453598109" class="anchor"></span>\
<span id="_Toc453688108" class="anchor"></span>2.3 Error handling

  ------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Overview               The HiPay Fullservice Financial Gateway API returns two levels of error information:
                         
                         -   a HTTP status code in the header
                         
                         -   response bodies with additional details that can help you determine how to handle the exception.
                         
                         
  ---------------------- -------------------------------------------------------------------------------------------------------------------------------------------------
  Exception properties   An exception has up to two properties.
                         
                           Property   Description
                           ---------- ---------------------------------------------
                           code       Error code to find help for the exception
                           message    Descriptive message regarding the exception
                         
                         <span id="_Toc453596283" class="anchor"><span id="_Toc453688149" class="anchor"></span></span>Table 7: Properties of an error message
                         
                         E.g.: If you receive an exception with status code 400 (Bad Request), the code and message properties are useful for debugging what went wrong.

  JSON Error Example     {
                         
                         "code":"403",
                         
                         "message":"Forbidden",
                         
                         }
  ------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<span id="_Toc453598110" class="anchor"><span id="_Toc453688109"
class="anchor"></span></span>Chapter 3. REST API resources

The following table lists the REST API resources used.

  ---------------------------------------------------------------------------------------------------------------------------------
  Resource                               Description
  ---------- --------------------------- ------------------------------------------------------------------------------------------
  GET        /settlement                 To request a list of settlements.

  GET        /settlement/{id}            To request general information about a settlement {id}.

  GET        /settlement/{id}/raw{ext}   To request a list of operations for a settlement {id} formatted on a specific extension.
                                         
                                         Accepted extensions {ext}:
                                         
                                         .csv, .txt, .xls
  ---------------------------------------------------------------------------------------------------------------------------------

<span id="_Toc453598111" class="anchor"><span id="_Toc453688110"
class="anchor"></span></span>3.1. Request a list of settlements

  --------------------------------------------------------------------------------------------------------------------------
  Overview   To request a list of settlements made on your account, make a HTTP GET request to the following resource URL.
             
             GET /settlement
  ---------- ---------------------------------------------------------------------------------------------------------------
  --------------------------------------------------------------------------------------------------------------------------

### 

<span id="_Toc453598112" class="anchor"></span>Optional GET parameters

  Field name   Format[^1]   Length   Description                                  Default value
  ------------ ------------ -------- -------------------------------------------- ---------------
  amount       N                     To filter by amount                          —
  currency     AN           3        To filter by currency                        —
  date\_from   DT                    To filter by date starting from YYYY-MM-DD   —
  date\_to     DT                    To filter by date before YYYY-MM-DD          —

**\
**

  ----------------------------------------------------------------------
  sort        AN   13   Sort the list of settlements.    +settlementid
                                                         
                        Accepted values:                 
                                                         
                        -   +settlementid                
                                                         
                        -   -settlementid                
                                                         
                        -   +amount                      
                                                         
                        -   -amount                      
                                                         
                                                         
  ----------- ---- ---- -------------------------------- ---------------
  page        N         Page number of the list          1

  per\_page   N         Number of settlements per page   10
  ----------------------------------------------------------------------

<span id="_Toc453596284" class="anchor"><span id="_Toc453688150"
class="anchor"></span></span>Table 8: Optional GET parameters

<span id="_Toc453598113" class="anchor"></span>Response fields

The following table lists and describes the response fields.

+--------------------------------------+--------------------------------------+
| **Field name**                       | **Description**                      |
+======================================+======================================+
| settlementid                         | Settlement ID                        |
+--------------------------------------+--------------------------------------+
| amount                               | Settlement amount                    |
+--------------------------------------+--------------------------------------+
| currency                             | Settlement currency                  |
+--------------------------------------+--------------------------------------+
| date\_value                          | Settlement value date\               |
|                                      | E.g.: 2016-04-17T00:12:51+0200       |
+--------------------------------------+--------------------------------------+
| links                                | Settlement details links             |
|                                      |                                      |
|                                      | **self:** Request general            |
|                                      | information about a settlement       |
|                                      |                                      |
|                                      | **raw:** Request a list of           |
|                                      | operations for a settlement          |
|                                      |                                      |
|                                      | formatted on a specific extension    |
+--------------------------------------+--------------------------------------+

<span id="_Toc453598114" class="anchor"></span>

<span id="_Toc453688111" class="anchor"></span>3.2. Request general
information about a settlement

  ----------------------------------------------------------------------------------------------------------------------
  Overview   To request general information about a settlement; make a HTTP GET request to the following resource URL.
             
             GET /settlement/{id}
  ---------- -----------------------------------------------------------------------------------------------------------
  ----------------------------------------------------------------------------------------------------------------------

<span id="_Toc453598115" class="anchor"></span>Response fields

The following table lists and describes the response fields.

+--------------------------------------+--------------------------------------+
| **Field name**                       | **Description**                      |
+======================================+======================================+
| recipient\_name                      | Recipient’s name                     |
+--------------------------------------+--------------------------------------+
| recipient\_iban                      | Recipient’s IBAN                     |
+--------------------------------------+--------------------------------------+
| recipient\_bic                       | Recipient’s BIC                      |
+--------------------------------------+--------------------------------------+
| recipient\_bank\_country             | Recipient’s bank country             |
+--------------------------------------+--------------------------------------+
| amount                               | Settlement amount                    |
+--------------------------------------+--------------------------------------+
| currency                             | Settlement currency                  |
+--------------------------------------+--------------------------------------+
| date\_value                          | Settlement value date\               |
|                                      | E.g.: 2016-04-17T00:12:51+0200       |
+--------------------------------------+--------------------------------------+
| merchant\_name                       | Merchant’s name                      |
+--------------------------------------+--------------------------------------+
| merchant\_id                         | Merchant’s ID                        |
+--------------------------------------+--------------------------------------+
| settlementid                         | Settlement ID                        |
+--------------------------------------+--------------------------------------+
| sales                                | Sales amount                         |
+--------------------------------------+--------------------------------------+
| refunds                              | Refunds amount                       |
+--------------------------------------+--------------------------------------+
| fees                                 | Fees amount                          |
+--------------------------------------+--------------------------------------+
| chargeback                           | Chargebacks amount                   |
+--------------------------------------+--------------------------------------+
| deferred                             | Deferred amount                      |
+--------------------------------------+--------------------------------------+
| rolling                              | Rolling reserve amount               |
+--------------------------------------+--------------------------------------+
| other                                | Other amount                         |
+--------------------------------------+--------------------------------------+
| links                                | Settlement details links.            |
|                                      |                                      |
|                                      | **self:** Request general            |
|                                      | information about a settlement       |
|                                      |                                      |
|                                      | **raw:** Request a list of           |
|                                      | operations for a settlement          |
|                                      |                                      |
|                                      | formatted on a specific extension.   |
+--------------------------------------+--------------------------------------+

<span id="_Toc453598116" class="anchor"></span>\
<span id="_Toc453688112" class="anchor"></span>3.3 Request for a
settlement file

  -----------------------------------------------------------------------------------------------------------------------------------------------
  Overview   To request a settlement file with the detailed list of operations included; make a HTTP GET request to the following resource URL.
             
             GET /settlement/{id}/raw{ext}
             
             Accepted extensions {ext}:
             
             -   .csv (default value)
             
             -   .txt
             
             -   .xls
             
             
  ---------- ------------------------------------------------------------------------------------------------------------------------------------
  -----------------------------------------------------------------------------------------------------------------------------------------------

<span id="_Toc453598117" class="anchor"></span>

Settlement file data

To get a list of fields included in settlement files, please refer to
the “HiPay Fullservice – Financial Gateway – Settlement file fields”
documentation.

For testing and more information about the Settlement API, please refer
to:

[*https://api.hipay-tpp.com/doc*](https://api.hipay-tpp.com/doc).

[^1]: Format of the element. Please refer to "Table 3: Available formats
    of data elements” for a list of available formats.
