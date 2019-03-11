# HiPay Marketplace - REST API to upload KYC documents

## Document conventions

Conventions help to locate and interpret information. This guide uses
several typographic conventions to highlight certain words or phrases
and point out specific pieces of information.

The following table clarifies the conventions used throughout this
guide.

|Convention            |Meaning
|--------------------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
|`Mono-space`            |Source code, code examples, input to the command line, application output, code lines embedded in text, variables and code elements
|`Italics`               |A term that is being defined or will be defined shortly.
|`...`                   |Horizontal ellipsis points in sample code indicate the omission of part of the code (i.e. when you would normally expect additional code to appear, but such code would not be related to the example).
|`$`                    |At the beginning of a command, indicates an operating system shell prompt.
|`<placeholder>`   |Indicates placeholders, most often method or function parameters; these placeholders represent information that must be supplied by the implementation or the user. For command-line input, indicates parameter values.
*Table 1: Typographic conventions used throughout this guide*

## Abbreviations used in tables

The tables of this document describe data elements, which are equivalent
to parameters in a query or to fields of a response message. The
following table helps understanding the different attributes (columns)
that define a data element.

|Convention    |Meaning
|------------- |-------------------------------------------------------------------------------------------------------------
|`Field name`    |Name of the data element
|`Format`        |Format of the element
|`Length`        |Maximum size of the data element (i.e. a size of 6 means that the data value cannot exceed six characters)           *Note: there is no limitation on the length of the size, when no size is specified.*
|`Req.`          |Specifies whether an element is required or not.<br/>**M** = Mandatory -&gt; must always be provided. <br/>**C** = Conditional -&gt; requirement depends on the value or presence of other elements
|`Description`   |Brief description of the data element and, where necessary, a list of valid values and element dependencies
*Table 2: Description of data elements*

|Format abbreviation   |Description                                                 |Example
|--------------------- |----------------------------------------------------------- |------------
|`AN`                    |Alphanumeric characters (a-z, A-Z, 0-9)                     |&nbsp;|
|`A`                     |Alphabetic characters only (a-z, A-Z)                       |&nbsp;|
|`N`                     |Numeric characters only                                     |&nbsp;|
|`R`                     |Decimal number with explicit decimal point, signed          |`12.34`
|`DT`                    |Date in the format YYYY-MM-DD                               |`2017-01-31`
|`TM`                    |Time in the format HH:MM with optional seconds (HH:MM:SS)   |`15:30`
*Table 3: Available formats of data elements*

##Acronyms and abbreviations 

The following acronyms and abbreviations are used in this guide.

|Acronym/abbreviation   |Full name or meaning
|---------------------- |---------------------------------
|`REST`                   |Representational State Transfer
|`KYC`                    |Know your customer
|`NA`                     |Not applicable
*Table 4: Acronyms and abbreviations*

##Response status codes

HTTP codes indicate whether a request was successful or not.

-   `200`: OK
-   `400`: Bad request (data error)
-   `401`: Unauthorized (login / password error)
-   `403`: Forbidden (unauthorized account)
-   `404`: Not found
-   `500`: Server error
-   `503`: Service unavailable

#How to upload KYC documents with the specific REST API

This web service enables you to upload KYC documents to a HiPay user
account.

Sent KYC documents are automatically processed in real time if the
pictures provided are usable. Otherwise, a manual check is done within 72 hours (working days).

If a document is uploaded and sent by mistake, please wait until it is
refused to re-submit the right document.

Please note that all the information collected will be kept confidential
and will not be disclosed to third parties, except as provided by the
law.

##Document constraints

-   Accepted formats: `JPEG, JPG, GIF, PDF, PNG`
-   Maximum file size: `5 MB`
-   A certified translation is required for documents in a non-Latin language

##Service endpoints

There are two endpoints (base URLs) to which you can make your API calls.

-   **Stage:** if you are testing your integration, and
-   **Production:** when you have finished testing and want your application to go live.

