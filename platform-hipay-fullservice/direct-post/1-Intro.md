![](images/media/image1.jpg){width="8.286897419072616in"
height="4.4375in"}![](images/media/image2.png){width="8.334722222222222in"
height="1.9951388888888888in"}![](images/media/image3.png){width="3.5388593613298336in"
height="0.4847222222222222in"}

<span id="_Toc328325173" class="anchor"><span
id="Chapter3_RESTAPI_Resources" class="anchor"></span></span>

HTML-based integration

This integration will merge with your existing HTML in a page using a
payment form from which data will be used by the HiPay’s JavaScript
Direct Post library.

  --------------------------------------------------------------------------------------------------------------------------------------------------------------------
  HTML Form Example   1.  &lt;form method="POST" action="\#" id="hipay-direct-post"&gt;
                      
                      2.  &lt;select name="cc\_type" id="cc\_type"&gt; &lt;!— Card brand —-&gt;
                      
                      3.  &lt;option value="cb"&gt;Carte Bancaire&lt;/option&gt;
                      
                      4.  &lt;option value="visa"&gt;Visa&lt;/option&gt;
                      
                      5.  &lt;option value="mastercard"&gt;MasterCard&lt;/option&gt;
                      
                      6.  &lt;option value="american-express"&gt;American Express&lt;/option&gt;
                      
                      7.  &lt;/select&gt;
                      
                      8.  &lt;label for="cc\_number"&gt;Card Number&lt;/label&gt;
                      
                      9.  &lt;input type="text" name="cc\_number" id="cc\_number" placeholder="XXXX XXXX XXXX XXXX" maxlength="20" size="20" autocomplete="off" &gt;
                      
                      10. &lt;label for="cc\_exp\_month"&gt;Expiration Month&lt;/label&gt;
                      
                      11. &lt;input type="text" name="cc\_exp\_month" id="cc\_exp\_month" placeholder="00" autocomplete="off"&gt;
                      
                      12. &lt;label for="cc\_exp\_year"&gt;Expiration Year&lt;/label&gt;
                      
                      13. &lt;input type="text" name="cc\_exp\_year" id="cc\_exp\_year" placeholder="00" autocomplete="off"&gt;
                      
                      14. &lt;label for="cc\_name"&gt;Cardholder's Name&lt;/label&gt;
                      
                      15. &lt;input type="text" name="cc\_name" id="cc\_name" placeholder="John Doe" autocomplete="off"&gt;
                      
                      16. &lt;label for="cc\_cvc"&gt;Card Validation Code&lt;/label&gt;
                      
                      17. &lt;input type="text" name="cc\_cvc" id="cc\_cvc" placeholder="123" maxlength="4" size="3" autocomplete="off"&gt;
                      
                      18. &lt;input type="submit" value="Pay" /&gt;
                      
                      19. &lt;/form&gt;
                      
                      
  ------------------- ------------------------------------------------------------------------------------------------------------------------------------------------
  --------------------------------------------------------------------------------------------------------------------------------------------------------------------

<span id="_Toc453598110" class="anchor"></span>

<span id="_Toc328325174" class="anchor"></span>JavaScript

With the above HTML template, you must include the HiPay JavaScript
library.

+--------------------------------------+--------------------------------------+
| JavaScript: Include this\            | 1.  &lt;script                       |
| example                              |     type="text/javascript"           |
|                                      |     src="javascripts/token-2.1.1.min |
|                                      | .js"&gt;&lt;/script&gt;              |
|                                      |                                      |
                                                                             
+======================================+======================================+
+--------------------------------------+--------------------------------------+

You must also add JavaScript instructions so that no card data is sent
to your server unencrypted.

<span id="_Toc453598112" class="anchor"></span>JavaScript request
parameters

  -------------------------------------------------------------------------------------------------------------------------------------------------------
  Field name       Format   Description
  ---------------- -------- -----------------------------------------------------------------------------------------------------------------------------
  setTarget        AN       If you are testing in your stage or production account, you can choose the following targets:
                            
                            -   test
                            
                            -   live
                            
                            

  setCredentials   AN       Your direct post API credentials.
                            
                            **Be careful! Do not use classic API credentials but public API credentials created on the HiPay Fullservice back office.**

  create                    Credit card information. *Please refer to the “Create token request parameters” table below.*
  -------------------------------------------------------------------------------------------------------------------------------------------------------

Create token request parameters

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  > Field name          Format   Length   Req.[^1]   Description
  --------------------- -------- -------- ---------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  card\_number          N        19       M          The card number.
                                                     
                                                     The length is from 12 to 19 digits.

  card\_expiry\_month   N        2        M          The card expiry month.
                                                     
                                                     Expressed with two digits (e.g.: 01).

  card\_expiry\_year    N        4        M          The card expiry year.
                                                     
                                                     Expressed with four digits (e.g.: 2014).

  card\_holder          AN       25                  The cardholder’s name as it appears on the card (up to 25 characters).

  cvc                   N        4                   The 3- or 4- digit security code (called CVC2, CVV2 or CID depending on the card brand) that appears on the credit card.

  multi\_use            N        1                   Indicates if the token should be generated either for a single-use or a multi-use.
                                                     
                                                     Possible values:
                                                     
                                                     **1** = Generate a multi-use token
                                                     
                                                     **0** = Generate a single-use token
                                                     
                                                     While a single-use token is typically generated for a short time and for processing a single transaction, multi-use tokens are generally generated for recurrent payments.
  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

