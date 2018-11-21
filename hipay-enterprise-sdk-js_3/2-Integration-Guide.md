# HiPay hosted fields integration guide

This guide will walk you through the creation of a payment form using HiPay hosted fields.

You will see how to create your payment form, customize it, interact with it, and securely get a `token` generated from sensitive card information. This token will enable you to make a transaction using the HiPay API.

Please follow these 6 steps to create your payment form with HiPay hosted fields:

1. [Set up the JavaScript SDK](#hipay-hostedfields-integration-guide-1-set-up-the-javascript-sdk)
2. [Set up your HTML form](#hipay-hostedfields-integration-guide-2-set-up-your-html-form)
3. [Create the payment product instance](#hipay-hostedfields-integration-guide-3-create-the-payment-product-instance)
4. [Customize your payment form](#hipay-hostedfields-integration-guide-4-style-your-payment-form)
5. [Interact with the form](#hipay-hostedfields-integration-guide-5-interact-with-the-form)
6. [Tokenize card information](#hipay-hostedfields-integration-guide-6-tokenize-your-card)
  
## 1 - Set up the JavaScript SDK

To get started, include the JavaScript SDK on your HTML page. 

The following link exposes a global variable, `HiPay`, as a function to init the SDK with your public credentials and your configuration.

```html
<script type="text/javascript" src="https://libs.hipay.com/js/sdkjs.js"></script>
```

Then, create an instance of the JavaScript SDK. 
<br>
You may replace `HIPAY-PUBLIC-LOGIN` and `HIPAY-PUBLIC-PASSWORD` with your own public credentials.

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

[Including HiPay SDK full documentation](#hipay-sdk-js-reference-including-hipay-sdk)

## 2 - Set up your HTML form

To collect sensitive information securely, the SDK will generate components hosted by HiPay on your page. 
<br>
With these components, sensitive information filled in by customers never hits your page or your web server.

You can now integrate your HTML page. You simply have to replace `ìnput` elements by `div` elements. These elements need to have a unique `id`.

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

In this example, our SDK will generate a hosted field in the `hipay-card-holder`, `hipay-card-number`, `hipay-expiry-date`, `hipay-cvc` divs.

## 3 - Create the payment product instance

Now that the HTML form is ready, we can generate fields inside. To do so, you need to create an instance of a credit card with the SDK instance previously initialized.

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
                selector: 'hipay-date-expiry' // expiry date div id
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

Please refer to the [hipay.create()](#hipay-sdk-js-reference-the-hipay-instance-hipaycreatetype-options) full documentation to see all supported configurations.


## 4 - Customize your payment form

There are two ways to customize the hosted fields integration, as we separate `internal` styling from `container` styling.

#### Container styling

To best match your style guides, the `external` styling (`height`, `border`, `background`, etc.) of the field completely depends on your cascading style sheets.

To help you with this integration, the following classes have been added to the container field:

* `HiPayField--empty `
* `HiPayField--focused `
* `HiPayField--valid `
* `HiPayField--invalid `

[Container documentation](#hipay-sdk-js-reference-hostedfield-container)

#### Internal styling

Internal styling refers to the properties inside generated fields, like text or icon properties. 

These CSS properties are set during Step 3. Let's now add styles to our previous configuration.

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
                selector: 'hipay-date-expiry' // expiry date div id
            },
            cvc: {
                selector: 'hipay-cvc', // cvc div id
                helpButton: true // activate the help button
            }
        }
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

[Internal styling documentation](#hipay-sdk-js-reference-the-hipay-instance-hipaycreatetype-options)


## 5 - Interact with the form

You can interact with the card instance previously initiated by listening to events on it.

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

[Events documentation](#hipay-sdk-js-reference-the-payment-product-instance-instanceonevent-callback)

## 6 - Tokenize card information

To securely transmit sensitive information, the HiPay hosted fields convert it into a token to be sent to the API to process your payment. With this token, you will never be able to retrieve the card information.

You can tokenize the information of the card you initiated previously by calling the `createToken()` function. 
<br>
The best way to tokenize is to add an event listener on the `submit` event of your form.

The `card.createToken()` function returns a `Promise`. When successful, it returns an object with the token. When rejected, it returns the error as String.
 
 ```html
<script>
    /* Get form */
    let cardForm = document.getElementById("hipay-form");
    
    /* Add event listener on submit button clicked */
    cardForm.addEventListener("submit", function(event) {
      event.preventDefault();
      /* Tokenize your card information when the submit button is clicked */
      cardInstance.createToken().then(
        function(response) {
          /* Send token to your server to process payment */
          handlePayment(response.token);
        },
        function(error) {
          /* Display error */
          document.getElementById("hipay-error-message").innerHTML = error;
        }
      );
    });
</script>
```

Here is the response from getToken():

```json
{ 
     "token": "f39bfab2b6c96fa30dcc0e55aa3da4125a49ab03", 
     "request_id": "0", 
     "card_hash": "f39bfab2b6c96fa30dcc0e55aa3da4125a49ab03", 
     "brand": "VISA", 
     "pan": "411111xxxxxx1111", 
     "card_holder": "AZE", 
     "card_expiry_month": "12", 
     "card_expiry_year": "2021", 
     "issuer": "JPMORGAN CHASE BANK, N.A.", 
     "country": "US", 
     "card_type": "CREDIT" 
}
```

[Create Token documentation](#hipay-sdk-js-reference-the-payment-product-instance-instancecreatetoken)
