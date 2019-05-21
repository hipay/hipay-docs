# HiPay Marketplace – Identification API

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
|`DT`                    |Date in the format YYYY-MM-DD                               |`2018-01-31`
|`TM`                    |Time in the format HH:MM with optional seconds (HH:MM:SS)   |`15:30`
*Table 3: Available formats of data elements*

##Acronyms and abbreviations 

The following acronyms and abbreviations are used in this guide.

|Acronym/abbreviation   |Full name or meaning
|---------------------- |---------------------------------
|`REST`                   |Representational State Transfer
|`KYC`                    |Know your customer
|`KYB`                    |Know your business
|`NA`                     |Not applicable
*Table 4: Acronyms and abbreviations*

##Response status codes

Status codes or HTTP codes indicate whether a request was successful or not.

|Code   |Message                                                 |Description
|--------------------- |----------------------------------------------------------- |------------
|`0`|Success|Everything went well.|
|`2`|Missing param|Both the front and back sides of ID cards are required.|
|`200`|OK|The request was successful.|
|`400`|Bad request / Validation failed|Data error: one or several parameters sent to the API are not valid.|
|`401`|Unauthorized|Login/password error: authentication with the wsLogin and wsPassword failed.|
|`403`|Forbidden|Unauthorized account: a document was uploaded without the proper access rights for that user account.|
|`404`|Not found||
|`414`|Already identified|A document was uploaded for an account that is already identified.|
|`415`|Already uploaded|This type of document has already been uploaded for this user account.|
|`500`|Server error||
|`503`|Service unavailable||

#How to upload KYC/KYB documents with this specific REST API

This web service enables you to upload KYC/KYB documents to a HiPay user
account.

Sent KYC/KYB documents are automatically processed in real time if the
documents provided are usable. Otherwise, a manual review is performed within 72 hours (working days).

If a document is uploaded and sent by mistake, please wait until it is
refused to re-submit the right document.

Please note that all the information collected will be kept confidential
and will not be disclosed to third parties, except as provided by the
law.

##Document constraints

-   Accepted formats: `JPG, GIF, PDF, PNG`
-   Maximum file size: `15 MB`
-   A certified translation is required for documents in a non-Latin language

##Service endpoints

There are two endpoints (base URLs) to which you can make your API calls.

-   **Stage:** if you are testing your integration, and
-   **Production:** when you have finished testing and want your application to go live.

