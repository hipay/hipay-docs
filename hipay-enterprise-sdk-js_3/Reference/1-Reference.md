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
<link href="https://libs.hipay.com/css/base-stylesheet.css" rel="stylesheet" />
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
| lang <br> <small>string `optional`</small> | Languages to translate placeholders or error messages in Hosted Fields. <br><br> values: `en`, `fr`, `es`, `it`, `de`, `cz`, `pl`, `pt` <br> default: `fr` |

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

This function creates an instance of the payment product specified by `type`.

#### Types

The type of payment product has to be instantiated as a string.

Please note that each type has required options.

| Type | Description / [Fields] |
|----------|------------|
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
| bnpp-3xb | [] (redirection) |
| bnpp-4xb | [] (redirection) |
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
| oxxo | [`national_identification_number`] |
| paypal | [] (redirection) |
| paysafecard | [] (redirection) |
| postfinance-card | [] (redirection) |
| postfinance-efinance | [] (redirection) |
| przelewy24 | [] (redirection) |
| santander-cash | [`national_identification_number`] |
| santander-home-banking | [`national_identification_number`] |
| sdd | SEPA form <br><br> required fields: [`gender`, `firstname`, `lastname`, `iban`, `issuer_bank_id`] <br> optional fields: [`bank_name`] |
| sisal | [] (redirection) |
| sofort-uberweisung | [] (redirection) |
| webmoney-transfer | [] (redirection) |
| yandex | [] (redirection) |

#### Options

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
| defaultText  <br> <small>string `optional`</small><br><small>`only text fields`</small> | Prefills the text field with the value. <small>(Uses defaultFirstName and defaultLastName for cardHolder.)</small>    |
| hideCardTypeLogo  <br> <small>boolean `optional`</small><br><small>`only cardNumber`</small> | Hides the detected credit card type logo. <br><br> default: `false`    |

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


### Example 

The following object provides a configuration example for a credit card form on your page.


```html
<form>
    <div id="custom-card-holder">
        /* Cardholder will be injected here */
    </div>
    <div id="custom-card-number">
        /* Card number will be injected here */
    </div>
    <div id="custom-expiry-date">
        /* Expiry date will be injected here */
    </div>
    <div id="custom-cvc">
        /* CVC will be injected here */
    </div>
</form>
```

```js
var options = {
    fields: {
        cardHolder: {
            selector: 'custom-card-holder',
            uppercase: true,
            placeholder: 'John Doe'
        },
        cardNumber: {
            selector: 'custom-card-number'
        },
        expiryDate: {
            selector: 'custom-expiry-date'
        },
        cvc: {
            selector: 'custom-cvc',
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
    },
    multi_use: true
}
```

```js
var card = hipay.create('card', options);
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
<link href="https://libs.hipay.com/shared/base-stylesheet.css" rel="stylesheet" />
```

You can override necessary classes with your own CSS stylesheet to customize your forms.
