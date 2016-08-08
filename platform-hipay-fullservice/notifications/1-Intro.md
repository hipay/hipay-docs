![](images/media/image2.jpeg){width="8.568452537182852in"
height="10.392856517935257in"}

![](images/media/image3.png){width="1.5762095363079616in"
height="1.0168547681539808in"}

**DISCLAIMER:**\
While every effort has been made to ensure the accuracy of the
information contained in this publication, the information is supplied
without representation or warranty of any kind, is subject to change
without notice and does not represent a commitment on the part of HiPay.
HiPay, therefore, assumes no responsibility and shall have no liability,
consequential or otherwise, of any kind arising from this material or
any part thereof, or any supplementary materials subsequently issued by
HiPay. HiPay has made every effort to ensure the accuracy of this
material.

**TECHNICAL SUPPORT\
**If you need any complementary information concerning the technical implementation of HiPay TPP don’t hesitate to contact our Technical Support team:\
\
Email: support.tpp@hipay.com\
Telephone: +33 (0)1 53 44 15 07 {#technical-support-if-you-need-any-complementary-information-concerning-the-technical-implementation-of-hipay-tpp-dont-hesitate-to-contact-our-technical-support-team-email-support.tpphipay.com-telephone-33-01-53-44-15-07 .En-ttedetabledesmatires}
=======================================================================================================================================================

 {#section .En-ttedetabledesmatires}

 {#section-1 .En-ttedetabledesmatires}

 {#section-2 .En-ttedetabledesmatires}

 {#section-3 .En-ttedetabledesmatires}

 {#section-4 .En-ttedetabledesmatires}

**Table of Contents** {#table-of-contents .En-ttedetabledesmatires}
=====================

About this Guide 4

How to Read this Guide 5

Chapter 1. Server-to-Server Notifications 7

What is a Server-to-Server Notification? 7

Setup 7

Configuration Parameters 8

Response Fields 8

Response fields specific to the payment product 11

Transaction Workflow 12

Chapter 2. Signature verification 15

Setup 15

Verification 16

**\
**

About this Guide {#about-this-guide .ListParagraph .ChapterTitle}
================

**Purpose**

This document is designed to provide you details on how to integrate
your business to the HiPay TPP payment gateway. This document provides
step-by-step instructions on how to simply and quickly get up and
running with our services as well as detailed reference material.

Where applicable, this document refers to the related documentation with
further details.

**Intended Audience**

The intended audience is the merchant's technical staff or the
merchant's system integrator.

Because almost all communication between the merchant's system and the
REST API is realized through predefined XML or JSON messages over the
Internet using standard protocols, you will need basic XML/JSON
programming skills and knowledge of HTTP(S). Furthermore, it is
recommended that you are familiar with the basics of tokenization
concepts.

**Copyright**

The information contained in this guide is proprietary and confidential
to HiPay and its members. This material may not be duplicated,
published, or disclosed, in whole or in part, without the prior written
permission of HiPay.

**Legal Notice**

This document contains the proprietary and confidential information of
HiPay. Such information may not be used for any unauthorized purpose and
may not be published or disclosed to third parties, in whole or part,
without the express written permission of HiPay. You acknowledge and
agree that between you and HiPay this document and all portions thereof,
including, but not limited to, any copyright, trade secret and other
intellectual property rights relating thereto, are and at all times
shall remain the sole property of HiPay and that title and full
ownership rights in the information contained herein and all portions
thereof are reserved to and at all times shall remain with HiPay. You
agree to safeguard the confidentiality of the information contained
herein using the same standard you employ to safeguard your own
confidential information of like kind, but in no event less than a
commercially reasonable standard of care. If you do not agree with the
foregoing conditions, you are required to return this document
immediately to HiPay.

<span id="_Toc241310459" class="anchor"><span id="_Toc253822243" class="anchor"></span></span>**How to Read this Guide** {#how-to-read-this-guide .ListParagraph}
========================================================================================================================

**Document Conventions**

To ease readability and improve understanding, this document uses a
number of conventions.

This guide uses several typographic conventions to highlight certain
words and phrases and draw attention to specific pieces of information.
The following table clarifies the conventions used across this guide.

Table 1: Typographic conventions that apply across this guide

  Convention            Purpose in this guide
  --------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Mono-space            Indicates source code, code examples, input to the command line, application output, code lines embedded in text, and variables and code elements.
  *Italic*              Italicized regular text is used for emphasis or to indicate a term that is being defined or will be defined shortly.
  ...                   Horizontal ellipsis points in sample code indicate the omission of part of the code. This is done when you would normally expect additional code to appear, but such code would not be related to the example.
  \$                    At the beginning of a command, indicates an operating system shell prompt.
  &lt;placeholder&gt;   Indicates placeholders, most often method or function parameters; these placeholders represent information that must be supplied by the implementation or the user. For command-line input, indicates parameter values.

**Abbreviations used in tables**

The tables of this document describe data elements. These data elements
are equivalent to parameters in a query or to fields of a response
message. The following table helps understanding the different
attributes (columns) that define a data element.

Table 2: Description of data elements

  ---------------------------------------------------------------------------------------------------------------------------------------
  Column        Explanation
  ------------- -------------------------------------------------------------------------------------------------------------------------
  Field Name    Contains the name of the data element.

  Format        The format of the element.

  Length        Denotes maximum size of the element.
                
                For example a size of 6 means that the data value cannot exceed six characters.
                
                When no size is specified, it means there is no limitation on the length of the field.

  Req.          Specifies whether an element is required or not.
                
                **M** = Mandatory; must always be provided
                
                **C** = Conditional; requirement depends on value or appearance of other elements.

  Description   Provides a brief description of the data element and where applicable, a list of valid values and element dependencies.
  ---------------------------------------------------------------------------------------------------------------------------------------

<span id="_Ref221954712" class="anchor"></span>Table 3: Available
formats of data elements

  Format Abbreviation   Description                                                 Example
  --------------------- ----------------------------------------------------------- ------------
  AN                    Alphanumeric characters (a-z, A-Z, 0-9)                     
  A                     Alphabetic characters only (a-z, A-Z)                       
  N                     Numerical only                                              
  R                     Decimal number with explicit decimal point, signed          12.34
  DT                    Date in the format YYYY-MM-DD                               2012-12-31
  TM                    Time in the format HH:MM with optional seconds (HH:MM:SS)   15:30

**Acronyms and Abbreviations**

Acronyms and abbreviations are used in this guide.

The following table shows the acronym or abbreviation and the
corresponding full name.

Table 4: Acronyms and Abbreviations

  Acronym or Abbreviation   Full Name
  ------------------------- -----------------------------------------------
  BIN                       Bank Identification Number
  PAN                       Primary Account Number
  PCI DSS                   Payment Card Industry Data Security Standards
  REST                      Representational State Transfer

\
<span id="_Toc248223031" class="anchor"><span id="_Toc253822244" class="anchor"></span></span>Server-to-Server Notifications {#server-to-server-notifications .ChapterTitle}
============================================================================================================================

### <span id="_Toc243383271" class="anchor"><span id="_Toc248223032" class="anchor"><span id="_Toc253822245" class="anchor"></span></span></span>What is a Server-to-Server Notification? {#what-is-a-server-to-server-notification .Chapter3}

In order to notify events related to your payment system, such as a new
transaction or a 3-D Secure transaction, our platform can send to your
application a Server-to-Server notification.

Please refer to “*HiPayTPP-GatewayAPI*” documentation for a complete and
detailed guide about all HiPay TPP API possibilities.

### <span id="_Toc243383272" class="anchor"><span id="_Toc248223033" class="anchor"><span id="_Toc253822246" class="anchor"></span></span></span>Setup {#setup .Chapter3}

To set your Notification URL you must login into your Hipay TPP back
office and go to\
“*Integration -&gt; Notifications*”.

![](images/media/image4.png){width="4.075580708661417in"
height="5.003472222222222in"}

### <span id="_Toc243383273" class="anchor"><span id="_Toc248223034" class="anchor"><span id="_Toc253822247" class="anchor"></span></span></span>Configuration Parameters {#configuration-parameters .ListParagraph .Chapter3}

  -----------------------------------------------------------------------------------------------------------------------------------------------------
  Field Name              Description
  ----------------------- -----------------------------------------------------------------------------------------------------------------------------
  Notification URL        The URL or IP on wich you want to receive server-to-server notifications.

  Request method          The method you wish to receive the requests:
                          
                          -   **XML**
                          
                          -   **HTTP POST**
                          
                          

  Desired notifications   Here you can define what notifications you want to receive based on transaction status.
                          
                          Refer to the appendices — "*Appendix B. Payment Status Definitions*” — for the full list of available transaction statuses.
  -----------------------------------------------------------------------------------------------------------------------------------------------------

### <span id="_Toc243383274" class="anchor"><span id="_Toc248223035" class="anchor"><span id="_Toc253822248" class="anchor"></span></span></span>Response Fields {#response-fields .ListParagraph .Chapter3}

The following table lists and describes the response fields received on
the notification call.

Field Name Description

**state** transaction state.\
\
Value must be a member of the following list.\
• completed\
• pending\
• declined\
• error\
\
Please report to the following section below —“*Transaction Workflow”* —
for further details.

**reason** optional element. Reason why transaction was declined.

**test** true if the transaction is a testing transaction, otherwise
false.

**mid** your merchant account number (issued to you by HiPay TPP).

**attempt\_id** attempt id of the payment.

**authorization\_code** an authorization code (up to 35 characters)
generated for each approved or pending transaction by the acquiring
provider.

**transaction\_reference** the unique identifier of the transaction.

**date\_created** time when transaction was created.

**date\_updated** time when transaction was last updated.

**date\_authorized** time when transaction was authorized.

**status** transaction status. A list of available statuses can be found
in the appendices – Appendix B “*Payment Status Definitions”* on
“*GatewayAPI*” documentation*.*

**message** transaction message.

**authorized\_amount** the transaction amount.

**captured\_amount** captured amount.

**refunded\_amount** refunded amount.

**decimals** decimal precision of transaction amount.

**currency** base currency for this transaction.\
This three-character currency code complies with ISO 4217.

**ip\_address** the IP address of the customer making the purchase.

**ip\_country** country code associated to the customer's IP address.

**device\_id** unique identifier assigned to device (the customer's
brower) by HiPay TPP.

**cdata1** Custom data.

**cdata2** Custom data.

**cdata3** Custom data.

**cdata4** Custom data.

**avs\_result** result of the Address Verification Service (AVS).\
Possible result codes can be found in the appendices on “*GatewayAPI*”
documentation

**cvc\_result** result of the CVC (Card Verification Code) check.\
Possible result codes can be found in the appendices on “*GatewayAPI*”
documentation

**eci** Electronic Commerce Indicator (ECI).

**payment\_product** payment product used to complete the transaction.\
Informs about the payment\_method section type.

**payment\_method** See tables below for further details.

**three\_d\_secure** optional element. Result of the 3-D Secure
Authentication.

**eci** the 3-D Secure (3DS) electronic commerce indicator

**enrollment\_status** the enrollment status.

**enrollment\_message** the enrollment message.

**authentication\_status** the authentication status. This field is only
included if payment authentication was attempted and a value was
received.

**authentication\_message** the authentication message. This field is
only included if payment authentication was attempted and a value was
received.

**authentication\_token** this is a value generated by the card issuer
as a token to prove that the cardholder was successfully authenticated.

**xid** a unique transaction identifier that is generated by the payment
server on behalf of the merchant to identify the 3DS transaction.

**fraud\_screening** Result of the fraud screening.

**scoring** total score assigned to the transaction (main risk
indicator).

**result** The overall result of risk assessment returned by the Payment
Gateway.\
\
Value must be a member of the following list.\
\
• pending rules were not checked.\
• accepted transaction accepted.\
• blocked transaction rejected due to system rules.\
• challenged transaction has been marked for review.

**review** The decision made when the overall risk result returns
challenged.\
\
An empty value means no review is required.\
Value must be a member of the following list.\
\
• pending a decision to release or cancel the transaction is pending.\
• allowed the transaction has been released for processing.\
• denied the transaction has been cancelled.

**order** information about the customer and his order.

**id** unique identifier of the order as provided by Merchant.

**date\_created** time when order was created.

**attempts** indicates how many payment attempts have been made for this
order.

**amount** the total order amount (e.g., 150.00). It should be
calculated as a sum of the items purchased, plus the shipping fee (if
present), plus the tax fee (if present).

**shipping** the order shipping fee.

**tax** the order tax fee.

**decimals** decimal precision of the order amount.

**currency** base currency for this order.\
This three-character currency code complies with ISO 4217.

**customer\_id** unique identifier of the customer as provided by
Merchant.

**language** language code of the customer.

> **email** email address of the customer.

### <span id="_Toc243383275" class="anchor"><span id="_Toc248223036" class="anchor"><span id="_Toc253822249" class="anchor"></span></span></span>Response fields specific to the payment product {#response-fields-specific-to-the-payment-product .ListParagraph .Chapter3}

#### Credit Card payments {#credit-card-payments .ListParagraph}

The following table lists and describes the response fields returned for
transactions by credit/debit card.

Field Name Description

**token** transaction state.

**brand** Card brand. (e.g., Visa, MasterCard, American Express,
Maestro).

**pan** Card number (up to 19 characters).\
Note that, due to the PCI DSS security standards, our system has to mask
credit card numbers in any output (e.g., 549619\*\*\*\*\*\*4769).

**card\_holder** Cardholder name.

**card\_expiry\_month** Card expiry month (2 digits).

**card\_expiry\_year** Card expiry year (4 digits).

**issuer** Card issuing bank name.\
Do not rely on this value to remain static over time. Bank names may
change over time due to acquisitions and mergers.

**country** Bank country code where card was issued.\
This two-letter country code complies with ISO 3166-1 (alpha 2).

#### QIWI payments {#qiwi-payments .ListParagraph}

The following table lists and describes the response fields returned for
transactions by VISA QIWI Wallet.

Field Name Description

**user** The Qiwi user's ID, to whom the invoice is issued.\
It is the user's phone number, in international format.\
Example: +79263745223<span id="_Toc243383276" class="anchor"><span
id="_Toc248223037" class="anchor"><span id="_Toc253822250"
class="anchor"></span></span></span>

### Transaction Workflow {#transaction-workflow .ListParagraph .Chapter3}

The HiPay TPP payment gateway can process transactions through many
different acquirers using different payment methods and involving some
anti-fraud checks. All these aspects change the transaction processing
flow significantly for you.

When you activate a server-to-server notification on Hipay TPP, you
receive a response describing the transaction state. Depending on the
transaction state there are five options to action:

Table 5: Transaction states

  -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Transaction state   Description
  ------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  **completed**       if the transaction state is completed you are done.
                      
                      This is the most common case for credit card transaction processing. Almost all credit card acquirers works in that way. Then you have to look into the status fied of the response to know the exact transaction status.

  **pending**         Transaction request was submitted to the acquirer but response is not yet available.

  **declined**        Transaction was processed and was declined by gateway.

  **Error**           Transaction was not processed due to some reasons.
  -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

####  {#section-5 .ListParagraph}

#### XML Response Example {#xml-response-example .ListParagraph}

1.  &lt;?xml version="1.0" encoding="UTF-8"?&gt;

<!-- -->

1.  &lt;notification&gt;

2.  &lt;state&gt;completed&lt;/state&gt;

3.  &lt;reason/&gt;

4.  &lt;test&gt;true&lt;/test&gt;

5.  &lt;mid&gt;00001326581&lt;/mid&gt;

6.  &lt;attempt\_id&gt;1&lt;/attempt\_id&gt;

7.  &lt;authorization\_code&gt;test123&lt;/authorization\_code&gt;

8.  &lt;transaction\_reference&gt;388997073285&lt;/transaction\_reference&gt;

9.  &lt;date\_created&gt;2012-10-14T12:29:51+0000&lt;/date\_created&gt;

10. &lt;date\_updated&gt;2012-10-14T12:29:55+0000&lt;/date\_updated&gt;

11. &lt;date\_authorized&gt;2012-10-14T12:29:54+0000&lt;/date\_authorized&gt;

12. &lt;status&gt;117&lt;/status&gt;

13. &lt;message&gt;Capture Requested&lt;/message&gt;

14. &lt;authorized\_amount&gt;5.00&lt;/authorized\_amount&gt;

15. &lt;captured\_amount&gt;5.00&lt;/captured\_amount&gt;

16. &lt;refunded\_amount&gt;0.00&lt;/refunded\_amount&gt;

17. &lt;decimals&gt;2&lt;/decimals&gt;

18. &lt;currency&gt;EUR&lt;/currency&gt;

19. &lt;ip\_address&gt;83.167.62.196&lt;/ip\_address&gt;

20. &lt;ip\_country&gt;FR&lt;/ip\_country&gt;

21. &lt;device\_id/&gt;

22. &lt;cdata1&gt;&lt;!\[CDATA\[My data 1\]\]&gt;&lt;/cdata1&gt;

23. &lt;cdata2&gt;&lt;!\[CDATA\[My data 2\]\]&gt;&lt;/cdata2&gt;

24. &lt;cdata3&gt;&lt;!\[CDATA\[My data 3\]\]&gt;&lt;/cdata3&gt;

25. &lt;cdata4&gt;&lt;!\[CDATA\[My data 4\]\]&gt;&lt;/cdata4&gt;

26. &lt;avs\_result/&gt;

27. &lt;cvc\_result/&gt;

28. &lt;eci&gt;9&lt;/eci&gt;

29. &lt;payment\_product&gt;visa&lt;/payment\_product&gt;

30. &lt;payment\_method&gt;

31. &lt;token&gt;ce5x096fx6xx05989x170x7x96f94432600491xx&lt;/token&gt;

32. &lt;brand&gt;VISA&lt;/brand&gt;

33. &lt;pan&gt;400000\*\*\*\*\*\*0000&lt;/pan&gt;

34. &lt;card\_holder&gt;Jhon Doe&lt;/card\_holder&gt;

35. &lt;card\_expiry\_month&gt;07&lt;/card\_expiry\_month&gt;

36. &lt;card\_expiry\_year&gt;2015&lt;/card\_expiry\_year&gt;

37. &lt;issuer&gt;MY BANK&lt;/issuer&gt;

38. &lt;country&gt;FR&lt;/country&gt;

39. &lt;/payment\_method&gt;

40. &lt;three\_d\_secure&gt;

41. &lt;eci&gt;5&lt;/eci&gt;

42. &lt;enrollment\_status&gt;Y&lt;/enrollment\_status&gt;

43. &lt;enrollment\_message&gt;Authentication
    > Available&lt;/enrollment\_message&gt;

44. &lt;authentication\_status&gt;Y&lt;/authentication\_status&gt;

45. &lt;authentication\_message&gt;Authentication
    > Successful&lt;/authentication\_message&gt;

46. &lt;authentication\_token&gt;&lt;/authentication\_token&gt;

47. &lt;xid&gt;&lt;/xid&gt;

48. &lt;/three\_d\_secure&gt;

49. &lt;fraud\_screening&gt;

50. &lt;scoring&gt;120&lt;/scoring&gt;

51. &lt;result&gt;accepted&lt;/result&gt;

52. &lt;review/&gt;

53. &lt;/fraud\_screening&gt;

54. &lt;order&gt;

55. &lt;id&gt;1381753783&lt;/id&gt;

56. &lt;date\_created&gt;2012-10-14T12:29:51+0000&lt;/date\_created&gt;

57. &lt;attempts&gt;1&lt;/attempts&gt;

58. &lt;amount&gt;5.00&lt;/amount&gt;

59. &lt;shipping&gt;10.00&lt;/shipping&gt;

60. &lt;tax&gt;0.98&lt;/tax&gt;

61. &lt;decimals&gt;2&lt;/decimals&gt;

62. &lt;currency&gt;EUR&lt;/currency&gt;

63. &lt;customer\_id&gt;UID1381753791&lt;/customer\_id&gt;

64. &lt;language&gt;fr\_FR&lt;/language&gt;

65. &lt;email&gt;customer@mail.com&lt;/email&gt;

66. &lt;/order&gt;

67. &lt;/notification&gt;

#### HTTP POST Response Example {#http-post-response-example .ListParagraph}

1.  state = completed

<!-- -->

1.  reason =

2.  test = false

3.  mid = 00001326581

4.  attempt\_id = 1

5.  authorization\_code = test123

6.  transaction\_reference = 781357613392

7.  date\_created = 2012-10-14T13:10:36+0000

8.  date\_updated = 2012-10-14T13:10:38+0000

9.  date\_authorized = 2012-10-14T13:10:38+0000

10. status = 116

11. message = Authorized

12. authorized\_amount = 5.00

13. captured\_amount = 0.00

14. refunded\_amount = 0.00

15. decimals = 2

16. currency = EUR

17. ip\_address = 83.167.62.196

18. ip\_country = FR

19. device\_id =

20. cdata1 = My data 1

21. cdata2 = My data 2

22. cdata3 = My data 3

23. cdata4 = My data 4

24. avs\_result =

25. cvc\_result =

26. eci = 7

27. payment\_product = visa

28. payment\_method\[token\] = ce5x096fx6xx05989x170x7x96f94432600491xx

29. payment\_method\[brand\] = VISA

30. payment\_method\[pan\] = 400000\*\*\*\*\*\*0000

31. payment\_method\[card\_holder\] = Jhon Doe

32. payment\_method\[card\_expiry\_month\] = 07

33. payment\_method\[card\_expiry\_year\] = 2015

34. payment\_method\[issuer\] = MYBANK

35. payment\_method\[country\] = FR

36. three\_d\_secure\[eci\] = 5

37. three\_d\_secure\[enrollment\_status\] = Y

38. three\_d\_secure\[enrollment\_message\]=Authentication Available

39. three\_d\_secure\[authentication\_status\]=Y

40. three\_d\_secure\[authentication\_message\]=Authentication
    > Successful

41. three\_d\_secure\[authentication\_token\]=

42. three\_d\_secure\[xid\]=

43. fraud\_screening\[scoring\] = 120

44. fraud\_screening\[result\] = accepted

45. fraud\_screening\[review\] =

46. order\[id\] = 1381756231

47. order\[date\_created\] = 2013-10-14T13:10:36+0000

48. order\[attempts\] = 1

49. order\[amount\] = 5.00

50. order\[shipping\] = 10.00

51. order\[tax\] = 0.98

52. order\[decimals\] = 2

53. order\[currency\] = EUR

54. order\[customer\_id\] = UID1381756236

55. order\[language\] = fr\_FR

56. order\[email\] = customer@mail.com

\
<span id="_Toc248223038" class="anchor"><span id="_Toc253822251" class="anchor"></span></span>Signature verification {#signature-verification .ChapterTitle}
====================================================================================================================

It is strongly recommended that you use a signature mechanism to verify
the contents of a request or redirection made to your servers. This will
prevent customers from tampering with the data in the data exchanges
between your servers and our payment system.

A unique signature is sent each time that Hipay contact a merchant URL,
notification or redirection.

### <span id="_Toc248223039" class="anchor"><span id="_Toc253822252" class="anchor"></span></span>Setup {#setup-1 .Chapter3}

First of all you will need to set a Secret Passphrase in your Hipay TPP
back office under “*Integration -&gt; Security Settings -&gt; Secret
Passsphrase*”.

![](images/media/image5.jpg){width="6.688888888888889in"
height="2.015277777777778in"}

This secret passphrase is used to generate a unique character string
(signature) hashed with SHA algorithm. The longer the passphrase the
more secure the signature will be.

### <span id="_Toc248223040" class="anchor"><span id="_Toc253822253" class="anchor"></span></span>Verification {#verification .Chapter3}

**Notification URL**

To the notification URL the signature is sent on the HTTP header under
the “*HTTP\_X\_ALLOPASS\_SIGNATURE*” parameter, to verify it, you just
need so concatenate the passphrase with the POST content of the query.

*Algorithm:*

*SHA Signature = SHA1(Raw POST Data + Secret Passphrase)*

**Redirection URL**

To each redirection page (accept page, decline page, etc.) the signature
is sent under the “*hash*” parameter, to verify it, you must concatenate
the parameters, the values of each and the passphrase under the
following conditions:

a)  The parameter must be predefined.

b)  The value can’t be empty.

c)  The parameter must be sorted in alphabetical order.

*Algorithm:*

a.  paramC = val3

b.  paramA = val1

c.  paramB = val2

*SHA Signature =
SHA1(paramAval1&lt;passphrase&gt;paramBval2&lt;passphrase&gt;paramCval3&lt;passphrase&gt;)*

#### PHP Signature Validation Example {#php-signature-validation-example .ListParagraph}

1.  \$secretPassphrase = 'mypassphrasse'; //Secret Passphrase

<!-- -->

1.  \$string2compute = '';

2.  3.  if (isset(\$\_GET\['hash'\])) { // If is a redirection URL

4.  \$signature = \$\_GET\['hash'\];

5.  \$parameters = \$\_GET;

6.  unset(\$parameters\['hash'\]);

7.  ksort(\$parameters);

8.  foreach (\$parameters as \$name =&gt; \$value) {

9.  if (!empty(\$value)) {

10. \$string2compute .= \$name . \$value . \$secretPassphrase;

11. }

12. }

13. }

14. else { // If is a Notification

15. \$signature = \$\_SERVER\['HTTP\_X\_ALLOPASS\_SIGNATURE'\];

16. \$string2compute = \$HTTP\_RAW\_POST\_DATA . \$secretPassphrase;

17. }

18. \$computedSignature = sha1(\$string2compute);

19. 20. // true if OK, false if not

21. if (\$computedSignature == \$signature) {

22. \$message = 'OK';

23. }

24. else {

25. \$message = 'KO';

26. }


