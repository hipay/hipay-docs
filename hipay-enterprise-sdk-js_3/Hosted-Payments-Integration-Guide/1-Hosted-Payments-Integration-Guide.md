# HiPay Hosted Payments integration guide


This guide will walk you through the creation of a fully integrated payment form with multiple payment products using the HiPay Hosted Fields.

You will see how to create your payment form, customize it, interact with it, and securely get `data` from it. These data will enable you to make a transaction using the [HiPay Order API](/doc-api/enterprise/gateway/#!/payments/requestNewOrder).

The HiPay Hosted Payments feature internally uses the HiPay Hosted Fields.

Please follow these 6 steps to create your payment form with the HiPay Hosted Payments:


1. [Set up the HiPay Enterprise JavaScript SDK](#hipay-hosted-payments-integration-guide-1-set-up-the-hipay-enterprise-javascript-sdk)
2. [Set up your HTML form](#hipay-hosted-payments-integration-guide-2-set-up-your-html-form)
3. [Create the Hosted Payments instance](#hipay-hosted-payments-integration-guide-3-create-the-hosted-payments-instance)
4. [Customize your payment form](#hipay-hosted-payments-integration-guide-4-customize-your-payment-form)
5. [Interact with the Hosted Payments instance](#hipay-hosted-payments-integration-guide-5-interact-with-the-hosted-payments-instance)
6. [Get payment data](#hipay-hosted-payments-integration-guide-6-get-payment-data)
  
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

In order to collect sensitive information in a secure way, the HiPay Enterprise JavaScript SDK will generate components hosted by HiPay on your page. 
<br>
With these components, sensitive information filled in by customers never hits your page or your web server.

You can now integrate your HTML page. 


You just need to place a `div` with a unique `id` in your page. The form will be injected inside automatically.


```html
<form id="hipay-form">
    /* The whole form will be inserted in this div */
    <div id="hipay-hostedpayments-form"></div>
    
    <button type="submit" id="hipay-submit-button" disabled="true">
        PAY
    </button>
</form>
```

## 3 - Create the Hosted Payments instance

Now that the HTML div is ready, we can generate the whole form inside.
Set config with the `hipay-hostedpayments-form` selector and call the `create` function with `hosted-payments` parameter.
```html
<script>
    var config = {
        selector: 'hipay-hostedpayments-form' // form container div id
    };
    
    var hostedPaymentsInstance = hipay.create('hosted-payments', config);
</script>
```

Go to: [hipay.create(type, options)](../Reference/#hipay-enterprise-javascript-sdk-reference-the-hipay-instance-hipaycreatetype-options) to see all supported configurations.


## 4 - Customize your payment form

#### Override default configuration

Hosted Payments form uses default parameters for each payment product. You can override them by adding custom configuration in the config object.

Here is an example of credit card and Sepa Direct Debit customization.
Like Hosted Fields, you can also customize the internal styling of the fields.


```html
<script>
    var config = {
       selector: 'hipay-hostedpayments-form',
       card: {
         multi_use: false,
         fields: {
           cardHolder: {
             defaultValue: 'John Doe'
           }
         }   
       },
       sdd: {
         fields: {
           firstname: {
             defaultValue: 'John'
           },
           lastname: {
             defaultValue: 'Doe'
           }
         }
       },
       styles: {
           base: { // default field styling
             color: "#000000",
             fontSize: "15px",
             fontWeight: 400,
             placeholderColor: "#999999",
             iconColor: 'green',
             caretColor: "green"
           },
           invalid: { // invalid field styling
             color: '#D50000',
             caretColor: '#D50000'
           }
       }
    };

    var hostedPaymentsInstance = hipay.create('hosted-payments', config);
</script>
```


Go to: [Create configuration](../Reference/#hipay-enterprise-javascript-sdk-reference-the-hipay-instance-hipaycreatetype-options)


#### Override default stylesheet

You can override the default CSS by adding your custom CSS file.

```css
.HiPayField--focused + .hipay-field-label {
  color: green;
}

.HiPayField--focused + .hipay-field-label + .hipay-field-baseline {
  border-bottom: solid 1px green;
}

[data-hipay-id='hipay-help-cvc'] {
  color: green;
  background-color: palegreen;
  border: solid 1px green;
}
```

Go to: [Base stylesheet reference](../Reference/#hipay-enterprise-javascript-sdk-reference-base-stylesheet)


## 5 - Interact with the Hosted Payments instance

You can interact with the Hosted Payments instance previously initialized by listening to events on it.

Here is how to enable your submit button when your form is valid.

```html
<script>
    /* Listen to change event on Hosted Payments instance */
    hostedPaymentsInstance.on('validityChange', function(event){
        /* Enable / disable submit button */
        document.getElementById("hipay-submit-button").disabled = !event.valid;
    });
</script>
```

Go to: [instance.on(‘event’, callback)](../Reference/#hipay-enterprise-javascript-sdk-reference-hosted-payments-instances-instanceonevent-callback)


## 6 - Get payment data

To securely transmit sensitive information, the HiPay Hosted Payments converts it into an object to be sent to the [HiPay Order API](/doc-api/enterprise/gateway/#!/payments/requestNewOrder) to process your payment. 

You can get information by calling the `getPaymentData()` function of the Hosted Payments instance.
<br>
The best way to do that is to add an event listener on the `submit` event of your form.

The `hostedPaymentsInstance.getPaymentData()` function returns a `Promise`. When successful, it returns an object with the payment product and data. When rejected, it returns error as an array.

 ```html
<script>
    /* Get form */
    let hostedForm = document.getElementById("hipay-form");
    
    /* Add event listener on the submit button when clicked */
    hostedForm.addEventListener("submit", function(event) {
      event.preventDefault();
      /* Get payment data when the submit button is clicked */
      hostedPaymentsInstance.getPaymentData().then(
        function(response) {
          /* Send response.datas to your server to process payment */
          handlePayment(response.datas);
        },
        function(error) {
          /* Display error */
        }
      );
    });
</script>
```

Here is a response from getPaymentData():

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

Go to: [instance.getPaymentData()](../Reference/#hipay-enterprise-javascript-sdk-reference-hosted-payments-instances-instancegetpaymentdata)
