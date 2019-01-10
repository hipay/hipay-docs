# HiPay Hostedfields integration guide

This guide walks through the creation of a payment form using the HiPay Hostedfields feature.

This feature helps you to create your form, customized it, interact with it, and get securely a `token` generated from sensible card information. You will be able to make a transaction with this token on the HiPay API.

The creation of your payment form with HiPay Hostedfields is divided in 6 steps:

1. [Set up the Javascript SDK](#hipay-hostedfields-integration-guide-1-set-up-the-javascript-sdk)
2. [Set up your HTML form](#hipay-hostedfields-integration-guide-2-set-up-your-html-form)
3. [Create the payment product instance](#hipay-hostedfields-integration-guide-3-create-the-payment-product-instance)
4. [Style your payment form](#hipay-hostedfields-integration-guide-4-style-your-payment-form)
5. [Interact with the form](#hipay-hostedfields-integration-guide-5-interact-with-the-form)
6. [Tokenize your card](#hipay-hostedfields-integration-guide-6-tokenize-your-card)
  
## 1 - Set up the Javascript SDK

To get started, include the Javascript SDK in your HTML page. 

This link exposes a global variable `HiPay` as a function to init the SDK with your public credentials and your configuration.

```html
<script type="text/javascript" src="https://libs.hipay.com/js/sdkjs.js"></script>
```

Next, create an instance of the Javascript SDK. 
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

In order to collect securely sensible information, the SDK will generate components hosted by HiPay in your page. 
<br>
With this components, the sensible information filled by the customer never pass through your page or your web server.

You can now integrate your HTML page. You simply have to replace `ìnput` elements by `div` elements. Those elements needs to have a unique `id`.

```html
<form id="hipay-form">
    <label>Fullname</label>
    <div class="hostedfield" id="hipay-card-holder">
        /* Card Holder field will be inserted here */
    </div>
   
    <label>Card number</label>
    <div class="hostedfield" id="hipay-card-number">
        /* Card Number field will be inserted here */
    </div>
    
    <label>Expiry date</label>
    <div class="hostedfield" id="hipay-expiry-date">
        /* Expiry Date field will be inserted here */
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

In this example, our SDK will generate an hosted field in the `hipay-card-holder`, `hipay-card-number`, `hipay-expiry-date`, `hipay-cvc` divs.

## 3 - Create the payment product instance

Now that the HTML form is ready, we can generate fields inside. To do that, you need to create a instance of a credit card with the previously initialized SDK instance.

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

See the [hipay.create()](#hipay-sdk-js-reference-the-hipay-instance-hipaycreatetype-options) full documentation to see all supported configurations.


## 4 - Style your payment form

The hosted fields integration can be styled in two ways. We separate the `internal` styling with the `container` styling.

#### Container styling

In order to match the maximum your style guides, the `external` styling (`height`, `border`, `background`, etc.) of the field is completely left to your CSS stylesheets.

To help you with this integration, the following classes are added to the container field:

* `HiPayField--empty `
* `HiPayField--focused `
* `HiPayField--valid `
* `HiPayField--invalid `

[Container documentation](#hipay-sdk-js-reference-hostedfield-container)

#### Internal styling

The internal styling refer to the properties inside the generated field like text or icons properties. 

These CSS properties are passed during Step 3. Let's add styles to our previous config.

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

You can interact with the card instance initiated above by listening to events on it.

Let's enable your submit button when form is valid and show the error if not.

```html
<script>
    /* Listening to change event on card instance */
    cardInstance.on('change', function(event){
        /* Display error if one */
        document.getElementById("hipay-error-message").innerHTML = event.error;
        /* Enable / disable submit button */
        document.getElementById("hipay-submit-button").disabled = !event.valid;
    });
</script>
```

[Events documentation](#hipay-sdk-js-reference-the-payment-product-instance-instanceonevent-callback)

## 6 - Tokenize your card

To securely transmit the sensible information, the HiPay Hostedfields converts them in a token that you will send to the API to process your payment. With this token, you'll never be able to get back the card information.

You can tokenize information by calling the `createToken()` function of the card you initiated previously. 
<br>
The best way to tokenize is to add an event listener on the `submit` event of your form.

The `card.createToken()` function return a `Promise`. When succeed, it returns an object with the token. When reject, it returns the error as String.
 
 ```html
<script>
    /* Get form */
    let cardForm = document.getElementById("hipay-form");
    
    /* Add event listener on submit button clicked */
    cardForm.addEventListener("submit", function(event) {
      event.preventDefault();
      /* Tokenize your card when the submit button is clicked */
      cardInstance.createToken().then(
        function(response) {
          /* Send token to your server  to process payment */
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