|Environment   |Endpoint
|------------- |----------
|`Stage`        |[*https://test-professional.hipay.com/api/identification*](https://test-professional.hipay.com/api/identification)<br/>{format} (POST)
|`Production`    |[*https://professional.hipay.com/api/identification*](https://professional.hipay.com/api/identification)<br/>{format} (POST)
*Table 5: Service endpoints*

##Format

**JSON | XML**

The format is not mandatory, but you can choose the format of the received response.
By default, the response is in JSON format.

##Authentication 

This web service requires a **basic HTTP** authentication, with the HiPay technical account wsLogin and wsPassword.

##Types of KYC/KYB documents

### For individual accounts

|Type   |Label          |Description
|------ |---------------|--------------
|`1`    | ID proof      |A copy of front and back sides of a valid identification document<br/>(ID card or passport) of the account holder (person)
|`2`    | Proof of address   |Proof of address issued within the last 3 months<br/>(e.g.: utility/phone bill, rent receipt)
*Table 6: KYC/KYB documents for individual accounts*

### For professional accounts (corporations)

  
|Type   |Label               |Description
|------ |----------------------- |--------------------------------------------------------------------------------------------------------------
|`1`      |ID proof           |A copy of front and back sides of a valid identification document<br/>(ID card or passport) of the legal representative
|`4`      |Company Registration    |Certificate of incorporation (document certifying Company registration) issued within the last 3 months (Kbis extract or any equivalent document)
|`5`      |Distribution of power   |Signed Articles of Association with the division of powers (featuring the address of the head office, the name of the company and the name of the legal representative)
*Table 7: KYC/KYB documents for professional accounts (corporations)*

### For professional accounts (persons)

|Type   |**Label**              |**Description**
|------ |---------------------- |---------------------------------------------------------------------------
|`1`      |ID proof               |A copy of front and back sides of a valid identification document<br/>(ID card or passport) of the individual
|`8`      |Company Registration   |Registration number with the Registry of Companies (SIREN number or its equivalent)
*Table 8: KYC/KYB documents for professional accounts (persons)*

### For association accounts

|Type   |**Label**                  |**Description**
|------ |-------------------------- |---------------------------------------------------------------------------
|`1`      |ID proof                   |A copy of front and back sides of a valid identification document<br/>(ID card or passport) of the legal representative
|`11`     |President of association   |Document certifying the function and the name of the legal representative of the association
|`12`    |Official Journal           |Publication in the dedicated register (or equivalent) or a receipt of the registration of the association issued by public authorities
|`13`     |Association status         |A copy of the association statutes
*Table 9: KYC/KYB documents for association accounts*

**Please note:** Professional (corporations) and association accounts also require a document
entitled “Identification of Ultimate Beneficial Owners”, duly completed,
signed and accompanied by supporting identification documents of
beneficial owners (to be sent through [*https://professional.hipay.com/login*](https://professional.hipay.com/login): to access this platform, please contact your HiPay account manager).

##Request header 

You may use the Account ID or the Account email login to select an
account different from the authenticated one.

|Field name                  |**Format**   |**Length**   |**Req.**   |**Description**
|--------------------------- |------------ |------------ |---------- |------------------------------------------------------------------------
|`php-auth-subaccount-id`      |N            |12           |C          |Account ID of the HiPay account for which to upload the KYC/KYB documents
|`php-auth-subaccount-login`   |AN           |255          |C          |Email login of the HiPay account for which to upload the KYC/KYB documents
*Table 10: KYC/KYB document upload related header*

##Request parameters

|Field name       |**Format**   |**Length**   |**Req.**   |**Description**
|---------------- |------------ |------------ |---------- |-----------------------------------------------------------------
|`type`             |N            |12           |M          |Type of KYC/KYB document (please see [here](#how-to-upload-kyckyb-documents-with-this-specific-rest-api-types-of-kyckyb-documents) for a detailed description)
|`file`             |AN           |255          |M          |KYC/KYB document to upload<br/>- Accepted formats:<br/>`JPG, GIF, PDF, PNG`<br/>- Maximum file size: `15 MB`
|`back`             |AN           |255          |C          |Back side of the identification document to upload<br/>- Accepted formats:<br/>`JPG, GIF, PDF, PNG`<br/>- Maximum file size: `15 MB`
|`validity_date`   |D            |10            |C           |Document expiry date<br/>(`YYYY-MM-DD` format)<br/>- Mandatory for identification documents (type `1`) <br/>- Optional for other KYC/KYB documents
*Table 11: KYC/KYB document upload related parameters*


###Example of a cURL request 

```shell
curl -v -X POST
https://professional.hipay.com/api/identification.json
-u "XX12xxab000b00ab0101010abcd0a0xx:abc0abcd1ee1ff6789abc2345fff0000"
-H "php-auth-subaccount-id: 543210"
-F "file=@/home/johndoe/Desktop/Projects/uploads/DSCF1131.jpg"
-F "type=1" 
-F "validity_date=2025-05-17"
```

###Example of a PHP request

```php
define('API_ENDPOINT', 'https://professional.hipay.com/api/identification');
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
   "type":1 
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

#How to request the status of sent KYC/KYB documents  

This web service enables you to request the list of sent KYC/KYB documents
as well as their status.

##Service endpoints

There are two endpoints (base URLs) to which you can make your API
calls.

-   **Stage:** if you are testing your integration, and

-   **Production:** when you have finished testing and want your
    application to go live.

|Environment   |Endpoint
|------------- |----------
|`Stage`        |[*https://test-professional.hipay.com/api/identification*](https://test-professional.hipay.com/api/identification)<br/>{format} (POST)
|`Production`    |[*https://professional.hipay.com/api/identification*](https://professional.hipay.com/api/identification)<br/>{format} (POST)
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
|`php-auth-subaccount-id`    |  N          |  12         |  C        | Account ID of the HiPay account for which to request the list of sent KYC/KYB documents and their status
|`php-auth-subaccount-login` |  AN         |  255        |  C        | Email login of the HiPay account for which to request the list of sent KYC/KYB documents and their status
*Table 13: KYC/KYB document status request related header*

### Example of a cURL request 

```shell
curl -v -X GET
https://professional.hipay.com/api/identification.json
-u "XX12xxab000b00ab0101010abcd0a0xx:abc0abcd1ee1ff6789abc2345fff0000"
-H "php-auth-subaccount-id: 543210"
```

### Example of a PHP request 

```php
define('API_ENDPOINT', 'https://professional.hipay.com/api/identification.json');
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
|`user_space`               |ID of the user space being verified <br/>- Deprecated: will be removed soon
|`identification_status`    |User space identification status<br/>- Unidentified: the user space is not identified, documents are missing<br/>- Identified<br/>- Identification in progress: a manual review is being performed to identify the user space
|`documents`                |All the required KYC/KYB documents for this user space:<br/>- type: please see [here](#how-to-upload-kyckyb-documents-with-this-specific-rest-api-types-of-kyckyb-documents) for a detailed description<br/>- label: please see [here](#how-to-upload-kyckyb-documents-with-this-specific-rest-api-types-of-kyckyb-documents) for a detailed description<br/>- status_code: please see table 15 for a detailed description<br/>- status_label: please see table 15 for a detailed description
*Table 14: Response fields*

##Status codes and status labels

|status_code        |status_label        |Description
|------------------ |------------------- |--------------------------------------------------------------------------------
|`-1`               |`NA`                | No document has been uploaded
|`1`                |`Waiting`           | The document has been sent to HiPay
|`2`                |`Validated`         | The document has been validated for identification
|`3`                |`Refused`           | The document has been refused because it is falsified, expired or inconsistent
|`5`                |`Waiting`           | The document is being reviewed
|`8`                |`Refused`           | The document has been refused
*Table 15: Status codes and status labels*

<span id="_Toc458697235" class="anchor"><span id="_Toc474760795"
class="anchor"></span></span>

##Response examples 

If the web service responds with the success code (0), all the required KYC/KYB documents are detailed in a JSON message as follows.

### Example of an ok response in JSON format

```json
{
  "code": 0,
  "message": "Identification documents list succeeded",
  "user_space": 012345,
  "identification_status": "Identified",
  "documents": [
    {
      "type": 1,
      "label": "ID proof",
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

In order to inform you of events related to KYC/KYB document validation, the
HiPay platform can send your application a server-to-server
notification.

To set your notification URL, please [submit a request](https://support.hipay.com/hc/en-us/requests/new) to our Support team (specifying HiPay Marketplace).

##Response fields 
|Field name              |**Description**
|----------------------- |----------------------------------------------------------------------------
|`md5content`              |SHA1 encoding of the &lt;result&gt; field
|`operation`               |Name of the notification (document_validation)
|`status`                  |- `ok`: the document is validated<br/>- `nok`: the document is not validated
|`message`                 |Description of the status
|`date`                    |Date of the notification (`YYYY-MM-DD`)
|`time`                    |Time of the notification (`HH:MM:SS Time zone`)
|`document_type`           |Type of KYC/KYB document<br/>(please see [here](#how-to-upload-kyckyb-documents-with-this-specific-rest-api-types-of-kyckyb-documents) for a detailed description)
|`document_type_label`     |Description of the document (please see [here](#how-to-upload-kyckyb-documents-with-this-specific-rest-api-types-of-kyckyb-documents) for a detailed description)
|`account_id`              |HiPay account ID for which the KYC/KYB was uploaded
*Table 16: Response fields*

##Callback messages

Please find hereafter the various reasons for refusal provided by notification.

|Reasons for refusal|
|---------------|
|Invalid date|
|Unreadable|
|Missing data|
|Inconsistency: {personalized-msg}|
|Not verified: missing document|
|Invalid document type|
|Falsified|
|Front missing|
|Supplier outside the scope|
|Bank outside the scope|
|Inconsistent|
|Other|
|Invalid Address|
|Expired|

*Table 17: Callback messages (reasons for refusal)

##Response example 

### Example of a KYC/KYB document notification in XML format 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<notification>
   <md5content>d9e43bb0a9b92aba4fb22eed4164a1f0</md5content>
   <result>
      <operation>document_validation</operation>
      <status>nok</status>
      <message>Not a proof of address</message>
      <date>2018-06-29</date>
      <time>16:39:42 Europe/Paris+0200</time>
      <document_type>2</document_type>
      <document_type_label>Proof of address</document_type_label>
      <account_id>543210</account_id>
   </result>
</notification>
```