|Environment   |Endpoint
|------------- |----------
|`Stage`        |[*https://test-merchant.hipaywallet.com/api/identification*](https://test-merchant.hipaywallet.com/api/identification)<br/>{format} (POST)
|`Production`    |[*https://merchant.hipaywallet.com/api/identification*](https://merchant.hipaywallet.com/api/identification)<br/>{format} (POST)
*Table 5: Service endpoints*

##Format

**JSON | XML**

The format is not mandatory, but you can choose the format of the received response.
By default, the response is in JSON format.

##Authentication 

This web service requires a **basic HTTP** authentication, with the HiPay technical account wsLogin and wsPassword.

##Types of KYC documents

### For individuals

|Type   |Label          |Description
|------ |---------------|--------------
|`1`    | ID proof      | Copy of a valid identification document<br/>(e.g.: passport or ID card<br/>– 2 files accepted for front and back sides*)
|`2`    | Proof of address   |Proof of address issued within the last three months<br/>(e.g.: rent receipt, utility bill)
*Table 6: KYC documents for individuals*

### For professionals (corporations)

  
|Type   |Label               |Description
|------ |----------------------- |--------------------------------------------------------------------------------------------------------------
|`3`      |Identity card           |Copy of a valid identification document of the legal representative<br/>(e.g.: passport or ID card – 2 files accepted for front and back sides*)
|`4`      |Company Registration    |Document certifying company registration issued within the last three months (Kbis extract)
|`5`      |Distribution of power   |Signed Articles of Association with the division of powers (featuring the address of the head office, the name of the company and the name of the legal representative)
*Table 7: KYC documents for professionals (corporations)*

### For professionals (persons)

|Type   |**Label**              |**Description**
|------ |---------------------- |---------------------------------------------------------------------------
|`7`      |ID proof               |Copy of a valid identification document of the legal representative<br/>(e.g.: passport or ID card – 2 files accepted for front and back sides*)
|`8`      |Company Registration   |Document certifying registration issued within the last three months (Kbis extract)
|`9`      |Tax status             |Document certifying tax status (“auto-entrepreneur” / independent /…)
*Table 8: KYC documents for professionals (persons)*

### For associations

|Type   |**Label**                  |**Description**
|------ |-------------------------- |---------------------------------------------------------------------------
|`7`      |ID proof                   |Copy of a valid identification document of the legal representative<br/>(e.g.: passport or ID card – 2 files accepted for front and back sides*)
|`11`     |President of association   |Document certifying who is the president of the association
|`12`    |Official Journal           |Publication in the Official Journal
|`13`     |Association status         |Document certifying the Association statutes
*Table 9: KYC documents for associations*

*Only identification documents (types 1, 3 & 7) enable to upload two files for front and back sides (one per request).

**Please note:** each category of account also requires a document
entitled “Identification of Controlling Stakeholders”, duly completed,
signed and accompanied by supporting identification documents of
beneficial owners (currently to be sent by email to
*wallet-ubo@hipay.com* until the feature under development to send it by
API is completed).

##Request header 

You may use the Account ID or the Account email login to select an
account different from the authenticated one.

|Field name                  |**Format**   |**Length**   |**Req.**   |**Description**
|--------------------------- |------------ |------------ |---------- |------------------------------------------------------------------------
|`php-auth-subaccount-id`      |N            |12           |C          |Account ID of the HiPay account for which to upload the KYC documents
|`php-auth-subaccount-login`   |AN           |255          |C          |Email login of the HiPay account for which to upload the KYC documents
*Table 10: KYC document upload related header*

##Request parameters

|Field name       |**Format**   |**Length**   |**Req.**   |**Description**
|---------------- |------------ |------------ |---------- |-----------------------------------------------------------------
|`type`             |N            |12           |M          |Type of KYC document (please see [here](#how-to-upload-kyc-documents-with-the-specific-rest-api-types-of-kyc-documents) for a detailed description)
|`file`             |AN           |255          |M          |KYC document to upload<br/>(1 document per request*)<br/>- Accepted formats:<br/>`JPEG, JPG, GIF, PDF, PNG`<br/>- Maximum file size: `5 MB`
|`validity_date`   |D            |10            |C           |Document expiry date<br/>(`YYYY-MM-DD` format)<br/>- Mandatory for identification documents (types `1, 3 or 7`) <br/>- Optional for other KYC documents
*Table 11: KYC document upload related parameters*

*For identification documents (types 1, 3 & 7), please make two
requests to upload one file for the front side and one file for the back
side respectively.

###Example of a cURL request 

```shell
curl -v -X POST
https://merchant.hipaywallet.com/api/identification.json
-u "XX12xxab000b00ab0101010abcd0a0xx:abc0abcd1ee1ff6789abc2345fff0000"
-H "php-auth-subaccount-id: 543210"
-F "file=@/home/johndoe/Desktop/Projects/uploads/DSCF1131.jpg"
-F "type=1" 
-F "validity_date=2025-05-17"
```

###Example of a PHP request

```php
define('API_ENDPOINT', 'https://merchant.hipaywallet.com/api/identification');
define('API_USERNAME', 'XX12xxab000b00ab0101010abcd0a0xx');
define('API_PASSWORD', 'abc0abcd1ee1ff6789abc2345fff0000');
$credentials = API_USERNAME . ':' . API_PASSWORD;

$resource = API_ENDPOINT;

// create a new cURL resource
$curl = curl_init();

// request headers
$header = array(
  'Accept: application/json',
  'php-auth-subaccount-id: 543210
);
// request parameters
$data = array(
  'type' => 1,
  'validity_date' => '2025-05-17',
  'file' => '@/home/johndoe/desktop/Projects/uploads/DSCF1131.jpg'
);

// set appropriate options
$options = array(
  CURLOPT_URL => $resource,
  CURLOPT_HTTPHEADER => $header,
  CURLOPT_USERPWD => $credentials,
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_FAILONERROR => false,
  CURLOPT_HEADER => false,
  CURLOPT_POST => true,
  CURLOPT_POSTFIELDS => $data
);

foreach ($options as $option => $value) {
  curl_setopt($curl, $option, $value);
}

// execute the given cURL session
if (false === ($result = curl_exec($curl))) {
  throw new RuntimeException(curl_error($curl), curl_errno($curl));
}

$status = (int)curl_getinfo($curl, CURLINFO_HTTP_CODE);
$response = json_decode($result);

var_dump($response);

curl_close($curl);
```

##Response examples

###Examples of responses in JSON format

*When the document upload is successful:*

```json
{ 
   "code":0, 
   "message":"Document uploaded", 
   "user_space":012345, 
   "type":6 
} 
```

*When validation failed:*

```json
{ 
  "code": 400, 
  "message": "Validation Failed", 
  "errors": [ 
    { 
      "field": "type", 
      "message": "This value should not be blank." 
    }, 
    { 
      "field": "file", 
      "message": "This value should not be blank." 
    } 
  ] 
}  
```

*When the account is not allowed (identity theft):*

```json
{ 
  "code": 403, 
  "message": "This account is not allowed to use this webservice for the user space 012345" 
}
```

*When the user space has already been identified:*

```json
{ 
  "code": 414, 
  "message": "User space 012345 already identified" 
} 
```

*When the document has already been validated or is pending validation:*

```json
{ 
  "code": 415, 
  "message": "A document of this type is already validated or waiting for validation. Please contact identification@hipay.com."
} 
```


### Example of an ok response in XML format

```xml
<?xml version="1.0" encoding="UTF-8"?>
<result>
  <code>0</code>
  <message><![CDATA[Document uploaded]]></message>
  <user_space>012345</user_space>
  <type>1</type>
  <validity_date><![CDATA[2025-06-01]]></validity_date>
</result>
```

If an error occurs, the API returns status code 4xx or 5xx, with a
descriptive message.

### Example of an authentication error response in JSON format 

```json
{
  "code": 401
  "message": "Authentication Failed."
}
```

### Example of an authentication error response in XML format 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<result>
  <code>401</code>
  <message>Authentication Failed.</message>
</result>
```

<span id="_Toc446517880" class="anchor"><span id="_Toc458697224"
class="anchor"><span id="_Toc474760784"
class="anchor"></span></span></span>

### Example of a parameter error response in JSON format

```json
{
  "code":400,
  "message":"Validation Failed",
  "errors":[
    {
      "field":"validity_date",
      "message":"This value should not be blank"
    }
  ]
}
```
### Example of a parameter error response in XML format

```xml
<?xml version="1.0" encoding="UTF-8"?>
<result>
  <code>400</code>
  <message><![CDATA[Validation Failed]]></message>
  <errors>
    <entry>
      <field><![CDATA[validity_date]]></field>
      <message><![CDATA[This value should not be blank]]></message>
    </entry>
  </errors>
</result>
```

#How to request the status of sent KYC documents  

This web service enables you to request the list of sent KYC documents
as well as their status.

##Service endpoints

There are two endpoints (base URLs) to which you can make your API
calls.

-   **Stage:** if you are testing your integration, and

-   **Production:** when you have finished testing and want your
    application to go live.

|Environment   |Endpoint
|------------- |----------
|`Stage`        |[*https://test-merchant.hipaywallet.com/api/identification*](https://test-merchant.hipaywallet.com/api/identification)<br/>{format} (POST)
|`Production`    |[*https://merchant.hipaywallet.com/api/identification*](https://merchant.hipaywallet.com/api/identification)<br/>{format} (POST)
*Table 12: Service endpoints*

##Format 

**JSON | XML**

The format is not mandatory, but you can choose the format of the received response.

By default, the response is in JSON format.

##Authentication

This web service requires a basic HTTP authentication, with the HiPay technical account wsLogin and wsPassword.

##Request header 

You may use the Account ID or the Account email login to select an account different from the authenticated one.

|**Field name**              |**Format**   |**Length**   |**Req.**   |**Description**
|--------------------------- |------------ |------------ |---------- |-------------------------------------------------------------------------------------------------------
|`php-auth-subaccount-id`    |  N          |  12         |  C        | Account ID of the HiPay account for which to request the list of sent KYC documents and their status
|`php-auth-subaccount-login` |  AN         |  255        |  C        | Email login of the HiPay account for which to request the list of sent KYC documents and their status
*Table 13: KYC document status request related header*

### Example of a cURL request 

```shell
curl -v -X GET
https://merchant.hipaywallet.com/api/identification.json
-u "XX12xxab000b00ab0101010abcd0a0xx:abc0abcd1ee1ff6789abc2345fff0000"
-H "php-auth-subaccount-id: 543210"
```

### Example of a PHP request 

```php
define('API_ENDPOINT', 'https://merchant.hipaywallet.com/api/identification.json');
define('API_USERNAME', 'XX12xxab000b00ab0101010abcd0a0xx');
define('API_PASSWORD', 'abc0abcd1ee1ff6789abc2345fff0000');
$credentials = API_USERNAME . ':' . API_PASSWORD;

$resource = API_ENDPOINT;

// create a new cURL resource
$curl = curl_init();

// header parameters
$header = array(
  'Accept: application/json',
  'php-auth-subaccount-id: 543210
);

// set appropriate options
$options = array(
  CURLOPT_URL => $resource,
  CURLOPT_HTTPHEADER => $header,
  CURLOPT_USERPWD => $credentials,
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_FAILONERROR => false,
  CURLOPT_HEADER => false,
  CURLOPT_POST => false,
);

foreach ($options as $option => $value) {
  curl_setopt($curl, $option, $value);
}

// execute the given cURL session
if (false === ($result = curl_exec($curl))) {
  throw new RuntimeException(curl_error($curl), curl_errno($curl));
}

$status = (int)curl_getinfo($curl, CURLINFO_HTTP_CODE);
$response = json_decode($result);

var_dump($response);

curl_close($curl);
```

##Response fields 

|Field name               |**Description**
|------------------------ |----------------------------------------------------------------------------------------------
|`code`                     |Indicates if the API call has been done without errors (0 = success)
|`message`                  |Displays the message related to the code
|`user_space`               |ID of the user space being verified
|`identification_status`    |User space identification status<br/>- Unidentified: the user space is not identified, documents are missing<br/>- Identified<br/>- Identification in progress: a manual check is being performed to identify the user space
|`documents`                |All the required KYC documents for this user space:<br/>- type: please see [here](#how-to-upload-kyc-documents-with-the-specific-rest-api-types-of-kyc-documents) for a detailed description<br/>- label: please see [here](#how-to-upload-kyc-documents-with-the-specific-rest-api-types-of-kyc-documents) for a detailed description<br/>- status_code: please see table 15 for a detailed description<br/>- status_label: please see table 15 for a detailed description
*Table 14: Response fields*

##Status codes and status labels

|status_code        |status_label        |Description
|------------------ |------------------- |--------------------------------------------------------------------------------
|`-1`               |`NA`                | No document has been uploaded
|`0`                |`New`               | The document has been uploaded but not sent
|`1`                |`Waiting`           | The document has been sent to HiPay
|`2`                |`Validated`         | The document has been validated for identification
|`3`                |`Refused`           | The document has been refused because it is falsified, expired or inconsistent
|`5`                |`Waiting`           | The document is being reviewed
|`8`                |`Refused`           | The document has been refused
|`9`                |`Waiting`           | A new review of the document is in progress
*Table 15: Status codes and status labels*

<span id="_Toc458697235" class="anchor"><span id="_Toc474760795"
class="anchor"></span></span>

##Response examples 

If the web service responds with the success code (0), all the required KYC documents are detailed in a JSON message as follows.

### Example of an ok response in JSON format

```json
{
  "code": 0,
  "message": "Identification documents list succeeded",
  "user_space": 012345,
  "identification_status": "Identified",
  "documents": [
    {
      "type": 3,
      "label": "Identity card",
      "status_code": 2,
      "status_label": "Validated"
    },
    {
      "type": 4,
      "label": "Company Registration",
      "status_code": 2,
      "status_label": "Validated"
    },
    {
      "type": 5,
      "label": "Distribution of power",
      "status_code": 2,
      "status_label": "Validated"
    },
    {
      "type": 6,
      "label": "Bank",
      "status_code": 2,
      "status_label": "Validated"
    }
  ]
}
```

### Example of an ok response in XML format 

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<result>
  <code>0</code>
  <message><!--[CDATA[Identification documents list succeeded]]--></message>
  <user_space>012345</user_space>
  <identification_status><!--[CDATA[Unidentified]]--></identification_status>
  <documents>
    <entry>
      <type>1</type>
      <label><!--[CDATA[Identity card]]--></label>
      <status_code>9</status_code>
      <status_label><!--[CDATA[Waiting]]--></status_label>
    </entry>
    <entry>
      <type>2</type>
      <label><!--[CDATA[Proof of address]]--></label>
      <status_code>9</status_code>
      <status_label><!--[CDATA[Waiting]]--></status_label>
    </entry>
  </documents>
</result>
```

If an error occurs, the API returns status code 4xx or 5xx, with a descriptive message.

### Example of an authentication error response in JSON format 

```json
{
  "code": 401,
  "message": "Authentication Failed."
}
```

### Example of an authentication error response in XML format 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<result><code>401</code>
<message>Authentication Failed.</message>
</result>
```

#Server-to-server notifications 

##What is a server-to-server notification?

In order to inform you of events related to KYC document validation, the
HiPay platform can send your application a server-to-server
notification.

To set your notification URL, you must send it by email to the HiPay
support team at [*support.mkp@hipay.com*](mailto:support.mkp@hipay.com).

##Response fields 
|Field name              |**Description**
|----------------------- |----------------------------------------------------------------------------
|`md5content`              |SHA1 encoding of the &lt;result&gt; field
|`operation`               |Name of the notification (document_validation)
|`status`                  |- `ok`: the document is validated<br/>- `nok`: the document is not validated
|`message`                 |Description of the status
|`date`                    |Date of the notification (`YYYY-MM-DD`)
|`time`                    |Time of the notification (`HH:MM:SS Time zone`)
|`document_type`           |Type of KYC document<br/>(please see [here](#how-to-upload-kyc-documents-with-the-specific-rest-api-types-of-kyc-documents) for a detailed description)
|`document_type_label`     |Description of the document (please see [here](#how-to-upload-kyc-documents-with-the-specific-rest-api-types-of-kyc-documents) for a detailed description)
|`account_id`              |HiPay account ID for which the KYC was uploaded
*Table 16: Response fields*

##Callback messages by document type 

|Type              |**Message**
|---------------   |-------------------------------------------
|`6`               |Bank outside of perimeter
|`1,2,3,4,7,8`     |Expired
|`2,4,6,8`         |Falsified
|`1,3,7`           |Falsified or under age
|`2,4,8`           |Incoherence: error with address
|`2`               |Incoherence: error with holder
|`4,8`             |Incoherence: error with name
|`1,3,6,7`         |Incoherence: name and first name reversed
|`1,3,6,7`         |Incoherence: other error
|`6`               |Not a bank ID
|`2`               |Not a proof of address
|`1,3,7`           |Not an ID
|`1,3,7`           |One side missing
|`2`               |Supplier outside of perimeter
|`1,2,3,4,6,7,8`   |Unreadable or cut
|`1,2,3,4,6,7,8`   |Valid and certified
|`1,3,7`           |Valid but not certifiable
|`2,6`             |Verified except holder’s first name
|`1,2,3,4,6,7,8`   |Waiting
*Table 17: Callback messages by document type*

##Response example 

### Example of a KYC document notification in XML format 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<notification>
   <md5content>d9e43bb0a9b92aba4fb22eed4164a1f0</md5content>
   <result>
      <operation>document_validation</operation>
      <status>nok</status>
      <message>Not a proof of address</message>
      <date>2016-06-29</date>
      <time>16:39:42 Europe/Paris+0200</time>
      <document_type>2</document_type>
      <document_type_label>Proof of address</document_type_label>
      <account_id>543210</account_id>
   </result>
</notification>
```
