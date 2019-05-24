# HiPay Enterprise JavaScript SDK reference

This is the API reference for the HiPay Enterprise JavaScript SDK in order to collect and tokenize customers' sensitive information securely. 

## Including the HiPay Enterprise JavaScript SDK

The first step to use the HiPay Enterprise JavaScript SDK is to include it on your page.


```html
<script src="https://libs.hipay.com/js/sdkjs.js"></script>
```

The SDK is exposed as a function in a global variable named `HiPay`.

If you only use Hosted Fields via custom fields configuration, you can skip this step.
Otherwise, you can manually include the [base stylesheet](#hipay-enterprise-javascript-sdk-reference-base-stylesheet) to increase loading performance. It will be included automatically if you skip this step.

```html
<link href="https://libs.hipay.com/themes/material.css" rel="stylesheet" />
```

### HiPay (options)

Once the HiPay Enterprise JavaScript SDK is included on your page, you can use the `HiPay` global variable to initialize the SDK.


```js
var hipay = HiPay({
    username: "YOUR_PUBLIC_USERNAME",
    password: "YOUR_PUBLIC_PASSWORD",
    environment: "stage",
    lang: "en"
});
```

Create an instance of the HiPay JavaScript SDK using your HiPay public credentials. You can add options to select your environment.


| Option | Description |
|----------|------------|
| username <br> <small>string `required`</small>| Public HiPay username |
| password <br> <small>string `required`</small>| Public HiPay password |
| environment <br> <small>string `optional`</small> | Corresponds to the HiPay API environment you want to use. <br> Use `stage` to test your integration and use `production` to make real payments. <br><br> values: `stage`, `production` <br> default: `production` |
| lang <br> <small>string `optional`</small> | Languages to translate placeholders or error messages in Hosted Fields. <br><br> values: `en`, `fr`, `es`, `it`, `de`, `cz`, `pl`, `pt`, `hu` <br> default: `fr` |
| i18n <br> <small>object `optional`</small>| Override the default translations. <br> See the [Reference translations file](#hipay-enterprise-javascript-sdk-reference-reference-translations-file). |

## The HiPay instance


* [hipay.tokenize(params)](#hipay-enterprise-javascript-sdk-reference-the-hipay-instance-hipaytokenizeparams)
* [hipay.updateToken(params)](#hipay-enterprise-javascript-sdk-reference-the-hipay-instance-hipayupdatetokenparams)
* [hipay.getDeviceFingerprint()](#hipay-enterprise-javascript-sdk-reference-the-hipay-instance-hipaygetdevicefingerprint)
* [hipay.injectBaseStylesheet()](#hipay-enterprise-javascript-sdk-reference-the-hipay-instance-hipayinjectbasestylesheet)
* [hipay.removeBaseStylesheet()](#hipay-enterprise-javascript-sdk-reference-the-hipay-instance-hipayremovebasestylesheet)
* [hipay.create(type, options)](#hipay-enterprise-javascript-sdk-reference-the-hipay-instance-hipaycreatetype-options) *Recommended*


### hipay.tokenize(params)

Directly call the tokenization API in order to tokenize credit card information. The function calls the API only if parameters are valid.
You may prefer the `Hosted Fields integration` with [hipay.create()](#hipay-enterprise-javascript-sdk-reference-the-hipay-instance-hipaycreatetype-options).

#### Tokenization request

The tokenize function accepts the following parameters.

| Parameter | Description |
|----------|------------|
| cardHolder <br> <small>string `required`</small>| Cardholder’s name as it appears on the card |
| cardNumber <br> <small>string `required`</small>| Card number, with a 12- to 19-digit length |
| expiryMonth <br> <small>string `required`</small>| Card expiry month, expressed with two digits (e.g.: 01) |
| expiryYear <br> <small>string `required`</small>| Card expiry year, expressed with two digits (e.g.: 22) |
| cvc <br> <small>string</small>| 3- or 4-digit security code (called CVC2, CVV2 or CID depending on the card brand) as it appears on the credit card |
| multiUse <br> <small>boolean `optional`</small>| Indicates if the token should be generated either for a single use or for multiple uses. <br><br> While a single-use token is typically generated for a short time and for processing a single transaction, multi-use tokens are generally generated for recurring payments. |

#### Tokenization response

When successful, the tokenize function returns an object with the following fields.

| Field name   |      Description     |
|----------|------------|
|`token`| Token that was created |
|`request_id`| Request ID linked to the token |
|`brand`| Card brand (e.g.: Visa, Mastercard, American Express, Maestro) |
|`pan`| Card number (up to 19 characters). Please note: due to PCI DSS security standards, our system has to mask credit card numbers in any output (e.g.: 549619******4769).|
|`card_holder`| Cardholder’s name |
|`card_expiry_month`| Card expiry month (2 digits) |
|`card_expiry_year`| Card expiry year (4 digits) |
|`issuer`| Card-issuing bank’s name<br/>Do not rely on this value to remain static over time. Bank names may change due to acquisitions and mergers.|
|`country`| Bank country code where the card was issued. This two-letter country code complies with ISO 3166-1 (alpha 2).|
|`card_type`| Card type (if applicable, e.g.: “DEBIT”, “CREDIT”) |
|`card_category`| Card category (if applicable, e.g.: “PLATINUM”) |
|`payment_product`| HiPay Order API payment product code to pass to the order call |

In case of an error, the function returns an error code describing what went wrong.

### hipay.updateToken(params)

Directly call the tokenization API in order to update the token previously created with [hipay.tokenize()](#hipay-enterprise-javascript-sdk-reference-the-hipay-instance-hipaytokenizeparams).

Please refer to the following documentation for parameters ([update token API reference](/doc-api/enterprise/token/)).

Please read this [integration example](../cvc-forced/) of payment card with CVC forced. This example use updating token function.

### hipay.getDeviceFingerprint()

Get the device fingerprint of the final user in order to send it to the HiPay API.

### hipay.injectBaseStylesheet()

Inject the [base stylesheet](#hipay-enterprise-javascript-sdk-reference-base-stylesheet) in your page dynamically.

### hipay.removeBaseStylesheet()

Remove the [base stylesheet](#hipay-enterprise-javascript-sdk-reference-base-stylesheet) from your page dynamically.


### hipay.create(type, options)

The create function is the entry point of **Hosted Fields** instances.

```js
var card = hipay.create(type, options);
```

Create function is the entry point to create **Hosted Payments**, **Hosted Fields** and **Carousel** instances.

```js
var hostedPaymentInstance = hipay.create('hosted-payments', options);
```
```js
var cardInstance = hipay.create('card', options);
```
This function creates an instance of the payment product specified by `type` (card here).

```js
var carouselInstance = hipay.create('carousel', options);
```
carousel is an Hosted Fields instance

This function creates an instance of the payment product specified by `type`.

#### Types

The type of payment product has to be instantiated as a string.

Please note that each type has required options.

##### Hosted Payments

| Type | Description / [Fields] |
|----------|------------|
| hosted-payments | Creates Hosted Payments instance with carousel and auto-generated payment form |

| Type | Description / [Fields] |
|----------|------------|
| carousel | Creates slideable carousel displaying available payment products |
| card | A credit card accepting multiple brands (mastercard, visa, american-express, maestro, bancontact). <br><br> required fields: [`cardHolder`,`cardNumber`,`expiryDate`,`cvc`]|
| 3xcb | [] (redirection) |
| 3xcb-no-fees | [] (redirection) |
| 4xcb | [] (redirection) |
| 4xcb-no-fees | [] (redirection) |
| aura | [`national_identification_number`] |
| banamex | [`national_identification_number`] |
| banco-do-brasil | [`national_identification_number`] |
| bbva-bancomer | [`national_identification_number`] |
| bcmc-mobile | [] (redirection) |
| belfius | [] (redirection) |
| bnpp-3xcb | [] (redirection) |
| bnpp-4xcb | [] (redirection) |
| boleto-bancario | [`national_identification_number`] |
| bradesco | [`national_identification_number`] |
| caixa | [`national_identification_number`] |
| carte-cadeau | [] (redirection) |
| credit-long | [] (redirection) |
| dexia-directnet | [] (redirection) |
| giropay | [`issuer_bank_id`] |
| ideal | [`issuer_bank_id`] |
| ing-homepay | [] (redirection) |
| itau | [`national_identification_number`] |
| klarnainvoice | [] (redirection) |
| multibanco | [] (redirection) |
| mybank | [] (redirection) |
| oxxo | [`national_identification_number`] |
| paypal | [] (redirection) |
| paysafecard | [] (redirection) |
| postfinance-card | [] (redirection) |
| postfinance-efinance | [] (redirection) |
| przelewy24 | [] (redirection) |
| santander-cash | [`national_identification_number`] |
| santander-home-banking | [`national_identification_number`] |
| sdd | SEPA form <br><br> required fields: [`gender`, `firstname`, `lastname`, `iban`] <br> optional fields: [`bank_name`, `issuer_bank_id`] |
| sisal | [] (redirection) |
| sofort-uberweisung | [] (redirection) |
| webmoney-transfer | [] (redirection) |
| yandex | [] (redirection) |

#### Options

##### Hosted Payments

| Option | Description |
|----------|------------|
| selector <br> <small>string `required`</small>| Unique div `id` to generate the carousel and form in. |
| Hosted Fields type <br> <small>objects `optional`</small>| Override a specific form by adding object named with Hosted Fields type and configure it as if you create an Hosted Fields instance. <br> See options below for Hosted Fields config.|
| styles  <br> <small>object `optional`</small> | Object with your custom styling CSS properties. <br> See the `Styles configuration` section below for more details. |

##### Hosted Fields 

There are two ways to create Hosted Fields configurations:

- using `template`, to generate the HTML form template automatically, 
- using `fields`, to create a custom HTML template.

You cannot use these options together.

| Option | Description |
|----------|------------|
| template <br> <small>string `optional`</small>| If set to `auto`, it activates the generation of the HTML form template automatically. |
| selector <br> <small>string `optional`</small>| Unique div `id` to generate the form when `template: 'auto'` is set. |
| fields  <br> <small>object `optional`</small> | Object with the fields to generate within your form. Each field has its own configuration. <br> See the `Fields configuration` section below for more details. |
| styles  <br> <small>object `optional`</small> | Object with your custom styling CSS properties. <br> See the `Styles configuration` section below for more details. |
| multi_use  <br> <small>boolean `optional`</small> | Only for `card` type. This boolean activates the multi_use option to add the one-click payment feature. |
| brand  <br> <small>array `optional`</small> | Only for `card` type. List of accepted credit card brands. (ex: ['visa', 'mastercard']) |
| payment_product  <br> <small>array `optional`</small> | Only for `carousel` type. The list of payment products to display in the carousel. Ex: ['card', 'sdd', 'paypal']|
| currency  <br> <small>string `optional`</small> | Only for `carousel` type. Base currency to filter carousel payments products by. This three-character currency code complies with ISO 4217. ('EUR', 'USD', 'GBP', ...) |
| country  <br> <small>string `optional`</small> | Only for `carousel` type. The country code of the customer to filter carousel payments product by. This two-letter country code complies with ISO 3166-1 (alpha 2). |

#### Fields configuration

Fields have a common set of options and some field-specific options. Some fields are required when you generate a payment product.

| Option | Description |
|----------|------------|
| selector  <br> <small>string `optional`</small> | Unique div `id` to generate the Hosted Fields. <br> All fields have a default selector `hipay-{payment product}-field-{field name}`. <small>(cardHolder => hipay-card-field-cardHolder)</small> |
| placeholder  <br> <small>string `optional`</small> | Customizes the placeholder text. <br> Be careful, default placeholders are translated according to the lang configuration.   |
| helpButton  <br> <small>boolean `optional`</small> | Adds a clickable help button at the end of the field. An event is triggered on click. <br>For CVC, we also send a generic help message in this event. <br><br> default: `false`    |
| uppercase  <br> <small>boolean `optional`</small><br><small>`only text fields`</small> | Automatically capitalizes all alphabetical cardholder characters.<br><br> default: `true`    |
| defaultFirstname  <br> <small>string `optional`</small><br><small>`only cardHolder`</small> | Needs to be used together with `defaultLastname`. Used to prefill the cardholder field by concatenating defaultFirstname and defaultLastname.    |
| defaultLastname  <br> <small>string `optional`</small><br><small>`only cardHolder`</small> | Needs to be used together with `defaultFirstname`. Used to prefill the cardholder field by concatenating defaultFirstname and defaultLastname.    |
| hideCardTypeLogo  <br> <small>boolean `optional`</small><br><small>`only cardNumber`</small> | Hides the detected credit card type logo. <br><br> default: `false`    |
| defaultValue  <br> <small>boolean `optional`</small><br><small>`except card fields`</small> | Used to prefill a fields with a default value. |

#### Styles configuration

This configuration customizes the field internal appearance. The external appearance depends on your parent CSS file.

Internal styling is divided into `4 optional categories`, each defined with an object.

| Category | Description |
|----------|------------|
| base | Custom base style  |
| valid | CSS applied when the field is valid  |
| invalid | CSS applied when the field is invalid  |
| disable | CSS applied when the field is disabled  |

For each of the above categories, the following properties are available.

| Property | Description |
|----------|------------|
| properties | `color`, `fontSize`, `fontFamily`, `fontStyle`, `fontVariant`, `fontWeight`, `textDecoration`, `iconColor`, `placeholderColor`, `caretColor` |


### Examples 


#### Hosted Payments

```html
<div id="hosted-payments"></div>
```

```js
var options = {
    selector: 'hosted-payments',
    carousel: {
        currency: 'EUR'
    },
    card: {
        multi_use: true
    },
    styles: {
        base: {
            color: "#000000"
        }
    }
}
```

```js
var hostedPaymentsInstance = hipay.create('hosted-payments', options);
```

#### Hosted Fields

The following object provides a configuration example for a credit card form on your page.

```html
<form>
    <div id="card"></div>
</form>
```

```js
var options = {
    template: 'auto',
    selector: 'card',
    multi_use: true,
    fields: {
        cardHolder: {
            uppercase: true,
            placeholder: 'John Doe'
        },
        cvc: {
            helpButton: true
        }
    },
    styles: {
        base: {
          color: "#000000",
          fontSize: "15px",
          fontFamily: "Roboto",
          fontWeight: 400,
          placeholderColor: "#999999",
          iconColor: '#00ADE9',
          caretColor: "#00ADE9"
        },
        invalid: {
          color: '#D50000',
          caretColor: '#D50000'
        }
    }
}
```

```js
var cardInstance = hipay.create('card', options);
```


## Hosted payments instances

Hosted payments instances are created by `hipay.create('hosted-payments', config)`.

* [instance.on(‘event’, callback)](#hipay-enterprise-javascript-sdk-reference-hosted-payments-instances-instanceonevent-callback)
* [instance.getPaymentData()](#hipay-enterprise-javascript-sdk-reference-hosted-payments-instances-instancegetpaymentdata)
* [instance.destroy()](#hipay-enterprise-javascript-sdk-reference-hosted-payments-instances-instancedestroy)

### instance.on('event', callback)

The only way to interact with your Hosted Payments instance is by listening to `events`. The following events are emitted by the Hosted Payments instance.

| Category | Description |
|----------|------------|
| paymentProductChange | Emitted when the payment product changes, i.e. when an item is clicked on the carousel.  <br><br> You can use this event to reset your submit button state. <br><br> Response: `"new_payment_product"` |
| validityChange | Emitted when the payment product validity changes, i.e. when it goes from invalid to valid, and conversely, and when the error changes.  <br><br> You can use this event to enable your submit button when your form is valid. <br><br> This event is always emitted when the form is ready after a `paymentProductChange`. <br><br> Response: `{ payment_product: string, valid: boolean, error_code: 'ERROR_CODE if error', error: translated error }` |

Example for an Hosted Payments instance:

```js
hostedPaymentsInstance.on('paymentProductChange', function(response){ 
  /* Disable submit button */
});

hostedPaymentsInstance.on('validityChange', function(response){ 
  if(!response.valid) {
      /* Disable the submit button and display error */
   } else {
      /* Enable the submit button */
   }
});
```

### instance.getPaymentData()


Use this function to get sensible data from the Hosted Payments form. To process the payment, send those data to [HiPay Order API](/doc-api/enterprise/gateway/#!/payments/requestNewOrder).

```js
hostedPaymentsInstance.getPaymentData().then(
    function(result) {
        /* Send result.datas to your server */
    },
    function(error) {
        /* Display error message */
    }
);
```

When successful, this function returns an object with collected data.

Example for a credit card:
```json
{ 
    "payment_product": "visa",
    "token": "f12bfab3b4fs5q6der7895a98ab76",
    "request_id": "0",
    "brand": "VISA",
    "pan": "411111xxxxxx1111",
    "card_holder": "JOHN DOE",
    "card_expiry_month": "12",
    "card_expiry_year": "2031",
    "issuer": "ANY BANK",
    "country": "US",
    "card_type": "CREDIT",
    "device_fingerprint": "..." 
}
```

When failing, it returns an array containing all errors. Each error contains the field concerned, the error code and the translated message.

Example: 

```json
[
  {
    "error": "Le numéro de la carte est invalide.",
    "error_code": "ERROR_CARD_NUMBER_INVALID",
    "field": "cardNumber"
  },
  {
    "error": "La date d'expiration est invalide.",
    "error_code": "ERROR_EXPIRY_DATE_INVALID",
    "field": "expiryDate"
  },
  {
    "error": "Le cryptogramme est manquant.",
    "error_code": "ERROR_CARD_CVC_MISSING",
    "field": "cvc"
  }
]
```



### instance.destroy()

Use this function to `destroy` properly the Hosted Payments instance.
 
It will remove active event listeners and clean the HTML container. 

```js
hostedPaymentsInstance.destroy();
```


## Payment product instances

Payment product instances are created by [hipay.create()](#hipay-enterprise-javascript-sdk-reference-the-hipay-instance-hipaycreatetype-options).

* [instance.on(‘event’, callback)](#hipay-enterprise-javascript-sdk-reference-payment-product-instances-instanceonevent-callback)
* [instance.createToken() [DEPRECATED]](#hipay-enterprise-javascript-sdk-reference-payment-product-instances-instancecreatetoken-deprecated)
* [instance.getPaymentData()](#hipay-enterprise-javascript-sdk-reference-payment-product-instances-instancegetpaymentdata)
* [instance.destroy()](#hipay-enterprise-javascript-sdk-reference-payment-product-instances-instancedestroy)


### instance.on('event', callback)

The only way to interact with your Hosted Fields form is by listening to `events`. The following events are emitted by the payment product instance.

| Category | Description |
|----------|------------|
| ready | Emitted when all fields are fully loaded. <br><br> Response: `None` |
| change | Emitted when the payment product validity changes, i.e. when it goes from invalid to valid, and conversely, and when the error changes.  <br><br> You can use this event to enable your submit button when your form is valid. <br><br> Response: `{ valid: boolean, error_code: 'ERROR_CODE if error', error: translated error }` |
| helpButtonToggled | Emitted when a help button is clicked inside a field. <br><br> Response: `{ element: 'field name clicked', message: 'help message for CVC' } `  |
| cardTypeChange | Emitted when the card type detected from the card number changes. <br><br> Response: `cardType as string`  |
| focus | Emitted when an input gains focus. <br><br> Response: `{ element:'input element',  validity: object }` The validity object contains booleans (focused, empty, disable, valid, potentiallyValid) and error(s), if any.  |
| blur | Emitted when an input loses focus. <br><br> Response: `{ element:'input element',  validity: object }` The validity object contains booleans (focused, empty, disable, valid, potentiallyValid) and error(s), if any.  |
| inputChange | Emitted when an input validity changes. <br><br> Response: `{ element:'input element',  validity: object }` The validity object contains booleans (focused, empty, disable, valid, potentiallyValid) and error(s), if any.  |

Example for a credit card instance:


```js
card.on('change', function(response){ 
 if(!response.valid) {
    /* Disable the pay button and display error */
 } else {
    /* Enable the pay button */
 }
});
```

### instance.createToken() [DEPRECATED]

This function has been deprecated. Please use [instance.getPaymentData()](#hipay-enterprise-javascript-sdk-reference-payment-product-instances-instancegetpaymentdata) instead.

### instance.getPaymentData() 

Use this function to get data from the Hosted Fields iFrames. These data can be securely sent to your server to call the [HiPay Order API](/doc-api/enterprise/gateway/#!/payments/requestNewOrder).

When successful, it returns the same object as [hipay.tokenize()](#hipay-enterprise-javascript-sdk-reference-the-hipay-instance-hipaytokenizeparams) for cards and returns an object with field data for other types.
<br>In addition, it always returns the payment product and the device fingerprint inside this object.

In case of error(s), it returns the list of error(s).

Example for a credit card Hosted Fields instance.

```js
card.getPaymentData().then(
    function(result) {
        /* Send result to your server */
    },
    function(error) {
        /* Display error message */
    }
);
```

Result
```json
{
  "payment_product": "visa",
  "token": "f39bfaxxxxxxxxxxxxxxxxxxxa12345a67ab89",
  "request_id": "0",
  "brand": "VISA",
  "pan": "411111xxxxxx1111",
  "card_holder": "JOHN DOE",
  "card_expiry_month": "12",
  "card_expiry_year": "2021",
  "issuer": "ANY BANK",
  "country": "US",
  "card_type": "CREDIT",
  "device_fingerprint": "..."
}
```

Error
```json
[
  {
    "field": "cardNumber",
    "error": "Card number is missing",
    "error_code": "ERROR_CARD_NUMBER_MISSING"
  },
  {
      "field": "cvc",
      "error": "CVC is invalid",
      "error_code": "ERROR_CARD_CVC_INVALID"
    }
]
```

### instance.destroy()

Use this function to properly `destroy` Hosted Fields instances.
 
It will remove active event listeners and clean iFrame containers. If you generated your form automatically via `template: 'auto'` configuration, it will clean the container.

```js
card.destroy();
```


## Hosted Fields container

With the Hosted Fields integration, customization is entirely up to you. Thus, you are in control of the fields' external layout (`border`, `background`, etc.).
This customization is done by styling the `div` parent container of fields as if it were the `input`.

Hosted Fields adapt their width and height from the parent container. You need to **explicitly specify a `height`** to your container.

The example below adds a 1px bottom margin to your fields and sets their height to 24px.


```html
<form>
    <div class="hostedfield" id="custom-card-holder"></div>
    <div class="hostedfield" id="custom-card-number"></div>
    <div class="hostedfield" id="custom-expiry-date"></div>
    <div class="hostedfield" id="custom-cvc"></div>
</form>
```

```css
.hostedfield{
    height: 24px;
    border-bottom: solid 1px black;
}
```

To help you with styling, the following classes are toggled on `div` containers according to the internal state of fields.

| Category | Description |
|----------|------------|
| HiPayField--empty | Applied when the field has an empty value |
| HiPayField--focused | Applied when the field is currently focused |
| HiPayField--valid | Applied when the field value is valid |
| HiPayField--invalid | Applied when the field value is invalid (and not empty) |

In your stylesheet, you can add rules like:


```css
.hostedfield.HiPayField--valid{
    border-bottom: solid 1px green;
}
.hostedfield.HiPayField--invalid{
    border-bottom: solid 1px red;
}
```


## Base stylesheet

Base stylesheet refers to the default CSS stylesheet loaded when you use Hosted Payments or Hosted Fields with `template: 'auto'`.

This stylesheet is automatically added in the `<head>` of your HTML page, but you can include it manually to increase loading performance.

```html
<link href="https://libs.hipay.com/themes/material.css" rel="stylesheet" />
```

You can override necessary classes with your own CSS stylesheet to customize your forms.


## Reference translations file

You can override default translations with the following keys.

```json
{
  "carousel-empty": "No available payment product",
  "placeholder-cardHolder": "Firstname Lastname",
  "placeholder-cardNumber": "1111 1111 1111 1111",
  "placeholder-expiryDate": "05 / 24",
  "placeholder-issuer_bank_id": "ABCDEFAA123",
  "placeholder-iban": "GB00 AAAA 0000 0000 0000 00",
  "placeholder-firstname": "Firstname",
  "placeholder-lastname": "Lastname",
  "placeholder-bank_name": "Bank name",
  "placeholder-cpf": "111.222.333-44",
  "placeholder-curp": "AABB801118MBSYAA11",
  "placeholder-company": "Company",
  "cvc-message": "The card verification code (CVC) is a 3 digits security code usually placed at the back of the credit card. For American Express cards, it is a 4 digits code at the front of the credit card.",
  "ERROR_CARD_HOLDER_ALPHANUMERICAL": "Card holder should be alphanumerical.",
  "ERROR_CARD_HOLDER_MAX_LENGTH": "Card holder maximum length is exceeded.",
  "ERROR_CARD_HOLDER_MAX_DIGITS": "Card holder should not contain more than 8 digits.",
  "ERROR_CARD_HOLDER_INVALID": "Card holder is invalid.",
  "ERROR_CARD_NUMBER_INVALID": "Card number is invalid.",
  "ERROR_CARD_BRAND_NOT_ALLOWED": "Card type is not allowed.",
  "ERROR_EXPIRY_DATE_INVALID_FORMAT": "Expiry date format is invalid.",
  "ERROR_EXPIRY_DATE_PAST_DATE": "Expiry date can't be in the past.",
  "ERROR_EXPIRY_DATE_INVALID": "Expiry date is invalid.",
  "ERROR_EXPIRY_MONTH_INVALID_FORMAT": "Expiry month format is invalid.",
  "ERROR_EXPIRY_MONTH_LENGTH_MAX": "Expiry month maximum length is exceeded.",
  "ERROR_EXPIRY_MONTH_INVALID": "Expiry month is invalid.",
  "ERROR_EXPIRY_YEAR_INVALID_FORMAT": "Expiry year format is invalid.",
  "ERROR_EXPIRY_YEAR_LENGTH_MAX": "Expiry year max length is exceeded.",
  "ERROR_EXPIRY_YEAR_PAST_DATE": "Expiry year can't be in the past.",
  "ERROR_EXPIRY_YEAR_INVALID": "Expiry year is invalid.",
  "ERROR_CARD_CVC_FORMAT": "CVC format is invalid.",
  "ERROR_CARD_CVC_MAX_LENGTH": "CVC max length is exceeded.",
  "ERROR_CARD_CVC_INVALID": "CVC is invalid.",
  "ERROR_CARD_HOLDER_MISSING": "Card holder is missing.",
  "ERROR_CARD_NUMBER_MISSING": "Card number is missing.",
  "ERROR_EXPIRY_DATE_MISSING": "Expiry date is missing.",
  "ERROR_CARD_CVC_MISSING": "CVC is missing.",
  "ERROR_CARD_TOKENIZATION": "An error occurred during tokenization.",
  "ERROR_BIC_FORMAT": "BIC format is invalid.",
  "ERROR_BIC_MAX_LENGTH": "BIC maximum length is exceeded.",
  "ERROR_BIC_INVALID": "BIC is invalid.",
  "ERROR_IBAN_FORMAT": "IBAN format is invalid.",
  "ERROR_IBAN_NOT_ALPHANUMERICAL": "IBAN should be alphanumerical.",
  "ERROR_IBAN_INVALID": "IBAN is invalid.",
  "ERROR_IBAN_INVALID_COUNTRY_CODE": "IBAN country code is invalid.",
  "ERROR_IBAN_MAX_LENGTH": "IBAN maximum length is exceeded.",
  "label-cardHolder": "Fullname",
  "label-cardNumber": "Card number",
  "label-expiryDate": "Expiry date",
  "label-cvc": "CVC",
  "label-issuer_bank_id": "BIC",
  "label-iban": "IBAN",
  "label-firstname": "Firstname",
  "label-lastname": "Lastname",
  "label-bank_name": "Bank name",
  "label-gender": "Gender",
  "label-cpf": "CPF",
  "label-curp": "CURP/CPN",
  "label-gender-M": "Male",
  "label-gender-F": "Female",
  "label-gender-U": "Unknown",
  "label-client_type": "Client type",
  "label-client_type-B2B": "Business",
  "label-client_type-B2C": "Individual",
  "label-company": "Company",
  "label-company_type": "Company type",
  "label-company_type-SA": "SA",
  "label-company_type-SAS": "SAS",
  "label-company_type-SARL": "SARL",
  "label-company_type-EURL": "EURL",
  "label-company_type-SELARL": "SELARL",
  "label-company_type-SASU": "SASU",
  "label-company_type-SNC": "SNC",
  "label-company_type-SCP": "SCP",
  "label-company_type-EI": "EI",
  "label-company_type-EIRL": "EIRL",
  "ERROR_TEXT_FIELD_INVALID": "Text field is invalid.",
  "ERROR_TEXT_FIELD_INVALID_CHARACTERS": "Text field contains invalid characters.",
  "ERROR_TEXT_FIELD_MAX_LENGTH": "Text field maximum length is exceeded.",
  "ERROR_TEXT_FIELD_MAX_DIGITS": "Text field should not contain more than 8 digits.",
  "ERROR_CPF_INVALID": "CPF is invalid.",
  "ERROR_CPF_NUMERICAL": "CPF should be numerical",
  "ERROR_CPF_MAX_LENGTH": "CPF maximum length is exceeded.",
  "ERROR_CURP_INVALID": "CURP is invalid.",
  "ERROR_CURP_ALPHANUMERICAL": "CURP should be alphanumerical",
  "ERROR_CURP_MAX_LENGTH": "CURP maximum length is exceeded.",
  "payment-means-redirection": "You will be redirected to an external payment page in order to finalize the transaction.",
  "ERROR_BANK_NAME_MISSING": "Bank name is missing.",
  "ERROR_FIRSTNAME_MISSING": "Firstname is missing.",
  "ERROR_LASTNAME_MISSING": "Lastname is missing.",
  "ERROR_COMPANY_MISSING": "Company name is missing.",
  "ERROR_IBAN_MISSING": "IBAN is missing.",
  "ERROR_ISSUER_BANK_ID_MISSING": "BIC is missing.",
  "ERROR_NATIONAL_IDENTIFICATION_NUMBER_MISSING": "National identification number is missing.",
  "ERROR_OCCURED": "An error occured."
}

```