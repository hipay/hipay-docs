# HiPay Enterprise JavaScript SDK for tokenization (Direct Post)

The HiPay Enterprise JavaScript SDK for tokenization allows you to tokenize credit or debit cards using the HiPay Enterprise payment platform, directly from the web browser. This method (called Direct Post in the PCI Council terminology) enables you to offer a unified payment workflow to your customers while remaining PCI compliant.

# Security principle

Payment data (card number, card verification code and so on) will never hit your server: they will remain in the browser and will be sent directly to the HiPay Enterprise Secure Vault. This method is called "Direct Post". That way, you can create your own payment form, hosted on your server. Once the user validates the form, payment data are sent to the HiPay Enterprise platform through the HiPay Enterprise JavaScript SDK, which returns a token. Then, you can process payments with the token on the server side.

# Installation

### Using Bower

The easiest way to use the SDK is to install it using [Bower](http://bower.io). To do so, type the following command into a terminal window:

```sh
$ bower install hipay-fullservice-sdk-js
```

### Cloning the repository

You can clone the repository by typing the following command into a terminal window:

```sh
$ git clone git://github.com/hipay/hipay-fullservice-sdk-js
```

### Downloading the archive

You can also download the source code in ZIP format.

# Example app

## Scope

You can test our example app which is available in the `example` directory. This app leverages both the HiPay JavaScript SDK and PHP SDK in order to tokenize the card number (in the browser, using the JavaScript SDK) and make a transaction with this token on the server side using the PHP SDK.

![HiPay Enterprise Direct Post Simulator](images/screenshot.png)

## Setup

### Prerequisites

The example app is plug-and-play with Docker. This procedure requires Docker to be installed on your machine.

### Procedure

In order to test the example app, open a terminal and go to the `example` directory. Then, type the following command:

	$ docker-compose up -d

Then, you need to open the following file: `example/credentials.php` to put your own HiPay Enterprise credentials. Follow the instructions inserted in PHP comments.

Finally, open your web browser and go to: http://localhost:8080/example/. 
You should see the form shown in the screenshot above.

# Integration guide

The following integration guide describes step by step how to use the HiPay Enterprise JavaScript SDK for tokenization. The implementation can also be found in the example app (see the previous section).

## Import the JavaScript SDK

To use the SDK, you need to add the following script tag to your HTML pages:

```html
<script type="text/javascript" src="dist/hipay-fullservice-sdk.js"></script>
```
For better performance, you can use the minified version:

```html
<script type="text/javascript" src="dist/hipay-fullservice-sdk.min.js"></script>
```

## HTML-side integration
Then, you need to add a payment form to your checkout. Here is a basic HTML example:

```html
<div class="scontainer" id="form">
                <p class="form-description details">
                    Enter your payment details:
                </p>

                <div class="row">
                    <div class="col-lg-12">
                        <div class="form-group">
                            <label class="sr-only" for="input-card">Card number</label>
                            <div class="input-group">
                                <div class="input-group-addon-icon input-group-addon"><span class="glyphicon glyphicon-credit-card" aria-hidden="true"></span></div>
                                <input type="tel" class="form-control" id="input-card"  placeholder="Ex : 5136 0000 0000 0000" autocomplete="off" pattern="\d*" name="cardNumber" value="">
                            </div>
                            <div id="creditCardNumberMessageContainer" class="inputMessageContainer"></div>
                        </div>
                    </div>
                </div>
                
                <div class="row">
                    <div class="col-lg-12">
                        <div class="form-group">
                            <label class="sr-only" for="input-name">Prénom Nom</label>
                            <div class="input-group">
                                <div class="input-group-addon-icon input-group-addon"><span class="glyphicon glyphicon-user" aria-hidden="true"></span></div>

                                <input type="text" class="form-control" id="input-name" placeholder="Prénom Nom" value="">
                            </div>
                        </div>
                    </div>
                </div>

                <div class="row">
                    <div class="col-lg-12">
                        <div class="form-group">
                            <label class="sr-only" for="input-name">MM / YY</label>
                            <div class="input-group">
                                <div class="input-group-addon-icon input-group-addon"><span class="glyphicon glyphicon-calendar" aria-hidden="true"></span></div>

                                <input type="text" class="form-control" id="input-expiry-date" placeholder="MM / YY" value="">
                            </div>
                            <div id="creditCardExpiryDateMessageContainer" class="inputMessageContainer"></div>

                        </div>
                    </div>
                </div>

                <div class="row">
                    <div class="col-lg-12">

                        <div class="form-group">
                            <label class="sr-only" for="input-cvv">123</label>
                            <div id="container-cvv" class="input-group">
                                <div class="input-group-addon-icon input-group-addon"><span class="glyphicon glyphicon-lock" aria-hidden="true"></span></div>
                                <input class="form-control" id="input-cvv" placeholder="123" maxlength="3" value="">
                                <span id="cvv-button" class="input-group-addon"><button type="button" data-toggle="modal" data-target="#cvv-modal">?</button></span>
                            </div>
                            <div id="creditCardCVVMessageContainer" class="inputMessageContainer"></div>
                        </div>
                    </div>
                </div>


                <div class="row">
                    <div class="col-lg-12">
                        <div id="submit-zone">
                            <div id="error"></div>
                            <button class="btn btn-large" type="button" data-toggle="modal" data-target="#other-method-modal" id="pay-button">Tokenize</button>
                        </div>
                    </div>
                </div>

            </div>
```

## JavaScript processing
You can verify the validity of the form with HiPay.Form.paymentFormDataIsValid().
In this example the submit button is disable until the form is valid :
```js
$("#pay-button").prop('disabled', !HiPay.Form.paymentFormDataIsValid());
```

You can add a callback function when the form data change. 
For example when the form change you can :

1) get CVV information with HiPay.getCVVInformation()
2) get the validity of the form with HiPay.Form.paymentFormDataIsValid()
3) get error list if the form is not valid
```js
            HiPay.Form.change(function() {
                // get CVV information
                console.log(HiPay.getCVVInformation());
                console.log('change form');
                // enable / disable submit button with form validity
                $("#pay-button").prop('disabled', !HiPay.Form.paymentFormDataIsValid());
                // get error list if form is not valid
                var errorCollection = HiPay.Form.paymentFormDataGetErrors();
                console.log("errorCollection from client");
                console.log(errorCollection);
            });
```
Once the user validates the form, you must use the JavaScript SDK in order to tokenize the card. Here is an example using *jQuery*:
First set target and credentials with  HiPay.setTarget() and  HiPay.setCredentials()
Then Use HiPay.Form.tokenizePaymentFormData() witch return a promise.
In case of success a card token is returned. In other case an error is returned.
For example HiPay.ErrorReason.APIIncorrectCredentials, HiPay.ErrorReason.InvalidCardToken, ...


