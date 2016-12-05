#HiPay Professional - Overview

This document is designed to provide you details on how to integrate your business to the HiPay Professional payment gateway. This document provides step-by-step instructions on how to simply and quickly get up and running with our services as well as detailed reference material.

#Disclaimer 
While every effort has been made to ensure the accuracy of the information contained in this publication, the information is supplied without representation or warranty of any kind, is subject to change without notice and does not represent a commitment on the part of HiPay. HiPay, therefore, assumes no responsibility and shall have no liability, consequential or otherwise, of any kind arising from this material or any part thereof, or any supplementary materials subsequently issued by HiPay. HiPay has made every effort to ensure the accuracy of this material. 

#Technical support 
If you need any complementary information concerning the technical implementation of HiPay Professional don’t hesitate to contact our Technical Support team: Email: support.tpp@hipay.com Telephone: +33 (0)1 53 44 15 07

#About this Guide 

##Purpose
This document is designed to provide you details on how to integrate your business to the HiPay TPP payment gateway. This document provides step-by-step instructions on how to simply and quickly get up and running with our services as well as detailed reference material. Where applicable, this document refers to the related documentation with further details. 

##Intended Audience 
The intended audience is the merchant's technical staff or the merchant's system integrator. Because almost all communication between the merchant's system and the SOAP API is realized through predefined XML messages over the Internet using standard protocols, you will need basic XML/SOAP programming skills and knowledge of HTTP(S). 

##Copyright 
The information contained in this guide is proprietary and confidential to HiPay and its members. This material may not be duplicated, published, or disclosed, in whole or in part, without the prior written permission of HiPay. 

##Legal Notice 
This document contains the proprietary and confidential information of HiPay. Such information may not be used for any unauthorized purpose and may not be published or disclosed to third parties, in whole or part, without the express written permission of HiPay. You acknowledge and agree that between you and HiPay this document and all portions thereof, including, but not limited to, any copyright, trade secret and other intellectual property rights relating thereto, are and at all times shall remain the sole property of HiPay and that title and full ownership rights in the information contained herein and all portions thereof are reserved to and at all times shall remain with HiPay. You agree to safeguard the confidentiality of the information contained herein using the same standard you employ to safeguard your own confidential information of like kind, but in no event less than a commercially reasonable standard of care. If you do not agree with the foregoing conditions, you are required to return this document immediately to HiPay.

#How to Read this Guide

##Document Conventions
These conventions help to locate and interpreted information.
This guide uses several typographic conventions to highlight certain words, phrases and
point out specific pieces of information.
The following table clarifies the conventions used across this guide.

*Table 1: Typographic conventions that apply across this guide*

| Convention   |      Purpose in this guide      |
|----------|:-------------:|
| Mono-space |  Indicates source code, code examples, input to the command line, application output, code lines embedded in text, and variables and code elements. |
| *Italic* |  Italicized regular text is used for emphasis or to indicate a term that is being defined or will be defined shortly. |
| ... |  Horizontal ellipsis points in sample code indicate the omission of part of the code. This is done when you would normally expect additional code to appear, but such code would not be related to the example. |
| $ |  At the beginning of a command, indicates an operating system shell prompt. |
| &lt;placeholder&gt; |  Indicates placeholders, most often method or function parameters; these placeholders represent information that must be supplied by the implementation or the user. For command-line input, indicates parameter values.|

##Abbreviations used in tables
The tables of this document describe data elements. These data elements are equivalent
to parameters in a query or to fields of a response message. The following table helps
understanding the different attributes (columns) that define a data element.

*Table 2: Description of data elements*

| Convention   |      Meaning    |
|----------|:-------------:|
| Field Name |  Name of the data element.
| Format |  Format of the element.
| Length |  Maximum size of the data element.<br/>(i.e. a size of 6 means the data value cannot exceed six characters)<br/>Note: there is no limitation on the length of the size, when no size is specified.
| Req. |  Specifies whether an element is required or not. <br/>**M** = Mandatory -> must always be provided. <br/>**C** = Conditional -> requirement depends on value or appearance of other elements.
| Description |  Brief description of the data element and where necessary, a list of valid values and element dependencies.

*Table 3: Available formats of data elements*

