![](images/media/image1.jpeg){width="8.568452537182852in"
height="10.392856517935257in"}![](images/media/image2.png){width="5.014930008748906in"
height="1.0681867891513561in"}

![](images/media/image3.png){width="1.5762095363079616in"
height="1.0168547681539808in"}

**DISCLAIMER:**\
While every effort has been made to ensure the accuracy of the
information contained in this publication, the information is supplied
without representation or warranty of any kind, is subject to change
without notice and does not represent a commitment on the part of HiPay.
HiPay, therefore, assumes no responsibility and shall have no liability,
consequential or otherwise, of any kind arising from this material or
any part thereof, or any supplementary materials subsequently issued by
HiPay. HiPay has made every effort to ensure the accuracy of this
material.

**TECHNICAL SUPPORT\
**If you need any complementary information concerning the technical implementation of HiPay TPP don’t hesitate to contact our Technical Support team:\
\
Email: support.tpp@hipay.com\
Telephone: +33 (0)1 53 44 15 07 {#technical-support-if-you-need-any-complementary-information-concerning-the-technical-implementation-of-hipay-tpp-dont-hesitate-to-contact-our-technical-support-team-email-support.tpphipay.com-telephone-33-01-53-44-15-07 .En-ttedetabledesmatires}
=======================================================================================================================================================

 {#section .En-ttedetabledesmatires}

 {#section-1 .En-ttedetabledesmatires}

 {#section-2 .En-ttedetabledesmatires}

 {#section-3 .En-ttedetabledesmatires}

**Table of Contents** {#table-of-contents .En-ttedetabledesmatires}
=====================

About this Guide 4

How to Read this Guide 5

Chapter 1. API Overview 7

**1.1. Security Considerations** 7

PCI DSS Requirements 7

Encrypted Communication 7

IP Restriction 7

Authentication 7

**1.2. BIN Lookup** 8

Chapter 2. Integration Guidelines 8

**2.1. Submitting a Request** 8

Service Endpoints 8

Authentication 8

Character Encoding 8

**2.2. Response Handling** 9

Response Status Codes 9

Response Formats 9

**2.3. Error Handling** 10

Examples 11

Catching exceptions in your integration 11

Chapter 3. REST API Resources 13

**3.1. Lookup a Token** 13

Request Parameters 13

Response Fields 13

Examples 14

**3.2. Generate a Token** 15

Request Parameters 15

Response Fields 16

Examples 16

**3.3. Update a Token** 18

Request Parameters 18

Response Fields 18

Examples 19

Appendix A. Error Codes 20

**About this Guide** {#about-this-guide .ListParagraph}
====================

**Purpose**

This document is designed to provide details on how to integrate your
business to the HiPay TPP tokenization solution. The tokenization
service is intended for merchants that store, process, or transmit
cardholder data and are seeking guidance on how to reduce the scope of
their compliance efforts with the Payment Card Industry Data Security
Standards (PCI DSS).

Where applicable, this document refers to the related documentation with
further details.

**Intended Audience**

The intended audience is the merchant's technical staff or the
merchant's system integrator.

Because almost all communication between the merchant's system and the
REST API is realized through predefined XML or JSON messages over the
Internet using standard protocols, you will need basic XML/JSON
programming skills and knowledge of HTTP(S). Furthermore, it is
recommended that you are familiar with the basics of tokenization
concepts.

**Copyright**

The information contained in this guide is proprietary and confidential
to HiPay and its members. This material may not be duplicated,
published, or disclosed, in whole or in part, without the prior written
permission of HiPay.

**Legal Notice**

This document contains the proprietary and confidential information of
HiPay. Such information may not be used for any unauthorized purpose and
may not be published or disclosed to third parties, in whole or part,
without the express written permission of HiPay. You acknowledge and
agree that between you and HiPay this document and all portions thereof,
including, but not limited to, any copyright, trade secret and other
intellectual property rights relating thereto, are and at all times
shall remain the sole property of HiPay TPP and that title and full
ownership rights in the information contained herein and all portions
thereof are reserved to and at all times shall remain with HiPay. You
agree to safeguard the confidentiality of the information contained
herein using the same standard you employ to safeguard your own
confidential information of like kind, but in no event less than a
commercially reasonable standard of care. If you do not agree with the
foregoing conditions, you are required to return this document
immediately to HiPay.

**How to Read this Guide** {#how-to-read-this-guide .ListParagraph}
==========================

 {#section-4 .ListParagraph}

 {#section-5 .ListParagraph}

**Document Conventions**

To ease readability and improve understanding, this document uses a
number of conventions.

This guide uses several typographic conventions to highlight certain
words and phrases and draw attention to specific pieces of information.
The following table clarifies the conventions used across this guide.

Table 1: Typographic conventions that apply across this guide

  Convention            Purpose in this guide
  --------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Mono-space            Indicates source code, code examples, input to the command line, application output, code lines embedded in text, and variables and code elements.
  *Italic*              Italicized regular text is used for emphasis or to indicate a term that is being defined or will be defined shortly.
  ...                   Horizontal ellipsis points in sample code indicate the omission of part of the code. This is done when you would normally expect additional code to appear, but such code would not be related to the example.
  \$                    At the beginning of a command, indicates an operating system shell prompt.
  &lt;placeholder&gt;   Indicates placeholders, most often method or function parameters; these placeholders represent information that must be supplied by the implementation or the user. For command-line input, indicates parameter values.

**Abbreviations used in tables**

The tables of this document describe data elements. These data elements
are equivalent to parameters in a query or to fields of a response
message. The following table helps understanding the different
attributes (columns) that define a data element.

Table 2: Description of data elements

  ---------------------------------------------------------------------------------------------------------------------------------------
  Column        Explanation
  ------------- -------------------------------------------------------------------------------------------------------------------------
  Field Name    Contains the name of the data element.

  Format        The format of the element.

  Length        Denotes maximum size of the element.
                
                For example a size of 6 means that the data value cannot exceed six characters.
                
                When no size is specified, it means there is no limitation on the length of the field.

  Req.          Specifies whether an element is required or not.
                
                **M** = Mandatory; must always be provided
                
                **C** = Conditional; requirement depends on value or appearance of other elements.

  Description   Provides a brief description of the data element and where applicable, a list of valid values and element dependencies.
  ---------------------------------------------------------------------------------------------------------------------------------------

<span id="_Ref221954712" class="anchor"></span>Table 3: Available
formats of data elements

  Format Abbreviation   Description                                                 Example
  --------------------- ----------------------------------------------------------- ------------
  AN                    Alphanumeric characters (a-z, A-Z, 0-9)                     
  A                     Alphabetic characters only (a-z, A-Z)                       
  N                     Numerical only                                              
  R                     Decimal number with explicit decimal point, signed          12.34
  DT                    Date in the format YYYY-MM-DD                               2012-12-31
  TM                    Time in the format HH:MM with optional seconds (HH:MM:SS)   15:30

**Acronyms and Abbreviations**

Acronyms and abbreviations are used in this guide.

The following table shows the acronym or abbreviation and the full name.

Table 4: Acronyms and Abbreviations

  Acronym or Abbreviation   Full Name
  ------------------------- -----------------------------------------------
  BIN                       Bank Identification Number
  PAN                       Primary Account Number
  PCI DSS                   Payment Card Industry Data Security Standards
  REST                      REpresentational State Transfer

**API Overview**
================

The Secure Vault API allows merchants to retrieve and update data
associated with their customers' payment information stored in the HiPay
TPP secure vault. Using Secure Vault API, merchants eliminate the risk,
liability and cost of storing sensitive data on their local servers and
storage devices.

Since the API is based on REST principles, it's very easy to write and
test applications. You can use your browser to access URLs, and you can
use pretty much any HTTP client in any programming language to interact
with the API.

**Security Considerations**
---------------------------

HiPay TPP REST API service is protected to ensure that only authorized
Merchants use it and to prevent payment information from being
compromised.

###  **PCI DSS Requirements**

Using HiPay TPP REST API to send payment data means that you will be
transmitting, and possibly storing card data. The Card Schemes, Visa
Europe and MasterCard International, have never permitted the storage of
sensitive data (track data and/or CVV2), and it is prohibited under
"Requirement 3" of the Payment Card Industry Data Security Standard (PCI
DSS). Merchants who store Sensitive Authentication Data (SAD) are being
fined by the Card Schemes.

Consequently, if you use the Secure Vault API you will need to
demonstrate that your system can handle this data securely and that you
are taking full responsibility for your PCI DSS compliance.

For further information on PCI security standard, please visit
[*www.pcisecuritystandards.org*](http://www.pcisecuritystandards.org).

###  **Encrypted Communication**

HiPay TPP provides all REST API methods over SSL (Secure Sockets Layer).
This guarantees that all transmitted data between HiPay TPP and the
Merchant system is encrypted (256-bit encryption using a DigiCert
certificate).

###  **IP Restriction**

When a request is sent to the API, the IP address or IP address range
from where the connection was made is verified. If it matches with the
IP address supplied by the Merchant at a previous stage (in the Merchant
Interface: Technical Integration Section), the request will be
processed. In the case of missing or incorrect information, the server
will respond with an appropriate error message, indicating the error in
the request.

**Important: when changing your IP address, do not forget to ensure any
new IP addresses are configured for your account. Failure to do so may
result in requests being rejected.**

###  **Authentication**

Only authenticated users and system components are allowed to access to
the Secure Vault API.

**BIN Lookup**
--------------

The Secure Vault API integrates a BIN lookup tool. Every submitted card
number is automatically searched in the BIN database to determine its
issuing bank. When the BIN lookup succeeds, the response includes the
card brand, the 2-letter country code where card was issued and when
applicable the issuing bank name.

**Integration Guidelines**
==========================

**Submitting a Request**
------------------------

###  **Service Endpoints**

There are two endpoints (base URLs) that you can make your API calls to.
**Stage** if you are testing your integration, and **Production** for
when you have finished testing and want your application to go live.

All URLs referenced in this guide must have one of the following bases:

Table 5: Service endpoints

  Environment   Endpoint
  ------------- ---------------------------------------------------
  Stage         https://stage-secure-vault.hipay-tpp.com/rest/v1/
  Production    https://secure-vault.hipay-tpp.com/rest/v1/

### **Authentication**

All requests to HiPay TPP REST API require you to authenticate using the
HTTP Basic Authentication to convey your identity.

Your API credentials can be found in the Merchant Interface in the
Integration section.

Most HTTP clients (including web-browsers) have built-in support for
HTTP Basic Authentication. If not, the following header must be included
in all HTTP requests.

Authorization: Basic base64("&lt;API login&gt;:&lt;API password&gt;")

Note: if the login and/or password is wrong, the **401 Unauthorized**
HTTP Status Code is returned.

### **Character Encoding**

All parameters accept UTF-8 encoded text via the API. All other
encodings must be converted to UTF-8 before sending them to HiPay TPP
API in order to guarantee that the data is not corrupted.

**Response Handling**
---------------------

Whether a request succeeded is indicated by the HTTP status code. A 2xx
status code indicates success, whereas a 4xx or a 5xx status code
indicates failure.

###  **Response Status Codes**

The HiPay TPP API attempts to return appropriate HTTP status codes for
every request.

These are the HTTP status codes you may receive and what they mean.

Table 6: HTTP status codes

  ------------------------------------------------------------------------------------------------------------
  HTTP Status               Description
  ------------------------- ----------------------------------------------------------------------------------
  200 OK                    The request was understood and successfully processed.

  400 Bad Request           The request was rejected due to a validation error.
                            
                            An accompanying error message will explain why.

  401 Unauthorized          Authentication error with either your login and/or password.
                            
                            Your credentials are invalid.

  403 Forbidden             You are making a call to a resource you don't have permission to.
                            
                            For example, you are trying to retrieve information about a token you don't own.
                            
                            An accompanying error message will explain why.

  404 Not Found             The resource requested, such as a token, does not exist.

  405 Method Not Allowed    The HTTP method you used is not allowed for the requested URI.
                            
                            Example, sending a POST request when a PUT is required.

  500 Server Error          HiPay TPP server error. Report this to HiPay TPP technical support.

  503 Service Unavailable   HiPay TPP API is temporarily unable to process the request. Try again later.
  ------------------------------------------------------------------------------------------------------------

###  **Response Formats**

The HiPay TPP API can respond to your requests with various formats.

To specify your preferred response format, use the HTTP Accept request
header.

Here are examples of possible headers.

Accept: application/json

Accept: application/xml

Accept: application/json, application/xml;q=0.8, \*/\*;q=0.5

Refer to the [*RFC 2616 HTTP Accept
Header*](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.1)
for details.

#### Responses in XML Format

By default, HiPay TPP REST API returns XML with a root element of
&lt;response&gt;.

For example, here is the default XML representation of a token lookup
result.

1.  &lt;?xml version="1.0" encoding="UTF-8"?&gt;

<!-- -->

1.  &lt;response&gt;

2.  &lt;token&gt;b57dxd30x32z0026bd036x359cf70a80436a3b10&lt;/token&gt;

3.  &lt;brand&gt;mastercard&lt;/brand&gt;

4.  &lt;pan&gt;549554\*\*\*\*\*\*0503&lt;/pan&gt;

5.  &lt;card\_holder&gt;John Smith&lt;/card\_holder&gt;

6.  &lt;card\_expiry\_month&gt;04&lt;/card\_expiry\_month&gt;

7.  &lt;card\_expiry\_year&gt;2015&lt;/card\_expiry\_year&gt;

8.  &lt;issuer&gt;CARD SERVICES FOR CREDIT UNIONS, INC.&lt;/issuer&gt;

9.  &lt;country&gt;US&lt;/country&gt;

10. &lt;/response&gt;

#### Responses in JSON Format

The API also supports returning resource representation as JSON.

Simply add the "Accept: application/json" header to any request.

1.  GET /rest/v1/token/b57dxd30b32x0026bd036x359cf70a80436a3b10 HTTP/1.1

<!-- -->

1.  Host: secure-vault.hipay-tpp.com

2.  Accept: application/json

3.  Connection: close

Here is the response to above request, represented as JSON.

1.  {

<!-- -->

1.  "token":"dxd30b32x0026bd036x359cf70a80436a3b10",

2.  "brand":"mastercard",

3.  "pan":"549554\*\*\*\*\*\*0503",

4.  "cardHolder":"John Smith",

5.  "cardExpiryMonth":"04",

6.  "cardExpiryYear":"2015",

7.  "issuer":"CARD SERVICES FOR CREDIT UNIONS, INC.",

8.  "country":"US"

9.  }

**Error Handling**
------------------

HiPay TPP Vault API returns two levels of error information:

-   an HTTP Status Code in the header

-   a response body with additional details that can help you determine
    > how to handle the exception.

An exception has up to three properties.

Table 7: Properties of an error message

  Property      Description
  ------------- -----------------------------------------------------
  code          An error code to find help for the exception.
  message       A descriptive message regarding the exception.
  description   A more descriptive message regarding the exception.

For example, if you receive an exception with status code 400 (Bad
Request), the code and message properties are useful for debugging what
went wrong.

### Examples

#### XML Error Example

1.  &lt;?xml version="1.0" encoding="UTF-8"?&gt;

<!-- -->

1.  &lt;response&gt;

2.  &lt;code&gt;1000001&lt;/code&gt;

3.  &lt;message&gt;Incorrect Credentials&lt;/message&gt;

4.  &lt;description&gt;Username and/or password is
    > incorrect.&lt;/description&gt;

5.  &lt;/response&gt;

#### JSON Error Example

1.  {

<!-- -->

1.  "code":"1000001",

2.  "message":"Incorrect Credentials",

3.  "description":"Username and/or password is incorrect."

4.  }

### Catching exceptions in your integration

When you will implement the API, you will need to catch the exception
and extract the message.

The following sample code illustrates how to handle an error using PHP.

1.  define('API\_ENDPOINT',
    > 'https://secure-vault.hipay-tpp.com/rest/v1');

<!-- -->

1.  define('API\_USERNAME', '&lt;API login&gt;');

2.  define('API\_PASSWORD', '&lt;API password&gt;');

3.  4.  \$credentials = API\_USERNAME . ':' . API\_PASSWORD;

5.  \$resource = API\_ENDPOINT .
    > '/token/dxd30b32x0026bd036x359cf70a80436a3b10';

6.  7.  // create a new cURL resource

8.  \$curl = curl\_init();

9.  10. // set appropriate options

11. \$options = array(

12. CURLOPT\_URL =&gt; \$resource,

13. CURLOPT\_USERPWD =&gt; \$credentials,

14. CURLOPT\_HTTPHEADER =&gt; array('Accept: application/json'),

15. CURLOPT\_RETURNTRANSFER =&gt; true,

16. CURLOPT\_FAILONERROR =&gt; false,

17. CURLOPT\_HEADER =&gt; false,

18. CURLOPT\_POST =&gt; false

19. );

20. 21. foreach (\$options as \$option =&gt; \$value) {

22. curl\_setopt(\$curl, \$option, \$value);

23. }

24. 25. // execute the given cURL session

26. if (false === (\$result = curl\_exec(\$curl))) {

27. throw new RuntimeException(curl\_error(\$curl),
    > curl\_errno(\$curl));

28. }

29. 30. \$status = (int)curl\_getinfo(\$curl, CURLINFO\_HTTP\_CODE);

31. \$response = json\_decode(\$result);

32. 33. if (floor(\$status/100) != 2) {

34. throw new RuntimeException(\$response-&gt;message,
    > \$response-&gt;code);

35. }

36. 37. printf('Requested Token: %s', \$response-&gt;token);

38. 39. curl\_close(\$curl);

**\
REST API Resources**
====================

  Resource   Description
  ---------- ---------------------- ----------------------------------------------------------------------------
  GET        /token/&lt;token&gt;
  POST       /token/create
  POST       /token/update

**Lookup a Token**
------------------

Allows you to lookup the details of a credit card that you have
tokenized.

To lookup the details of a previously tokenized credit card, make an
HTTP GET request to the following resource URI.

GET /token/&lt;token&gt;

### **Request Parameters**

  Field Name    Format[^1]   Length   Req.[^2]   Description
  ------------- ------------ -------- ---------- -------------------------------------
  token         AN           40       M          The token to look for
  request\_id   N                     M          The request ID linked to the token.

### Response Fields

The following table lists and describes the response fields.

Field Name Description

**token** Token that was found.

**brand** Card brand. (e.g., Visa, MasterCard, American Express, JCB,
Discover, Diners Club, Solo, Laser, Maestro).

**pan** Card number (up to 19 characters).\
Note that, due to the PCI DSS security standards, our system has to mask
credit card numbers in any output (e.g., 549619\*\*\*\*\*\*4769).

**card\_holder** Cardholder name.

**card\_expiry\_month** Card expiry month (2 digits).

**card\_expiry\_year** Card expiry year (4 digits).

**issuer** Card issuing bank name.\
Do not rely on this value to remain static over time. Bank names may
change over time due to acquisitions and mergers.

**country** Bank country code where card was issued.\
This two-letter country code complies with ISO 3166-1 (alpha 2).

**domesticNetwork** Card domestic network (if applicable, e.g. “cb”).

### Examples

The following are examples JSON and XML responses.

#### Example Request

1.  \$ curl \\

2.  https://secure-vault.hipay-tpp.com/rest/v1/token/b57dad30b32a0026bd036b359cf70a80436a3b10
    > \\

3.  -u "&lt;your API username&gt;:&lt;your API password&gt;"

#### XML Response Example

1.  &lt;?xml version="1.0" encoding="UTF-8"?&gt;

<!-- -->

1.  &lt;response&gt;

2.  &lt;token&gt;b57dad30b32a0026bd036b359cf70a80436a3b10&lt;/token&gt;

3.  &lt;brand&gt;mastercard&lt;/brand&gt;

4.  &lt;pan&gt;549554\*\*\*\*\*\*0503&lt;/pan&gt;

5.  &lt;card\_holder&gt;John Smith&lt;/card\_holder&gt;

6.  &lt;card\_expiry\_month&gt;04&lt;/card\_expiry\_month&gt;

7.  &lt;card\_expiry\_year&gt;2015&lt;/card\_expiry\_year&gt;

8.  &lt;issuer&gt;CARD SERVICES FOR CREDIT UNIONS, INC.&lt;/issuer&gt;

9.  &lt;country&gt;US&lt;/country&gt;

10. &lt;domesticNetwork&gt;cb&lt;/domesticNetwork&gt;

11. &lt;/response&gt;

#### JSON Response Example

1.  {

<!-- -->

1.  "token":"b57dad30b32a0026bd036b359cf70a80436a3b10",

2.  "brand":"mastercard",

3.  "pan":"549554\*\*\*\*\*\*0503",

4.  "cardHolder":"John Smith",

5.  "cardExpiryMonth":"04",

6.  "cardExpiryYear":"2015",

7.  "issuer":"CARD SERVICES FOR CREDIT UNIONS, INC.",

8.  "country":"US",

9.  "domesticNetwork":"cb"

10. }

**Generate a Token**
--------------------

Allows you to perform a credit card tokenization.

To tokenize a credit card, make an HTTP POST request to the following
resource URI.

POST /token/create

### **Request Parameters**

  -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Parameter             Format[^3]   Length   Req.[^4]   Description
  --------------------- ------------ -------- ---------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  card\_number          N            19       M          The card number.
                                                         
                                                         The length is from 12 to 19 digits.

  card\_expiry\_month   N            2        M          The card expiry month.
                                                         
                                                         Expressed with two digits (e.g., 01).

  card\_expiry\_year    N            4        M          The card expiry year.
                                                         
                                                         Expressed with four digits (e.g., 2014).

  card\_holder          AN           25                  The cardholder name as it appears on the card (up to 25 chars).

  cvc                   N            4                   The 3- or 4- digit security code (called CVC2, CVV2 or CID depending on the card brand) that appears on the credit card.

  multi\_use            N            1                   Indicates if the token should be generated either for a single-use or a multi-use.
                                                         
                                                         Possible values:
                                                         
                                                         **1** = Generate a multi-use token
                                                         
                                                         **0** = Generate a single-use token
                                                         
                                                         While a single-use token is typically generated for a short time and for processing a single transaction, multi-use tokens are generally generated for recurrent payments.
  -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Response Fields

The following table lists and describes the response fields.

Field Name Description

**token** Token that was created.

**request\_id** The request ID linked to the token.

**brand** Card brand. (e.g., Visa, MasterCard, American Express, JCB,
Discover, Diners Club, Solo, Laser, Maestro).

**pan** Card number (up to 19 characters).\
Note that, due to the PCI DSS security standards, our system has to mask
credit card numbers in any output (e.g., 549619\*\*\*\*\*\*4769).

**card\_holder** Cardholder name.

**card\_expiry\_month** Card expiry month (2 digits).

**card\_expiry\_year** Card expiry year (4 digits).

**issuer** Card issuing bank name.\
Do not rely on this value to remain static over time. Bank names may
change over time due to acquisitions and mergers.

**country** Bank country code where card was issued.\
This two-letter country code complies with ISO 3166-1 (alpha 2).

**domesticNetwork** Card domestic network (if applicable, e.g. “cb”).

### Examples

The following are examples JSON and XML responses.

#### Example Request

1.  \$ curl -X POST
    > https://secure-vault.hipay-tpp.com/rest/v1/token/create \\

2.  -u "&lt;your API username&gt;:&lt;your API password&gt;" \\

3.  -H "Accept: application/json"

4.  -d "card\_number=5495540000000503" \\

5.  -d "card\_expiry\_month=04" \\

6.  -d "card\_expiry\_year=2015" \\

7.  -d "card\_holder=John Smith" \\

8.  -d "cvc=123

#### XML Response Example

1.  &lt;?xml version="1.0" encoding="UTF-8"?&gt;

<!-- -->

1.  &lt;response&gt;

2.  &lt;token&gt;b57dad30b32a0026bd036b359cf70a80436a3b10&lt;/token&gt;

3.  &lt;request\_id&gt;2U6YRQAWTGDXTAG6RZQ4RQX&lt;/request\_id&gt;

4.  &lt;brand&gt;mastercard&lt;/brand&gt;

5.  &lt;pan&gt;549554\*\*\*\*\*\*0503&lt;/pan&gt;

6.  &lt;card\_holder&gt;John Smith&lt;/card\_holder&gt;

7.  &lt;card\_expiry\_month&gt;04&lt;/card\_expiry\_month&gt;

8.  &lt;card\_expiry\_year&gt;2015&lt;/card\_expiry\_year&gt;

9.  &lt;issuer&gt;CARD SERVICES FOR CREDIT UNIONS, INC.&lt;/issuer&gt;

10. &lt;country&gt;US&lt;/country&gt;

11. &lt;domesticNetwork&gt;cb&lt;/domesticNetwork&gt;

12. &lt;/response&gt;

#### JSON Response Example

1.  {

<!-- -->

1.  "token":"b57dad30b32a0026bd036b359cf70a80436a3b10",

2.  "requestId":"2U6YRQAWTGDXTAG6RZQ4RQX",

3.  "brand":"mastercard",

4.  "pan":"549554\*\*\*\*\*\*0503",

5.  "cardHolder":"John Smith",

6.  "cardExpiryMonth":"04",

7.  "cardExpiryYear":"2015",

8.  "issuer":"CARD SERVICES FOR CREDIT UNIONS, INC.",

9.  "country":"US",

10. "domesticNetwork":"cb"

11. }

**\
****Update a Token**
--------------------

Allows you to update the details of a credit card that you have
tokenized.

To update the details of a previously tokenized credit card, make an
HTTP POST request to the following resource URI.

POST /token/update

### Request Parameters

  ------------------------------------------------------------------------------------------------------------------------
  Parameter             Format[^5]   Length   Req.[^6]   Description
  --------------------- ------------ -------- ---------- -----------------------------------------------------------------
  token                 AN           40       M          The token to be updated.

  request\_id           N                     M          The request ID linked to the card token.

  card\_expiry\_month   N            2        M          The card expiry month.
                                                         
                                                         Expressed with two digits (e.g., 01).

  card\_expiry\_year    N            4        M          The card expiry year.
                                                         
                                                         Expressed with four digits (e.g., 2014).

  card\_holder          AN           25                  The cardholder name as it appears on the card (up to 25 chars).
  ------------------------------------------------------------------------------------------------------------------------

### Response Fields

The following table lists and describes the response fields.

Field Name Description

**token** Token that was updated.

**request\_id** The request ID linked to the card token

**brand** Card brand. (e.g., Visa, MasterCard, American Express, JCB,
Discover, Diners Club, Solo, Laser, Maestro).

**pan** Card number (up to 19 characters).\
Note that, due to the PCI DSS security standards, our system has to mask
credit card numbers in any output (e.g., 549619\*\*\*\*\*\*4769).

**card\_holder** Cardholder name.

**card\_expiry\_month** Card expiry month (2 digits).

**card\_expiry\_year** Card expiry year (4 digits).

**issuer** Card issuing bank name.\
Do not rely on this value to remain static over time. Bank names may
change over time due to acquisitions and mergers.

**country** Bank country code where card was issued.\
This two-letter country code complies with ISO 3166-1 (alpha 2).

**domesticNetwork** Card domestic network (if applicable, e.g. “cb”).

### Examples

The following are examples JSON and XML responses.

#### Example Request

1.  \$ curl -X POST
    > https://secure-vault.hipay-tpp.com/rest/v1/token/update \\

<!-- -->

1.  -u "&lt;your API username&gt;:&lt;your API password&gt;" \\

2.  -d "token=b57dad30b32a0026bd036b359cf70a80436a3b10" \\

3.  -d "card\_expiry\_month=11" \\

4.  -d "card\_expiry\_year=2016"

#### XML Response Example

1.  &lt;?xml version="1.0" encoding="UTF-8"?&gt;

<!-- -->

1.  &lt;response&gt;

2.  &lt;token&gt;b57dad30b32a0026bd036b359cf70a80436a3b10&lt;/token&gt;

3.  &lt;request\_id&gt;2U6YRQAWTGDXTAG6RZQ4RQX&lt;/request\_id&gt;

4.  &lt;brand&gt;mastercard&lt;/brand&gt;

5.  &lt;pan&gt;549554\*\*\*\*\*\*0503&lt;/pan&gt;

6.  &lt;card\_holder&gt;John Smith&lt;/card\_holder&gt;

7.  &lt;card\_expiry\_month&gt;11&lt;/card\_expiry\_month&gt;

8.  &lt;card\_expiry\_year&gt;2016&lt;/card\_expiry\_year&gt;

9.  &lt;issuer&gt;CARD SERVICES FOR CREDIT UNIONS, INC.&lt;/issuer&gt;

10. &lt;country&gt;US&lt;/country&gt;

11. &lt;domesticNetwork&gt;cb&lt;/domesticNetwork&gt;

12. &lt;/response&gt;

#### JSON Response Example

1.  {

<!-- -->

1.  "token":"b57dad30b32a0026bd036b359cf70a80436a3b10",

2.  "requestId":"2U6YRQAWTGDXTAG6RZQ4RQX",

3.  "brand":"mastercard",

4.  "pan":"549554\*\*\*\*\*\*0503",

5.  "cardHolder":"John Smith",

6.  "cardExpiryMonth":"11",

7.  "cardExpiryYear":"2016",

8.  "issuer":"CARD SERVICES FOR CREDIT UNIONS, INC.",

9.  "country":"US",

10. "domesticNetwork":"cb"

11. }

####### **Error Codes**

Table 8: Configuration errors

  ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Code      Message                    Description                                         Correcting this error
  --------- -------------------------- --------------------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------
  1000001   Incorrect Credentials      Username and/or password is incorrect               This error can be caused by an incorrect API username or an incorrect API password. Make sure that all of these values are correct.
                                                                                           
                                                                                           For your security, HiPay TPP does not report exactly which of these values might be in error.

  1000003   Account Not Active         Account is inactive                                 

  1000004   Account Locked             Account is locked                                   

  1000005   Insufficient Permissions   You do not have permissions to make this API call   

  1000006   Forbidden Access           API access is disabled for this account             

  1000007   Unsupported Version        API version is not supported                        

  1000008   Temporarily Unavailable    The gateway is temporarily unavailable              Please try later
  ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Table 9. Validation errors

  Code      Message              Description
  --------- -------------------- ------------------------------------------------------------
  1010001   Missing Parameter    A required parameter is missing
  1010002   Invalid Parameter    A parameter is invalid
  1010003   Method Not Allowed   The specified HTTP Method is not allowed for this API call
  1012003   Invalid Card Token   

[^1]: The format of the element. Refer to "Table 3: Available formats of
    data elements" for the list of available formats.

[^2]: Specifies whether an element is required or not.

[^3]: The format of the element. Refer to "Table 3: Available formats of
    data elements" for the list of available formats.

[^4]: Specifies whether an element is required or not.

[^5]: The format of the element. Refer to "Table 3: Available formats of
    data elements" for the list of available formats.

[^6]: Specifies whether an element is required or not.