JavaScript response parameters

The following table lists and describes the response fields.

+--------------------------------------+--------------------------------------+
| **Field name**                       | **Description**                      |
+======================================+======================================+
| **token**                            | Token that was created.              |
+--------------------------------------+--------------------------------------+
| **request\_id**                      | The request ID linked to the token.  |
+--------------------------------------+--------------------------------------+
| **brand**                            | Card brand. (e.g.: Visa, MasterCard, |
|                                      | American Express, JCB, Discover,     |
|                                      | Diners Club, Solo, Laser, Maestro)   |
+--------------------------------------+--------------------------------------+
| **pan**                              | Card number (up to 19 characters)\   |
|                                      | Please note: due to PCI DSS security |
|                                      | standards, our system has to mask    |
|                                      | credit card numbers in any output    |
|                                      | (e.g.: 549619\*\*\*\*\*\*4769).      |
+--------------------------------------+--------------------------------------+
| **card\_holder**                     | Cardholder’s name.                   |
+--------------------------------------+--------------------------------------+
| **card\_expiry\_month**              | Card expiry month (2 digits)         |
+--------------------------------------+--------------------------------------+
| **card\_expiry\_year**               | Card expiry year (4 digits)          |
+--------------------------------------+--------------------------------------+
| **issuer**                           | Card-issuing bank’s name             |
|                                      |                                      |
|                                      | Do not rely on this value to remain  |
|                                      | static over time.                    |
|                                      |                                      |
|                                      | Bank names may change over time due  |
|                                      | to acquisitions and mergers.         |
+--------------------------------------+--------------------------------------+
| **country**                          | Bank country code where the card was |
|                                      | issued.                              |
|                                      |                                      |
|                                      | This two-letter country code         |
|                                      | complies with ISO 3166-1 (alpha 2).  |
+--------------------------------------+--------------------------------------+
| **card\_type**                       | Card type (if applicable, e.g.:      |
|                                      | “DEBIT, CREDIT”).                    |
+--------------------------------------+--------------------------------------+
| **card\_category**                   | Card category (if applicable, e.g.:  |
|                                      | “PLATINUM”).                         |
+--------------------------------------+--------------------------------------+

  ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  JavaScript direct post Example   1.  &lt;script type="text/javascript"&gt;
                                   
                                   2.  3.  \$(document).ready(function(){
                                   
                                   4.  5.  \$("\#pay").click(function() {
                                   
                                   6.  TPP.setTarget('test');
                                   
                                   7.  TPP.setCredentials('12345.stage-secure-gateway.hipay-tpp.com', ' Test\_YhghjTrfdErthIo');
                                   
                                   8.  TPP.create({
                                   
                                   9.  card\_number: document.getElementById("cc\_number").value,
                                   
                                   10. cvc: document.getElementById("cc\_cvc").value,
                                   
                                   11. card\_expiry\_month:document.getElementById("cc\_exp\_month").value,
                                   
                                   12. card\_expiry\_year: document.getElementById("cc\_exp\_year").value,
                                   
                                   13. card\_holder: document.getElementById("cc\_name").value,
                                   
                                   14. multi\_use: '0'
                                   
                                   15. },
                                   
                                   16. function(result) {
                                   
                                   17. var dump = document.getElementById('dump');
                                   
                                   18. var payment = document.getElementById('paymentlink');
                                   
                                   19. var type = document.getElementById("cc\_type").options\[document.getElementById("cc\_type").selectedIndex\].value;
                                   
                                   20. var out = '';
                                   
                                   21. 22. for (var i in result) {
                                   
                                   23. out += i + ": " + result\[i\] + "\\n";
                                   
                                   24. if (i == "token") {
                                   
                                   25. payment.href = 'hipay/order.php?type='+type+'&token=' + result\[i\]; document.getElementById('usetoken').style = 'display:inherit;';
                                   
                                   26. }
                                   
                                   27. }
                                   
                                   28. 29. dump.value = out;
                                   
                                   30. },
                                   
                                   31. function(result) {
                                   
                                   32. var dump = document.getElementById('dump');
                                   
                                   33. var out = '';
                                   
                                   34. for (var i in result) {
                                   
                                   35. out += i + ": " + result\[i\] + "\\n";
                                   
                                   36. }
                                   
                                   37. 38. dump.value = out;
                                   
                                   39. }
                                   
                                   40. );
                                   
                                   41. 42. return false;
                                   
                                   43. });
                                   
                                   44. 45. });
                                   
                                   46. &lt;/script&gt;
                                   
                                   
  -------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------
  ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------

[^1]: Specifies whether an element is mandatory (M) or not.