| Format Abbreviation   |      Description    | Example |
|----------|:-------------:|:-------------:|
| AN |  Alphanumeric characters (a-z, A-Z, 0-9) | -
| A |  Alphabetic characters only (a-z, A-Z) | -
| N |  Numerical only | -
| R |  Decimal number with explicit decimal point, signed | 12.34
| DT |  Date in the format YYYY-MM-DD | 2012-12-31
| TM |  Time in the format HH:MM with optional seconds 15:30 (HH:MM:SS) | 15:30

##Acronyms and Abbreviations
The following acronyms and abbreviations are used in this guide.

*Table 4: Acronyms and Abbreviations*

| Acronym   |      Full Name    |
|----------|:-------------:|
| API |  Application Programming Interface
| HTTPS |  HyperText Transfer Protocol Secure
| SOAP |  Simple Object Access Protocol

#API Overview

The API is based on SOAP principles; thus it is very easy to write and test applications.
How to access the API:

- Use your browser to access URLs,.
- Use almost any HTTP client in any programming language, to interact with the API.

##Security Considerations
HiPay Professional SOAP API service is protected to:

- ensure that only authorized Merchants use it,
- prevent payment information from being compromised

###Encrypted Communication

| Description   |      Guarantee of transcription    |
|----------|:-------------:|
| HiPay provides all SOAP API methods over SSL (Secure Sockets Layer). |  This guarantees that all data transmitted between HiPay and the Merchant system is encrypted (256-bit encryption using a DigiCert certificate).

#Integration Guidelines
This chapter outlines the basic integration requirements that you app must meet.

##Submitting a Request

###Service Endpoints
There are two endpoints (base URLs) that you can make your API calls to.

- Stage if you are testing your integration, and
- Production for when you have finished testing and want your application to go live.
All URLs referenced in this guide must have one of the following bases:

