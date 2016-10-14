# HiPay Fullservice SDK for JavaScript (Direct Post)

The HiPay Fullservice SDK for JavaScript allows you to tokenize credit or debit cards against the HiPay Fullservice payment platform, directly from the web browser. This method (called Direct Post in the PCI council terminology) allows you to offer a unfied payment workflow to your customers while remaining PCI compliant.

# Security principle

The payment data (card number, card verification code and so on) will never hit your server, they will remain in the browser and will be sent directly yo the HiPay Fullservice's Secure Vault. This method is called "Direct Post". That way, you can create your own payment form, hosted on your server. Once the user validates the form, payment data are sent to the HiPay Fullservice platform through the HiPay Fullservice SDK for Javascript which returns a token. Then, you can process payments with the token on the server side.

# Installation

### Using Bower

The easiest way to use the SDK is to install it using [Bower](http://bower.io). To do so, type the following into a terminal window:

```sh
$ bower install hipay-fullservice-sdk-js
```

### Cloning the repository

You can clone the repository by typing the following command into a terminal window:

```sh
$ git clone git://github.com/hipay/hipay-fullservice-sdk-js
```

### Downloading archive

You can also download the source code in ZIP format.

# Example app

## Scope

You can test our example app which is available in the `example` directory. This app leverages both the HiPay's JavaScript SDK and PHP SDK in order to tokenize the card number (in the browser, using the JavaScript SDK) and make a transaction with this token on the server side using the PHP SDK.

![HiPay Fullservice Direct Post Simulator](images/screenshot.png)

## Setup

### Prerequisites

The example app is plug and play with Docker. This procedure assumes that Docker is installed on your machine.

### Procedure

In order to test the example app, open a terminal and go to the `example` directory. Then, type the following command:

	$ docker-compose up -d

Once the Docker container is up and running, type the following command:

	$ docker-compose run web /bin/bash example/setup.sh

Then, you need to open the following file: `example/credentials.php` to put your own HiPay Fullservice credentials. Follow the instructions inserted in PHP comments.

Finally, open your web browser and go to: http://localhost:8080/example/  
You should see the form presented in the screenshot above.

# Integration guide

The integration guide below describes step by step how to use the HiPay Fullservice SDK for JavaScript. The implementation can also be found in the example app (see the previous section).

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
<div id="form" class="col-md-5">
  
  <div class="details">Enter your payment details:</div>
  
  <div>
      <div class="input-group" id="card-number">
          <span class="input-group-addon"></span>
          <input id="input-card" type="number" placeholder="Card number" maxlength="16">
      </div>
      <div class="input-group" id="name">
          <span class="input-group-addon"></span>
          <input id="input-name" type="text" placeholder="Cardholder">
      </div>
  </div>
  
  <div>
      <div class="input-group" id="date">
          <span class="input-group-addon"></span>
          <input id="input-month" type="number" placeholder="Month" maxlength="2">
          <input id="input-year" type="number" placeholder="Year" maxlength="4">
      </div>
  <div>
      <div class="input-group col-md-6" id="cvv">
          <span class="input-group-addon"></span>
          <input id="input-cvv" type="number" placeholder="CVV" maxlength="3">
      </div>
      <div id="cvv-button" class="col-md-6">
          <button type="button" data-toggle="modal" data-target="#cvv-modal">?</button>
      </div>
  </div>

  </div>

  <div id="submit-zone">
      <div id="error"></div>
      <button type="button" data-toggle="modal" data-target="#other-method-modal" id="pay-button">Tokenize</button>
  </div>
</div>
```

## JavaScript processing
Once the user validates the form, you must use the JavaScript SDK in order to tokenize the card. Here is an example using *jQuery*:

```js
$("#pay-button").click(function() {

  var params = {
    card_number: $('#input-card')[0].value,
    cvc: $('#input-cvv')[0].value,
    card_expiry_month: $('#input-month')[0].value,
    card_expiry_year: $('#input-year')[0].value,
    card_holder: $('#input-name')[0].value,
    multi_use: '0'
  };


  HiPay.setTarget('stage'); // default is production/live
  
  // These are fake credentials, put your own credentials here (HiPay Fullservice back office > Integration > Security settings and create credentials with public visibility)
  HiPay.setCredentials('11111111.stage-secure-gateway.hipay-tpp.com', 'Test_pMAjfghBqya7TA9jqhYah56');

  HiPay.create(params,
    
    function(result) {
	   
		// The card has successfully been tokenized
			   	
		token = result.token;

    }, 

    function (errors) {
    	
      // An error occurred
      	
      if (typeof errors.message != "undefined") {
        $("#error").text("Error: " + errors.message);
      } else {
        $("#error").text("An error occurred with the request.");
      }
    }
  );

  return false;
});
```

Once the token is retrieved, you can process a payment on the server side. Check out the example app's source code for more information.

# SDK reference

You will find below the methods and parameters made available by the SDK.

## Basic methods and request parameters

| Field name   |      Format      |  Description |
|----------|:-------------:|------|
| `setTarget` |  AN | If you are testing in your stage or production account, you can choose the following targets:<br/>- `stage`<br/>- `production`
| `setCredentials` |  AN | Your direct post API credentials. **Be careful! Do not use classic API credentials but public API credentials created on the HiPay Fullservice back office.**
| `create` |  - | Credit card information. Please refer to the “Create token request parameters” table below.

## Create token request parameters

| Field name   |  Format |  Length |   Req. |  Description |
|----------|:-------------:|:------:|:------:|------|
| `card_number` |  N | 19 |  M | The card number. The length is from 12 to 19 digits.
| `card_expiry_month` |  N | 2|  M | The card expiry month. Expressed with two digits (e.g.: 01).
| `card_expiry_year` |  N | 4|  M | The card expiry year. Expressed with four digits (e.g.: 2014).
| `card_holder` |  AN | 25|  - | The cardholder’s name as it appears on the card (up to 25 characters).
| `cvc` |  N | 4|  - | The 3- or 4- digit security code (called CVC2, CVV2 or CID depending on the card brand) that appears on the credit card.
| `multi_use` |  N | 1|  - | Indicates if the token should be generated either for a single-use or a multi-use.<br/>Possible values:<br/>1 = Generate a multi-use token<br/>0 = Generate a single-use token.<br/>While a single-use token is typically generated for a short time and for processing a single transaction, multi-use tokens are generally generated for recurrent payments.
 
## Create token response parameters
The following table lists and describes the response fields.

| Field name   |      Description     |
|----------|------------|
|`token`| Token that was created. |
|`request_id`| The request ID linked to the token. |
|`brand`| Card brand. (e.g.: Visa, MasterCard, American Express, JCB, Discover, Diners Club, Solo, Laser, Maestro) |
|`pan`| Card number (up to 19 characters). Please note: due to PCI DSS security standards, our system has to mask credit card numbers in any output (e.g.: 549619******4769).|
|`card_holder`| Cardholder’s name. |
|`card_expiry_month`| Card expiry month (2 digits) |
|`card_expiry_year`| Card expiry year (4 digits) |
|`issuer`| Card-issuing bank’s name<br/>Do not rely on this value to remain static over time. Bank names may change over time due to acquisitions and mergers.|
|`country`| Bank country code where the card was issued. This two-letter country code complies with ISO 3166-1 (alpha 2).|
|`card_type`| Card type (if applicable, e.g.: “DEBIT, CREDIT”). |
|`card_category`| Card category (if applicable, e.g.: “PLATINUM”). |
