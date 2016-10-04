# HiPay Fullservice - Javascript SDK (Direct Post)

##Payment Gateway Direct Post Integration Guide

## HTML-based integration
This integration will merge with your existing HTML in a page using a payment form from which data will be used by the HiPay’s JavaScript Direct Post library.

This document is designed to give you details on how to integrate your business to the HiPay Fullservice Payment Gateway using a “direct post” method. It provides step-by-step instructions on how to simply and quickly get up and running with our services, as well as detailed reference material.

**Disclamer**
While every effort has been made to ensure the accuracy of the information contained in this publication, the information is supplied without representation or warranty of any kind, is subject to change without notice and does not represent a commitment on the part of HiPay. HiPay, therefore, assumes               no responsibility and shall have no liability, consequential or otherwise, of any kind arising from this material or any part thereof, or any supplementary materials subsequently issued by HiPay.

**Technical support**
If you need any complementary information concerning the technical implementation of HiPay Fullservice, don’t hesitate to contact our Business IT Services:- Web: http://help.hipay.com - Email: support.tpp@hipay.com- Telephone: +33 (0)1 82 88 68 68

##HTML Form Example

```html
<form method="POST" action="#" id="hipay-direct-post">	<select name="cc_type" id="cc_type"> <!— Card brand —->        	   <option value="cb">Carte Bancaire</option>        	   <option value="visa">Visa</option>        	   <option value="mastercard">MasterCard</option>        	   <option value="american-express">American Express</option>      	</select>      	<label for="cc_number">Card Number</label>      	<input type="text" name="cc_number" id="cc_number" placeholder="XXXX XXXX XXXX XXXX" maxlength="20" size="20" autocomplete="off" >      	<label for="cc_exp_month">Expiration Month</label>      	<input type="text" name="cc_exp_month" id="cc_exp_month" placeholder="00" autocomplete="off">      	<label for="cc_exp_year">Expiration Year</label>      	<input type="text" name="cc_exp_year" id="cc_exp_year" placeholder="00" autocomplete="off">      	<label for="cc_name">Cardholder's Name</label>      	<input type="text" name="cc_name" id="cc_name" placeholder="John Doe" autocomplete="off">      	<label for="cc_cvc">Card Validation Code</label>      	<input type="text" name="cc_cvc" id="cc_cvc" placeholder="123" maxlength="4" size="3" autocomplete="off">    	<input type="submit" value="Pay" /></form>```

##JavaScript
With the above HTML template, you must include the HiPay JavaScript library.

###JavaScript: Include this example
`<script type="text/javascript" src="javascripts/token-2.1.1.min.js"></script>`

You must also add JavaScript instructions so that no card data is sent to your server unencrypted.

###JavaScript request parameters

| Field name   |      Format      |  Description |
|----------|:-------------:|------:|
| setTarget |  AN | If you are testing in your stage or production account, you can choose the following targets:<br/>- test<br/>- live
| setCredentials |  AN | Your direct post API credentials. *Be careful*! Do not use classic API credentials but public API credentials created on the HiPay Fullservice back office.
| create |  - | Credit card information. Please refer to the “Create token request parameters” table below.

###Create token request parameters

| Field name   |  Format |  Length |   Req. |  Description |
|----------|:-------------:|------:|------:|------:|
| card_number |  N | 19 |  M | The card number. The length is from 12 to 19 digits.
| card_expiry_month |  N | 2|  M | The card expiry month. Expressed with two digits (e.g.: 01).
| card_expiry_year |  N | 4|  M | The card expiry year. Expressed with four digits (e.g.: 2014).
| card_holder |  AN | 25|  - | The cardholder’s name as it appears on the card (up to 25 characters).
| cvc |  N | 4|  - | The 3- or 4- digit security code (called CVC2, CVV2 or CID depending on the card brand) that appears on the credit card.
| multi_use |  N | 1|  - | Indicates if the token should be generated either for a single-use or a multi-use.<br/>Possible values:<br/>1 = Generate a multi-use token<br/>0 = Generate a single-use token.<br/>While a single-use token is typically generated for a short time and for processing a single transaction, multi-use tokens are generally generated for recurrent payments.
 ###JavaScript response parameters
The following table lists and describes the response fields.

| Field name   |      Description     |
|----------|:------------:|
|token| Token that was created. ||request_id| The request ID linked to the token. ||brand| Card brand. (e.g.: Visa, MasterCard, American Express, JCB, Discover, Diners Club, Solo, Laser, Maestro) ||pan| Card number (up to 19 characters). Please note: due to PCI DSS security standards, our system has to mask credit card numbers in any output (e.g.: 549619******4769).||card_holder| Cardholder’s name. ||card_expiry_month| Card expiry month (2 digits) ||card_expiry_year| Card expiry year (4 digits) ||issuer| Card-issuing bank’s name<br/>Do not rely on this value to remain static over time. Bank names may change over time due to acquisitions and mergers.||country| Bank country code where the card was issued. This two-letter country code complies with ISO 3166-1 (alpha 2).||card_type| Card type (if applicable, e.g.: “DEBIT, CREDIT”). ||card_category| Card category (if applicable, e.g.: “PLATINUM”). |

##JavaScript direct post Example

```js
<script type="text/javascript">    $(document).ready(function(){        $("#pay").click(function() {        TPP.setTarget('test');        TPP.setCredentials('12345.stage-secure-gateway.hipay-tpp.com', ' Test_YhghjTrfdErthIo');        TPP.create({          card_number:  document.getElementById("cc_number").value,          cvc: document.getElementById("cc_cvc").value,          card_expiry_month:document.getElementById("cc_exp_month").value,          card_expiry_year: document.getElementById("cc_exp_year").value,          card_holder: document.getElementById("cc_name").value,          multi_use: '0'        },          function(result) {            var dump = document.getElementById('dump');            var payment = document.getElementById('paymentlink');            var type = document.getElementById("cc_type").options[document.getElementById("cc_type").selectedIndex].value;            var out = '';		            for (var i in result) {                out += i + ": " + result[i] + "\n";                if (i == "token") {                 	payment.href = 'hipay/order.php?type='+type+'&token=' + result[i]; document.getElementById('usetoken').style = 'display:inherit;';                 }            }            dump.value = out;          },          function(result) {            var dump = document.getElementById('dump');            var out = '';
                        for (var i in result) {                out += i + ": " + result[i] + "\n";            }            dump.value = out;         }        );                return false;    });    });    </script>
```