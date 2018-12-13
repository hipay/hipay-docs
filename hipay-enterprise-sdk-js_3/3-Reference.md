# HiPay Enterprise JavaScript SDK reference

This is the API reference for the HiPay Enterprise JavaScript SDK in order to collect and tokenize customers' sensitive information securely. 

## Including the HiPay Enterprise JavaScript SDK

The first step to use the HiPay Enterprise JavaScript SDK is to include it on your page.


```html
<script src="https://libs.hipay.com/js/sdkjs.js"></script>
```

The SDK is exposed as a function in a global variable named `HiPay`.

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
| lang <br> <small>string `optional`</small> | Languages to translate placeholders or error messages in Hosted Fields. <br><br> values: `en`, `fr`, `es`, `it`, `cz`, `pl`, `pt` <br> default: `en` |

## The HiPay instance


* [hipay.tokenize(params)](#hipay-sdk-js-reference-the-hipay-instance-hipaytokenizeparams)
* [hipay.create(type, options)](#hipay-sdk-js-reference-the-hipay-instance-hipaycreatetype-options) *Recommended*


### hipay.tokenize(params)

Directly call the tokenization API in order to tokenize credit card information. The function calls the API only if parameters are valid.
You may prefer the `Hosted Fields integration` with [hipay.create()](#hipay-sdk-js-reference-the-hipay-instance-hipaycreatetype-options).

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


### hipay.create(type, options)

Create a **HiPay Hosted fields** instance.


```js
var card = hipay.create('card', options);
```

This function creates an instance of the payment product specified by `type`.

#### Types

The type of payment product has to be instantiated as a string.

Please note that each type has required fields to configure in options.

| Type | Description |
|----------|------------|
| card | A credit card accepting multiple brands (mastercard, visa, american-express, maestro, bancontact). <br><br> required fields: [`cardHolder`,`cardNumber`,`expiryDate`,`cvc`]|

#### Options

All types require a list of `fields` with a configuration. Other parameters are optional.

| Option | Description |
|----------|------------|
| fields  <br> <small>object `required`</small> | Object with the fields to generate into your form. Each field has its own configuration. <br> See the `Fields configuration` section below for more details. |
| styles  <br> <small>object `optional`</small> | Object with your custom styling CSS properties. <br> See the `Styles configuration` section below for more details. |
| multi_use  <br> <small>boolean `optional`</small> | Only for `card` type. This boolean activate the multi_use option to add the one-click payment feature. |

#### Fields configuration

Fields have a common set of options and some field-specific options. Some fields are required when you generate a payment product.

| Option | Description |
|----------|------------|
| selector  <br> <small>string `optional`</small> | Unique div `id` to generate the hosted field. <br> All fields have a default selector `hipay-{PAYMENT_PRODUCT}-{field}` in snake case. <small>(cardHolder => hipay-card-card-holder)</small> |
| placeholder  <br> <small>string `optional`</small> | Customizes the placeholder text. <br> Be careful, default placeholders are translated according to the lang configuration.   |
| helpButton  <br> <small>boolean `optional`</small> | Adds a clickable help button at the end of field. An event is triggered on click. <br>For CVC, we also send a generic help message in this event. <br><br> default: `false`    |
| uppercase  <br> <small>boolean `optional`</small><br><small>`only cardHolder`</small> | Automatically capitalizes all alphabetical cardholder characters<br><br> default: `true`    |
| defaultFirstname  <br> <small>string `optional`</small><br><small>`only cardHolder`</small> | Needs to be used together with `defaultLastname`. Used to prefill the cardholder field by concatenating defaultFirstname and defaultLastname.    |
| defaultLastname  <br> <small>string `optional`</small><br><small>`only cardHolder`</small> | Needs to be used together with `defaultFirstname`. Used to prefill the cardholder field by concatenate defaultFirstname and defaultLastname.    |
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

The following object provides a configuration example for a credit card with this form on your page.


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

Payment product instances are created by hipay.create().

* [instance.on(‘event’, callback)](#hipay-sdk-js-reference-the-payment-product-instance-instanceonevent-callback)
* [instance.createToken()](#hipay-sdk-js-reference-the-payment-product-instance-instancecreatetoken)

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

### instance.createToken()

Use this function to get a token generated from the Hosted Fields values. This token can be securely sent to your server to call the [HiPay Order API](https://developer.hipay.com/doc-api/enterprise/gateway/#!/payments/requestNewOrder).

When successful, it returns the same object as [hipay.tokenize()](#hipay-sdk-js-reference-the-hipay-instance-hipaytokenizeparams).
<br>
In case of an error, it returns a `translated error message` as text <small>(e.g.: 'Card number is missing').</small>


```js
card.createToken().then(
    function(result) {
        /* Send result.token to your server */
    },
    function(error) {
        /* Display error message */
    }
);
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

In your style sheet, you can add rules like:


```css
.hostedfield.HiPayField--valid{
    border-bottom: solid 1px green;
}
.hostedfield.HiPayField--invalid{
    border-bottom: solid 1px red;
}
```