```js
 $("#pay-button").click(function() {

            // disable submit button and display loader
            $("#form :input").prop("disabled", true);
            $("#form :button").prop("disabled", true);
            $("#error").text("");

            $("#pay-button").text("Loading…");

            // set target and credentials
            HiPay.setTarget('stage'); // default is production/live
            HiPay.setCredentials('<?php echo $credentials['public']['username']; ?>', '<?php echo $credentials['public']['password']; ?>');


            HiPay.Form.tokenizePaymentFormData()
                .then(function(cardToken) {
                token = cardToken.token;
                $("#pay-button").text("Tokenize");
                $("#order").text("The token has been created using the JavaScript SDK (client side).");

                $('#code').text(JSON.stringify(cardToken, null, 4));
                $('#link-area').text('');

                $("#charge-button").show();
            })
                .catch(function(error){
                    if (error.code === HiPay.ErrorReason.APIIncorrectCredentials) { // égal à 1012003
                        console.log("Invalid crédentials");
                    }

                    if (error.code === HiPay.ErrorReason.InvalidCardToken) { // égal à 1012003
                        console.log("Token passé invalide…");
                    }


                    $("#pay-button").text("Tokenize");
                    $("#form :input").prop("disabled", false);
                    $("#form :button").prop("disabled", false);


                    if (error.errorCollection != undefined && error.errorCollection.length > 0) {
                        for (var i = 0; i < error.errorCollection.length; i++) {
                            var errorParameters = error.errorCollection[i];
                            $("#error").append(errorParameters.message);
                        }
                    }

                });

            return false;
        });
```

Once the token is retrieved, you can process a payment on the server side. Check out the example app's source code for more information.

# SDK reference

Please find below the methods and parameters made available by the SDK.

## Basic methods and request parameters

| Field name   |      Format      |  Description |
|----------|:-------------:|------|
| `setTarget` |  AN | If you are testing in your stage or production account, you can choose the following targets:<br/>- `stage`<br/>- `production`
| `setCredentials` |  AN | Your Direct Post API credentials. **Be careful! Do not use classic API credentials but public API credentials created in the HiPay Enterprise back office.**
| `create` |  - | Credit card information. Please refer to the “Create token request parameters” table below.

## Create token request parameters

| Field name   |  Format |  Length |   Req. |  Description |
|----------|:-------------:|:------:|:------:|------|
| `card_number` |  N | 19 |  M | Card number, with a 12- to 19-digit length
| `card_expiry_month` |  N | 2|  M | Card expiry month, expressed with two digits (e.g.: 01)
| `card_expiry_year` |  N | 4|  M | Card expiry year, expressed with four digits (e.g.: 2014)
| `card_holder` |  AN | 25|  - | Cardholder’s name as it appears on the card (up to 25 characters)
| `cvc` |  N | 4|  - | 3- or 4-digit security code (called CVC2, CVV2 or CID depending on the card's brand) as it appears on the credit card
| `multi_use` |  N | 1|  - | Indicates if the token should be generated either for single use or multi-use.<br/>Possible values:<br/>1 = Generate a multi-use token<br/>0 = Generate a single-use token.<br/>While a single-use token is typically generated for a short time and for processing a single transaction, multi-use tokens are generally generated for recurring payments.
 
## Create token response parameters
The following table lists and describes response fields.

| Field name   |      Description     |
|----------|------------|
|`token`| Token that was created |
|`request_id`| Request ID linked to the token |
|`brand`| Card's brand (e.g.: Visa, MasterCard, American Express, JCB, Discover, Diners Club, Solo, Laser, Maestro) |
|`pan`| Card number (up to 19 characters). Please note: due to PCI DSS security standards, our system has to mask credit card numbers in any output (e.g.: 549619******4769).|
|`card_holder`| Cardholder’s name |
|`card_expiry_month`| Card expiry month (2 digits) |
|`card_expiry_year`| Card expiry year (4 digits) |
|`issuer`| Card-issuing bank’s name<br/>Do not rely on this value to remain static over time. Bank names may change over time due to acquisitions and mergers.|
|`country`| Bank country code where the card was issued. This two-letter country code complies with ISO 3166-1 (alpha 2).|
|`card_type`| Card type (if applicable, e.g.: “DEBIT, CREDIT”) |
|`card_category`| Card category (if applicable, e.g.: “PLATINUM”) |
