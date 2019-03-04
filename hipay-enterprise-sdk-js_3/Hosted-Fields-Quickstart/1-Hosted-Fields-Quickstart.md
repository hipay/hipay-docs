# HiPay Hosted Fields quickstart


This guide will walk you through the creation of a credit card payment form using the HiPay Hosted Fields.

You will see how to create your payment form, customize it, interact with it, and securely get a `token` generated from sensitive card information. This token will enable you to make a transaction using the [HiPay Order API](/doc-api/enterprise/gateway/#!/payments/requestNewOrder).

This guide show you the 2 integration modes:
- Auto-generated form with material design
- Custom form

Please follow these 6 steps to create your payment form with the HiPay Hosted Fields:


1. [Set up the HiPay Enterprise JavaScript SDK](#hipay-hosted-fields-integration-guide-1-set-up-the-hipay-enterprise-javascript-sdk)
2. [Set up your HTML form](#hipay-hosted-fields-integration-guide-2-set-up-your-html-form)
3. [Create the payment product instance](#hipay-hosted-fields-integration-guide-3-create-the-payment-product-instance)
4. [Customize your payment form](#hipay-hosted-fields-integration-guide-4-customize-your-payment-form)
5. [Interact with the form](#hipay-hosted-fields-integration-guide-5-interact-with-the-form)
6. [Tokenize card information](#hipay-hosted-fields-integration-guide-6-tokenize-card-information)
  
## 1 - Set up the HiPay Enterprise JavaScript SDK

To get started, include the HiPay Enterprise JavaScript SDK on your HTML page. 

The following link exposes a global variable, `HiPay`, as a function to initialize the SDK with your public credentials and your configuration.

```html
<script type="text/javascript" src="https://libs.hipay.com/js/sdkjs.js"></script>
```

Then, create an instance of the HiPay Enterprise JavaScript SDK. 
<br>
You must replace `HIPAY-PUBLIC-LOGIN` and `HIPAY-PUBLIC-PASSWORD` with your own public credentials.

```html
<script>
    var hipay = HiPay({
       username: 'HIPAY-PUBLIC-LOGIN',
       password: 'HIPAY-PUBLIC-PASSWORD',
       environment: 'production',
       lang: 'en'
    });
</script>
```

Go to: [HiPay Enterprise JavaScript SDK reference](../Reference/#hipay-enterprise-javascript-sdk-reference-including-the-hipay-enterprise-javascript-sdk)

## 2 - Set up your HTML form

To collect sensitive information securely, the HiPay Enterprise JavaScript SDK will generate components hosted by HiPay on your page. 
<br>
With these components, sensitive information filled in by customers never hits your page or your web server.

You can now integrate your HTML page. 

### Mode auto

You simply have to place a `div` with a unique `id` in your page. The form will be injected inside automatically.


```html
<form id="hipay-form">
    <div id="hipay-hostedfields-form">
        /* All the form will be inserted here */
    </div>
    
    <button type="submit" id="hipay-submit-button" disabled="true">
        PAY
    </button>
    
    <div id="hipay-error-message"></div>
</form>
```

### Mode custom
 
You simply have to replace `ìnput` elements by `div` elements. These elements need to have a unique `id`.

```html
<form id="hipay-form">
    <label>Fullname</label>
    <div class="hostedfield" id="hipay-card-holder">
        /* Cardholder field will be inserted here */
    </div>
   
    <label>Card number</label>
    <div class="hostedfield" id="hipay-card-number">
        /* Card number field will be inserted here */
    </div>
    
    <label>Expiry date</label>
    <div class="hostedfield" id="hipay-expiry-date">
        /* Expiry date field will be inserted here */
    </div>
    
    
    <label>CVC</label>
    <div class="hostedfield" id="hipay-cvc">
        /* CVC field will be inserted here */
    </div>
    
    <button type="submit" id="hipay-submit-button" disabled="true">
        PAY
    </button>
    
    <div id="hipay-error-message"></div>
</form>
```

In this example, the HiPay Enterprise JavaScript SDK will generate a hosted field in the `hipay-card-holder`, `hipay-card-number`, `hipay-expiry-date`, `hipay-cvc` divs.

## 3 - Create the payment product instance

### Mode auto

Now that the HTML div is ready, we can generate all the fields with their label inside.
```html
<script>
    var config = {
        selector: 'hipay-hostedfields-form' // form container div id
    };
    
    var cardInstance = hipay.create('card', config);
</script>
```

### Mode custom

Now that the HTML form is ready, we can generate fields inside. To do so, you need to create an instance of a credit card with the HiPay Enterprise JavaScript SDK instance previously initialized.

```html
<script>
    var config = {
        fields: {
            cardHolder: {
                selector: 'hipay-card-holder' // card holder div id
            },
            cardNumber: {
                selector: 'hipay-card-number' // card number div id
            },
            expiryDate: {
                selector: 'hipay-expiry-date' // expiry date div id
            },
            cvc: {
                selector: 'hipay-cvc', // cvc div id
                helpButton: true // activate the help button
            }
        }
    };
    
    var cardInstance = hipay.create('card', config);
</script>
```

Go to: [hipay.create(type, options)](../Reference/#hipay-enterprise-javascript-sdk-reference-the-hipay-instance-hipaycreatetype-options) to see all supported configurations.


## 4 - Customize your payment form

There are two ways to customize the Hosted Fields integration, as we separate `internal` styling from `container` styling.

#### Container styling

To best match your style guides, the `external` styling (`height`, `border`, `background`, etc.) of the field completely depends on your cascading style sheets.

To help you with this integration, the following classes have been added to the container field:

* `HiPayField--empty `
* `HiPayField--focused `
* `HiPayField--valid `
* `HiPayField--invalid `

Go to: [Hosted Fields container](../Reference/#hipay-enterprise-javascript-sdk-reference-hosted-fields-container)

#### Internal styling

Internal styling refers to the properties inside generated fields, like text or icon properties. 

These CSS properties are set during Step 3. Let's now add styles to our previous configuration.

```html
<script>
    var config = {
        selector: 'hipay-hostedfields-form',
        styles: {
            base: { // default field styling
              color: "#000000",
              fontSize: "15px",
              fontWeight: 400,
              placeholderColor: "#999999",
              iconColor: '#00ADE9',
              caretColor: "#00ADE9"
            },
            invalid: { // invalid field styling
              color: '#D50000',
              caretColor: '#D50000'
            }
        }
    };

    var cardInstance = hipay.create('card', config);
</script>
```

Go to: [Styles configuration](../Reference/#hipay-enterprise-javascript-sdk-reference-the-hipay-instance-hipaycreatetype-options)


## 5 - Interact with the form

You can interact with the card instance previously initialized by listening to events on it.

Here is how to enable your submit button when your form is valid and show the error(s), if not.

```html
<script>
    /* Listen to change event on card instance */
    cardInstance.on('change', function(event){
        /* Display error(s), if any */
        document.getElementById("hipay-error-message").innerHTML = event.error;
        /* Enable / disable submit button */
        document.getElementById("hipay-submit-button").disabled = !event.valid;
    });
</script>
```

Go to: [instance.on(‘event’, callback)](../Reference/#hipay-enterprise-javascript-sdk-reference-payment-product-instances-instanceonevent-callback)

## 6 - Tokenize card information

To securely transmit sensitive information, the HiPay Hosted Fields convert it into a token to be sent to the [HiPay Order API](/doc-api/enterprise/gateway/#!/payments/requestNewOrder) to process your payment. With this token, you will never be able to retrieve the card information.

You can tokenize the information of the card you initialized previously by calling the `getPaymentData()` function. 
<br>
The best way to tokenize is to add an event listener on the `submit` event of your form.

The `card.getPaymentData()` function returns a `Promise`. When successful, it returns an object with the token and all the payment data. When rejected, it returns the list of errors.
 
 ```html
<script>
    /* Get form */
    let cardForm = document.getElementById("hipay-form");
    
    /* Add event listener on the submit button when clicked */
    cardForm.addEventListener("submit", function(event) {
      event.preventDefault();
      /* Tokenize your card information when the submit button is clicked */
      cardInstance.getPaymentData().then(
        function(response) {
          /* Send token to your server to process payment */
          handlePayment(response.token);
        },
        function(errors) {
          /* Display first error */
          document.getElementById("hipay-error-message").innerHTML = errors[0].error;
        }
      );
    });
</script>
```

Here is the response from getToken():

```json
{ 
     "token": "a12bcab3d4d56ef78abc0d99aa1bc4321a56ab78", 
     "request_id": "0", 
     "card_hash": "a12bcab3d4d56ef78abc0d99aa1bc4321a56ab78", 
     "brand": "VISA", 
     "pan": "411111xxxxxx1111", 
     "card_holder": "DOE", 
     "card_expiry_month": "12", 
     "card_expiry_year": "2021", 
     "issuer": "ANY BANK", 
     "country": "US", 
     "card_type": "CREDIT",
     "device_fingerprint": "..." 
}
```

Go to: [instance.getPaymentData()](../Reference/#hipay-enterprise-javascript-sdk-reference-payment-product-instances-instancegetpaymentdata)