| Environment   |      Endpoint    |
|----------|:-------------:|
| **Stage** |  [https://test-ws.hipay.com/](https://test-ws.hipay.com/)
| **Production** |  [https://ws.hipay.com/](https://ws.hipay.com/)

###Authentication

*Table 5: Service endpoints*

| Overview   |      Authentication    |
|----------|:-------------:|
| All requests to HiPay SOAP REST API require the Merchant to authenticate himself at each request. |  Your API credentials can be found in the Merchant Interface under Sell with Hipay -> Hipay integration -> Merchant Tool Kit / API -> Webservice Access.

###Character Encoding
All parameters accept UTF-8 encoded text via the API. All other encodings must be
converted to UTF-8 before sending them to the HiPay API in order to guarantee that the
data is not corrupted.

##Response Handling
Whether a request succeeded or not, it is indicated by the HTTP status code. A 2xx status
code indicates a success, whereas a 5xx status code indicates a failure.

###Response Status Codes
The HiPay API attempts to return appropriate HTTP status codes for every request.
These are the HTTP status codes you may receive and their meaning.

*Table 6: HTTP status codes*

| HTTP Status   |      Description    |
|----------|:-------------:|
| 200 OK |  The request was understood and is processing.
| 404 Not Found |  The resource requested does not exist.
| 500 Server Error |  The request has an error, mandatory parameters are missing or you are trying to send unknown parameters to the resource.
| 503 Service Unavailable |  HiPay API is temporarily unable to process the request. Try again later.

###Response Format
The HiPay API will respond to your request in SOAP XML format.
By default, HiPay SOAP API returns XML in a SOAP Envelope.
For example, here is the default XML representation of a “generate” result.

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAPENV="http://schemas.xmlsoap.org/soap/envelope/"
xmlns:ns1="https://ws.hipay.com/soap/payment-v2">
	<SOAP-ENV:Body>
		<ns1:generateResponse>
			<generateResult>
				<redirectUrl>https://payment.hipay.com/index/pay/payid/716…7106fc</redirectUrl>
				<code>0</code>
				<description>N/A</description>
			</generateResult>
		</ns1:generateResponse>
	</SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

##Error Handling
**Overview:** HiPay API returns one level of error information: a response body with additional details that can help you determine how to handle the exception.

**Exception properties** An exception has up to three properties.

| Property   |      Description    |
|----------|:-------------:|
| code |  0 = OK / >0 = Error.
| description |  A descriptive message regarding the exception.

*Table 7: Properties of an error message*

i.g. if you receive an exception with status code 400 (Bad Request), the and   properties are useful for debugging what went wrong.

**XML Error Example**

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP- ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="https://ws.hipay.com/soap/payment-v2">
	<SOAP-ENV:Body>
		<ns1:generateResponse>
			<generateResult>
				<redirectUrl/>
				<code>1</code>
				<description>Bad password for login '4f8x3x80fd4x476f1e227c68xefdx0x5'</description>
			</generateResult>
		</ns1:generateResponse>
	</SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```


#SOAP API Resources
The following table lists the REST API resources used.

| Resource   |      Description    |
|----------|:-------------:|
| **POST** /soap/payment-v2/generate |  Allows you to request an order and initialize a hosted payment page.
| **POST** /soap/transaction-v2/confirm |  A request instructing the payment gateway to capture a previously- authorized transaction, i.e. transfer the funds from the customer's bank account to the merchant's bank account. This transaction is always preceded by an authorization.
| **POST** /soap/transaction-v2/cancel |  A request instructing the payment gateway to cancel a previously- authorized transaction. Only authorized transactions can be canceled, captured transactions must be refunded.
| **POST** /soap/refund-v2/card |  A request instructing the payment gateway to fully refund a credit card previously captured transaction.

##Request a New Order
At the time of payment, cardholders are redirected to a secured payment page hosted by HiPay.
This page can be personalized with merchants’ CSS style sheet to fit his website look and feel.
To post your CSS please go to “HiPay Professional integration -> Creating a button -> Edit (on your website details)“

**Order Parameters**

|FieldName|Format|Length|Req|Description|
|----------|:-------------:|----------|:-------------:|----------|
|wsLogin|AN|32|M|Your API Webservice Login.
|wsPassword|AN|32|M|Your API Webservice Password.
|websiteId|N|11|M|ID of the website created on merchants’ account.
|categoryId|N|11|M|The order or product categories are attached to, and depend upon, the merchant site’s category. Depending on the category that is associated with the site, the categories that are available to the order and products will NOT be the same. You can obtain the list of order and product category ID’s for the merchant site at this URL:<br/>Production platform: https://payment.hipay.com/order/list-categories/id/[production websiteID]<br/>Stage platform:<br/> https://test-payment.hipay.com/order/list-categories/id/[stage websiteID]
|currency|A|3|M|The currency specified in your HiPay Professional account. This three-character currency code complies with ISO 4217.
|amount|R|-|M|The total order amount. It should be calculated as a sum of the items purchased, plus the shipping fee (if present), plus the tax fee (if present).
|rating|AN|3|M|Age category of your order.<br/>Accepted values :<br/>+12 : For ages 13 and over<br/>+16 : For ages 16 and over<br/>+18 : For ages 18 and over<br/>ALL : For all ages
|locale|AN|5|M|Locale code of your customer (Default to en_GB – English – Great Britain).It may be used for sending confirmation emails to your customer or for displaying payment pages.<br/>Examples:<br/>en_GB<br/>fr_FR<br/>es_ES<br/>it_IT
|customerIpAddress|AN|15|M|The IP address of your customer making a purchase.
|executionDate|AN|32|M|Date and time of execution of the payment in MySQL DATETIME format (Y-m-dTH:i:s). e.g.: 2014-12-25T10:57:55
|manualCapture|N|1|M|Indicates how you want to process the payment.<br/>0: indicates transaction is sent for authorization, and if approved, is automatically submitted for capture.<br/>1: indicates this transaction is sent for authorization only. The transaction will not be sent for settlement until the transaction is submitted for capture manually by the Merchant.
|description|AN|255|M|The order short description.
|customerEmail|AN|32|-|The customer's e-mail address.
|urlCallback|AN|255|M|The URL will be used by our server to send you information in order to update your database. Please refer to “Server-to-Server notification” chapter.
|urlAccept|AN|255|-|The URL to return your customer to once the payment process is completed successfully.
|urlDecline|AN|255|-|The URL to return your customer to after the acquirer declines the payment.
|urlCancel|AN|255|-|The URL to return your customer to when he or her decides to abort the payment.
|urlLogo|AN|255|-|This URL is where the logo you want to appear on your payment page is located.<br/>Important: **HTTPS** protocol is required.
|merchantReference|AN|255|-|Merchants’ order refernce.
|merchantComment|AN|255|-|Merchants’ comment concerning the order.
|emailCallback|AN|255|-|Email used by HiPay Professional to post operation notifications.
|freedata|AN|-|-|Custom data. You may use these parameters to submit values you wish to receive back in the API response messages or in the notifications, e.g. you can use these parameters to get back session data, order content or user info.

You must format it like this:

``` xml  
<freeData>
 <item>
  <key>keyOne</key>
  <value>ValueOne</value>
 </item>
 <item>
  <key>keyTwo</key>
  <value>ValueTwo</value>
 </item>
</freeData>
```

**Response Fields**
The following table lists and describes the response fields.

| Field Name   |      Description    |
|----------|:-------------:|
| redirectUrl |  Payment page URL. Merchant must redirect the customer’s browser to this URL.
| code |  Status code of the answer.
| description |  Reason description (if error).

##Refund an order
To perform a refund on an existing transaction, make an HTTP POST request to the following resource.

| Operation Type   |      Resource    | Description    |
|----------|:-------------:|:-------------:|
| refund |  /soap/refund-v2/card |  A request instructing the payment gateway to fully refund a previously credit card captured transaction. 

**Request Parameters**

| Parameter   |      Format    |      Length    |      Req    |      Description    |
|----------|:-------------:|-------------:|-------------:|-------------:|
| wsLogin |  AN |  32 |  M |  Your API Webservice Login.
| wsPassword |  AN |  32 |  M |  Your API Webservice Password.
| websiteId |  N |  32 |  M |  Id of merchants’ website were transaction was made.
| transactionPublicId |  AN |  32 |  M |  The unique identifier of the transaction sent to the merchant on the urlCallback (Notification) called “transid”.

**Response Fields**

The following table lists and describes the response fields.

| Field Name   |      Description    |
|----------|:-------------:|
| transactionPublicId |  The unique identifier of the transaction.
| code |  Status code of the answer.
| description |  Description of the answer.
| amount |  Refunded amount.
| currency |  Currency of refunded transaction.

##Maintenance Operations
To perform maintenance on an existing transaction, make an HTTP POST request to the following resources.

| Operation Type   |      Resource    |      Description    |
|----------|:-------------:|-------------:|
| confirm |  /soap/transaction-v2/confirm |  A request instructing the payment gateway to capture a previously-authorized transaction, i.e. transfer the funds from the customer's bank account to the merchant's bank account. This transaction is always preceded by an authorization.
| cancel |  /soap/transaction-v2/cancel |  A request instructing the payment gateway to cancel a previously-authorized transaction. Only authorized transactions can be canceled, captured transactions must be refunded.

**Request Parameters**

| Parameter   |      Format    |      Length    |      Req    |      Description    |
|----------|:-------------:|-------------:|-------------:|-------------:|
| wsLogin |  AN |  32 |  M |  Your API Webservice Login.
| wsPassword |  AN |  32 |  M |  Your API Webservice Password.
| transactionPublicId |  AN |  32 |  M |  The unique identifier of the transaction sent to the merchant on the urlCallback (Notification) called “transid”.

**Response Fields**

The following table lists and describes the response fields.

| Field Name   |      Description    |
|----------|:-------------:|
| transactionPublicId |  The unique identifier of the transaction.
| code |  Status code of the answer.
| description |  Description of the answer.

**Examples**

The following are SOAP XML examples.

*Example Request*

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="https://ws.hipay.com/soap/refund-v2">
   	<SOAP-ENV:Body>
   	  <ns1:card>
   	   <parameters>
   		  <websiteId>107</websiteId>
   	    <transactionPublicId>51X6F4C37X253</transactionPublicId>
   		  <wsLogin>4f8x3e8xfd4ex76f1ex27c6xfefdb3e5</wsLogin>   	    <wsPassword>2x0c409xc4254x819bfx246d6x02d1x2</wsPassword>
   	   </parameters>
   	  </ns1:card>
   	</SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

*XML Response Example*

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="https://ws.hipay.com/soap/refund-v2">
   	 <SOAP-ENV:Body>
   	  <ns1:cardResponse>
   	   <cardResult>
   		  <code>0</code>
   		  <description>Refund to credit card request for transaction 51X6F4C37X253 has been sent !</description>
   		  <currency>EUR</currency>
   		  <amount>4</amount>
   		  <transactionPublicId>51X6F4C37X253</transactionPublicId>
   	   </cardResult>
   	  </ns1:cardResponse>
   	 </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```


#Server-to-Server Notifications
##What is a Server-to-Server Notification?
**Description**: In order to notify events related to your payment system, such as a new transaction or a 3-D Secure transaction, our platform can send to your application a Server-to-Server notification.

##Setup
To set your Notification URL you must set it on "urlCallback" parameter at the moment of generate a new order (please refer to Chapter 3.1 Request a New Order).
After a successful purchase, HiPay calls twice your Notification URL in background with comprehensive information about the payment passed through an HTTP POST parameter in an XML array `POST['xml']` the first time for the authorization notification and the second one for the capture notification.

| Operation Type   |      Description    |
|----------|:-------------:|
| authorization |  Authorization from the customer’s bank to make the capture.
| capture |  Notification of the real capture to debit the customer’s account.
| cancellation |  Previously-authorized transaction was cancelled.
| refund |  Previously-captured transaction was refunded.
| reject |  Charge Back. The cardholder reversed a capture processed by their bank or credit card company. For instance, the cardholder contacts his credit card company and denies having made the transaction. The credit card company then revokes the already captured payment. Please note the legal difference between shopper who ordered the goods and cardholder who owns the credit card and ends up paying for the order.<br/>In general charge backs only occur incidentally. When they do, contacting the shopper can often solve the situation. Occasionally it is an indication of credit card fraud.
*Table 8: Types of possible actions (represented by the value of operation variable)*

| Operation Type   |      Description    |
|----------|:-------------:|
| ok |  Operation succeeded.
| nok |  Operation not succeeded.
| cancel |  Cancelation of the operation.
| waiting |  Operation waiting for an action.

*Table 9: Types of possible status (represented by the value of status variable):*

**Response Fields** :

The following table lists and describes the response fields received on the notification call.

| Field Name	  |      Description    |
|----------|:-------------:|
| operation |  Operation Type. Please report to “Types of possible actions” table.
| status |  Operation Status. Please report to “Types of possible status” table.
| date |  Date of the transaction (YYYY-mm-dd).
| time |  Time of the transaction (e.g., 11:00:58 UTC+0000).
| origAmount |  The total order amount (e.g., 150.00). It should be calculated as a sum of the items purchased, plus the shipping fee (if present), plus the tax fee (if present).
| origCurrency |  Base currency for this order. This three-character currency code complies with ISO 4217.
| idForMerchant |  The transaction ID used by the merchant.
| emailClient |  Email address of the customer.
| merchantDatas |  Custom merchant data provided at the moment of generate a new order (please refer to Chapter 3.1 Request a New Order).
| transid |  The unique identifier of the transaction.
| is3ds |  Indicates if the used card is 3-D Secure enrolled.
| paymentMethod |  Payment method used by the customer.
| customerCountry |  Country code of the customer. This two-letter code complies with ISO-3166.
| returnCode |  -
| returnDescriptionShort |  -
| returnDescriptionLong |  -

*Notification POST Example*

```xml
<mapi>
   	 <mapiversion>1.0</mapiversion>
   	 <md5content>05049866c4538df62c780fdb08b95ee0</md5content>
   	 <result>
   	  <operation>authorization</operation>
   	  <status>nok</status>
   	  <date>2014-02-11</date>
   	  <time>11:00:58 UTC+0000</time>
   	  <origAmount>1.00</origAmount>
   	  <origCurrency>EUR</origCurrency>
   	  <idForMerchant>ORDER123456</idForMerchant>
   	  <emailClient>customer@mycustomer.com</emailClient>
   	  <merchantDatas>  
   	   <_aKey_keyOne>aValueOne</_aKey_keyOne>  
   	   <_aKey_keyTwo>aValueTwo</_aKey_keyTwo>
   	  </merchantDatas>
   	  <transid>52F9FFA68486E</transid>
   	  <is3ds>Yes</is3ds>
   	  <paymentMethod>VISA</paymentMethod>
   	  <customerCountry>FR</customerCountry>
   	  <returnCode></returnCode>
   	  <returnDescriptionShort></returnDescriptionShort>
   	  <returnDescriptionLong></returnDescriptionLong>
   	 </result>
</mapi>;
```

#Signature verification

##Introduction
It is strongly recommended to use a signature mechanism to verify the contents of a request or redirection made to your servers.

This prevents customers from tampering with the data in the data exchanges between your servers and our payment system.
A unique signature is sent each time that HiPay contact a merchant URL.

##Verification
For the URL notification, the signature is sent on the "md5content" parameter, to verify it, you just need so concatenate your "wsPassword" with the "result" part of the XML posted to you.

**Algorithm**:`Md5 Signature = md5('<result>...</result>' + wsPassword)`