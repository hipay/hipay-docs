![](images/media/image2.jpeg){width="8.568452537182852in"
height="10.392856517935257in"}

![](images/media/image3.png){width="1.5167082239720036in"
height="0.9784722222222222in"}

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
**If you need any complementary information concerning the technical implementation of HiPay Fullservice TPP don’t hesitate to contact our Technical Support team:\
\
Email: support.tpp@hipay.com\
Telephone: +33 (0) 1 53 44 15 07 {#technical-support-if-you-need-any-complementary-information-concerning-the-technical-implementation-of-hipay-fullservice-tpp-dont-hesitate-to-contact-our-technical-support-team-email-support.tpphipay.com-telephone-33-0-1-53-44-15-07 .En-ttedetabledesmatires}
===================================================================================================================================================================

 {#section .En-ttedetabledesmatires}

 {#section-1 .En-ttedetabledesmatires}

 {#section-2 .En-ttedetabledesmatires}

 {#section-3 .En-ttedetabledesmatires}

 {#section-4 .En-ttedetabledesmatires}

**Table of Contents** {#table-of-contents .En-ttedetabledesmatires}
=====================

About this Guide 4

How to Read this Guide 5

Chapter 1. Working priciple 7

1.1. API Credentials 7

Accounts and sub-accounts 7

1.2. Test mode 7

1.3. Administrator experience 7

1.4. Customer experience 8

Credit card and debit card payment methods 8

Real-Time Banking and other payment methods 8

Chapter 2. Payment Gateway integration modes 9

API mode (*please refer to Appendix B. Payment Products Integration*) 9

API mode transaction flow 9

API mode display exemple 10

Hosted page mode (*please refer to Appendix B. Payment Products
Integration*) 11

Hosted page mode transaction flow 11

Hosted page mode display exemple 12

iFrame mode (only available for credit-card and debit-card payment
methods) 13

iFrame mode transaction flow 13

iFrame mode display exemple 14

Chapter 3. Transactions 15

3.1. Transaction Life Cycle 15

3.2. Final message to customer 17

3.3. Callback **Erreur ! Signet non défini.**

3.4. Challenged transactions 17

3.5. Manual Capture 18

3.6. Refund 18

3.7. Credit card token (Oneclick payment) 19

3.8. Logs 19

3.9. Signature verification 20

3.10. Countries & Currencies 20

3.11. Shopping Cart 20

3.12. Offline transactions 20

3.13. Recurring payments 20

Chapter 4. 3-D Secure 22

4.1. 3-D Secure rules 22

Chapter 5. CMS Back Office 23

5.1. Mockups 23

5.2. Living data 29

5.3. Field details 29

Appendix A. Payment Products Details 33

Appendix B. Payment Products Integration 35

About this Guide {#about-this-guide .ListParagraph .ChapterTitle}
================

**Purpose**

**What this specification does**

This document provides outline functional specifications and
requirements for the development of HiPay TPP module for CMS (Content
management system). Where applicable, this document refers to the
related documentation with further details.

**What this specification does not do**

This is ***not*** a project plan. It is a guide for system architecture
and development, not for phasing, timelines or deliverables. Portent
will provide project scheduling information as necessary.

**Intended Audience**

The intended audience is the agency or developer that will develop the
module for a specified CMS.

Because almost all communication between the CMS system and the REST API
is realized through predefined XML or JSON messages over the Internet
using standard protocols, you will need basic XML/JSON programming
skills and knowledge of HTTP(S). Furthermore, it is recommended that you
are familiar with the basics of tokenization concept.

**Copyright**

The information contained in this guide is propriety of HiPay. This
material shall not be duplicated, published, or disclosed, in whole or
in part, without the prior written permission of HiPay.

**Legal Notice**

This document contains the proprietary and confidential information of
HiPay. Such information may not be used for any unauthorized purpose and
may not be published or disclosed to third parties, in whole or part,
without the express written permission of HiPay. You acknowledge and
agree that this document and all portions thereof, including, but not
limited to, any copyright, trade secret and other intellectual property
rights relating thereto, are and at all times shall remain the sole
property of HiPay and that title and full ownership rights in the
information contained herein and all portions thereof are reserved to
and at all times shall remain with HiPay. You agree to safeguard the
confidentiality of the information contained herein using the same
standard you employ to safeguard your own confidential information of
like kind, but in no event less than a commercially reasonable standard
of care. If you do not agree with the foregoing conditions, you are
required to return this document immediately to HiPay.

<span id="_Toc241310459" class="anchor"><span id="_Toc275705496" class="anchor"></span></span>**How to Read this Guide** {#how-to-read-this-guide .ListParagraph}
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
Working principle {#working-principle .ChapterTitle}
=================

API Credentials  {#api-credentials .Chapter2}
----------------

### Accounts and sub-accounts {#accounts-and-sub-accounts .Chapter3}

Merchant must have the possibility to configure the API credentials of
all his HiPay Fullservice TPP test & production accounts.

Merchant must have the possibility (if CMS allows it) to create multiple
parallel configurations (multisites) and configure on each one the
affected HiPay Fullservice credentials, currencies and payment methods.

Test mode  {#test-mode .Chapter2}
----------

Merchant must have the possibility to call the “test gateway” or the
“production gateway” of each configured HiPay Fullservice account.

Administrator experience {#administrator-experience .Chapter2}
------------------------

The merchant must have the possibility to select one of the different
integration modes of HiPay Fullservice TPP payment gateway on his CMS
administration panel under payment gateways section.

Merchant will have the possibility to select all payment methods wanted
and the module will show them correctly, for example:

-   Merchant selects Visa, MasterCard and Sisal

-   Merchant selects API mode integration

-   Customer will see Visa and MasterCard on API mode

-   Customer will see Sisal on Hosted Page mode (as API mode is only
    available for credit-card and debit-card)

Please refer to *Appendix* for the full detailed list of specifications
for each payment product.

Customer experience {#customer-experience .Chapter2}
-------------------

### Credit card and debit card payment methods {#credit-card-and-debit-card-payment-methods .Chapter3}

The customer will have the possibility to select HiPay Fullservice TPP
payment methods on his checkout page. Credit card and debit-card payment
methods will be displayed with a logo and a short description.

### Real-Time Banking and other payment methods {#real-time-banking-and-other-payment-methods .Chapter3}

Other payment methods different than credit card and debit-card will be
displayed independently, each payment method will have one line
dedicated to it with a logo and a short description.

Please refer to *Appendix* for the full detailed list of specifications
for each payment product.

<span id="_Toc243383271" class="anchor"><span id="_Toc248223032" class="anchor"></span></span>\
Payment Gateway integration modes {#payment-gateway-integration-modes .ChapterTitle}
===============================================================================================

### API mode (*please refer to Appendix B. Payment Products Integration*) {#api-mode-please-refer-to-appendix-b.-payment-products-integration .Chapter3}

In case of API mode integration the customer will fill his bank
information directly on merchants’ website, the module calls HiPay
Fullservice TPP API to validate the transaction and the merchants’
website display the transaction confirmation / refused / pending
message.

If API integration mode is selected, a message must alert the merchant:

“*If you are a merchant that accepts payment cards, you are required to
be compliant with the PCI Data Security Standard. More information
under* https://www.pcisecuritystandards.org/”

**Note:** some payment methods don’t have a cryptogram or expect more
data (name, first name, etc.), please refer to “*Appendix B. Payment
Products Integration*” for the full detailed list of specifications for
each payment product.

Please refer to *“HiPayTPP-Tokenization documentation”* and
“*HiPayTPP-GatewayAPI documentation, chapter Request a New Order ”.*

### API mode transaction flow {#api-mode-transaction-flow .Chapter3}

### \
 {#section-5 .Chapter3}

###  {#section-6 .Chapter3}

### \
 {#section-7 .ListParagraph .Chapter3}

### API mode display example {#api-mode-display-example .Chapter3}

For example only. Look and feel completely free.

![](images/media/image4.jpg){width="6.688888888888889in"
height="7.094444444444444in"}

- *Note:* The payment details fields are included directly on the
merchants’ webpage

### Hosted page mode (*please refer to Appendix B. Payment Products Integration*) {#hosted-page-mode-please-refer-to-appendix-b.-payment-products-integration .Chapter3}

To process the payment, cardholders are redirected to a secured payment
page hosted by HiPay. This page can be personalized with merchants’ CSS
style sheet to fit his website look and feel.

After payment validation customer must be redirected to merchants’
website transaction confirmation / error / pending message.

Payment products list sent to the hosted page must be on the order
chosen by the merchant.

Please refer to “*HiPayTPP-GatewayAPI documentation, chapter Initialize
a hosted payment page”.*

### Hosted page mode transaction flow {#hosted-page-mode-transaction-flow .Chapter3}

### \
 {#section-8 .Chapter3}

 {#section-9 .ListParagraph}

### Hosted page mode display example  {#hosted-page-mode-display-example .Chapter3}

For example only. Look and feel completely free.

![](images/media/image5.jpg){width="6.688888888888889in" height="4.11875in"} {#section-10 .ListParagraph}
--------------------------------------------------------------------------------------------------------------

- *Note:* There must be a full redirection outside merchants’ website.

### iFrame mode (only available for credit-card and debit-card payment methods) {#iframe-mode-only-available-for-credit-card-and-debit-card-payment-methods .Chapter3}

In case of iFrame integration cardholders will fill their payment card
information on a secured payment page hosted by HiPay displayed on an
iFrame inside merchants’ payment page.

This page can be personalized with merchants’ CSS style sheet to fit his
website look and feel.

  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  > **Important!**
  >
  > A particular precaution must be taken if 3-D Secure validation is asked, please refer to *“Chapter 4 – 3-D Secure”.*![](images/media/image6.png){width="0.38333333333333336in" height="0.38333333333333336in"}
  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

After payment validation customer must be redirected to merchants’
website transaction confirmation / error / pending message.

Please refer to “*HiPayTPP-GatewayAPI documentation, chapter 3.3
Initialize a hosted payment page”.*

### iFrame mode transaction flow {#iframe-mode-transaction-flow .Chapter3}

### \
 {#section-11 .ListParagraph .Chapter3}

###  {#section-12 .ListParagraph .Chapter3}

### \
 {#section-13 .Chapter3}

### iFrame mode display example {#iframe-mode-display-example .Chapter3}

For example only. Look and feel completely free.

![](images/media/image7.jpg){width="6.688888888888889in"
height="4.370833333333334in"}

- *Note:* The iFrame must be almost invisible to the customer and be an
API LIKE payment mode.

<span id="_Toc243383273" class="anchor"><span id="_Toc248223034" class="anchor"></span></span>\
Transactions {#transactions .ChapterTitle}
===============================================================================================

Transaction Life Cycle  {#transaction-life-cycle .Chapter2}
-----------------------

The life cycle of a transaction processed by the HiPay Fullservice TPP
Payment Service is characterised by the different events that mark a
change in the status of the transaction.

These events and the resulting changes in transaction status play a
crucial role in the payment process. All financial reporting is based on
the status of transactions and any possible action for a transaction,
whether performed by the merchant, the financial institution or by the
payment system, depends on the actual status.

-   A transaction must be created since the first notification received
    (or before if possible)

-   A transaction must be validated by a “Capture Requested” status
    (status 117)

-   If status 117 doesn’t exists, transaction must be validated by a
    “Captured” status (status 118)

-   HiPay transaction reference must be found in transaction detail
    under the CMS back office.

-   CMS order ID must be found in transaction detail under HiPay
    Fullservice back office.

-   Payment method detail must be found on transaction detail.

-   A link to HiPay Fullservice back office must be added in the CMS
    transaction detail:

    -   https://merchant.hipay-tpp.com/transaction/detail/index/trxid/\[HiPay
        transaction id\]

Please refer to *“HiPayTPP-GatewayAPI”* documentation Appendix B.
*“Payment Status Definitions”.*

The following diagram shows the typical flow of a transaction through
the different main payment statuses.

Figure 1: Typical Transaction Flow with 3D-Secure authentication

![](images/media/image8.jpg){width="5.6090277777777775in"
height="8.895799431321086in"}

Final message to customer {#final-message-to-customer .Chapter2}
-------------------------

The final page, where the customer is redirected after payment must be
on merchants’ server and display one of the following messages with the
transaction ID:

-   Validated payment

    -   If transaction is successfully validated

-   Refused payment

    -   If transaction is refused

-   Pending validation

    -   If transaction is pending cause fraud suspicion
        (challenge status)

-   Payment error

    -   If there was an error in payment treatment

Callback {#callback .Chapter2}
--------

The module must manage ***all the callback notifications*** sent by
HiPay to update the order’s status.

Please refer to *“HiPayTPP-GatewayAPI”* documentation chapter
*“Server-to-Server Notifications”.*

  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  > **Important!**
  >
  > Notifications can be treated by merchant server on a different order than transaction life cycle![](images/media/image6.png){width="0.38333333333333336in" height="0.38333333333333336in"}. *Priorities must exist!*
  >
  > Please refer to *“HiPayTPP-GatewayAPI”* documentation Appendix *“Payment Status Definitions”* for all possible transaction status.
  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Challenged transactions {#challenged-transactions .Chapter2}
-----------------------

If the anti-fraud system blocks a transaction this can be in a
“*Pending*” (Challenge) status.

If this occurs, customer will be redirected to a page specified by the
merchant and the order must be on a “*waiting*” status until the
merchant validates or not the transaction on his HiPay Fullservice TPP
Back Office.

Merchant must have the possibility to send (or not) an automatic email
to inform the customer that the transaction is pending by fraud review.
The merchant must have the possibility to choose the text and template
of this email.

Once the challenged transaction is treated, a callback notification will
be sent to the merchants’ server to update the order status. Customer
must be informed by mail about the validation of his transaction (status
116, 117 or 118) or about the transaction denied (status 111). Merchant
must choose the text and template of this last email (denied
transaction).

Manual Capture {#manual-capture .Chapter2}
--------------

Manual capture option can be selected on the module configuration.

When manual capture is disabled (*payment action: Authorization +
Capture*), the capture is done immediately (operation “sale”).

When manual capture is activated (*payment action: Authorization Only*),
the capture is delayed (operation “authorization”).

Please refer to *HiPayTPP-GatewayAPI documentation, chapter “3.1 Request
a New Order – operation”.*

When a transaction is on “*Authorization Only*” there will be 4 ways to
proceed:

-   **Manual capture:**

    -   The merchant can manually capture the authorized transaction on
        his HiPay Fullservice TPP back office or in his CMS back office.

    -   *Capture can be partial*.

    -   *Multiple partial captures must be possible*.

-   **Automatic delayed capture:**

    -   If merchant has configured on his HiPay Fullservice TPP an
        automatic delayed capture, this capture will be done
        automatically by HiPay Fullservice TPP server (notifications are
        sent for each capture).

-   **Cancel authorization:**

    -   The merchant can “cancel” the authorization on his HiPay
        Fullservice TPP back office or in his CMS back office *before
        the capture requested* if there is any issue with the
        transaction or the product.

-   **Expire authorization:**

    -   An authorization is valid 7 days, after this period, it will be
        expired and no capture will be available. A notification will be
        sent to the server to notify this.

Please refer to “*HiPayTPP-GatewayAPI documentation, chapter 3.2
Maintenance Operations”* for more details.

Refund {#refund .Chapter2}
------

A refund can be made “only” if transaction has already passed the
“Captured” (status 118) mode, before this status refund must not be
possible.

Refund must not be available for all payment methods, *please refer to
Appendix A. Payment Product Details*.

There are 2 refund modes:

1)  Refund can be done on the CMS back office: module calls the API to
    do the refund.

2)  Refund can be done on the TPP back office: a notification is sent to
    the module to change the order status

-   *A refund can be partial*.

-   *Multiple partial refunds must be possible*.

Please refer to “*HiPayTPP-GatewayAPI documentation, chapter Maintenance
Operations”* for more details.

Credit card token (One click payment) {#credit-card-token-one-click-payment .Chapter2}
-------------------------------------

The module can save the « token » of the credit card used to be able to
create new transactions without asking again the bank information to the
customer (only for Visa, MasterCard and American Express cards).

At each payment the customer can choose to save his bank information for
the next payments. (Only for customers registered and logged)

Customer can save one or more credit cards.

The card token and brand are returned on the notification sent to the
merchant (*refer to HiPayTPP-GatewayAPI documentation – Chapter
“Server-to-Server Notifications”*).

3-D Secure authentication will be used on one-click payments only if
merchant selected to in CMS back office.

One-click transactions must be generated using the “order” payment
method.

(*Refer to HiPayTPP-GatewayAPI documentation – Chapter “Request a New
Order”*).

Customer *must have the possibility to delete each one of his saved
payment cards* at any moment through a “Manage Payment Methods page” on
website front office.

Logs {#logs .Chapter2}
----

The merchant can activate the logs (API calls and answers). The file
needs to be secure and not visible by an external URL.

The credit card number should be deleted on the API calls.

There must be a possibility to export the log file on CSV format.

Signature verification {#signature-verification .Chapter2}
----------------------

If the merchant wants to, he can activate the signature verification on
all the HiPay callbacks filling his “passphrase” on the configuration
form. *See HiPayTPP-GatewayAPI documentation, chapter “7 Signature
verification”*

*A different passphrase can be set to each HiPay Fullservice account.*

Countries & Currencies {#countries-currencies .Chapter2}
----------------------

If the CMS allows it, there must be the possibility to configure the
authorized payment methods by country and currencies. (e.g.: BCMC only
proposed for Belgium billing address customers, Sisal only proposed for
Italian billing address customers only, etc.)

Shopping Cart {#shopping-cart .Chapter2}
-------------

If the transaction is refused or not finished the customer MUST retrieve
his shopping cart so he can retry a new payment.

Shopping cart must be emptied only if a transaction is successful.

Offline transactions {#offline-transactions .Chapter2}
--------------------

The merchant must have the possibility to create new transactions and
payments from his CMS back office.

The merchant must have the possibility to create a shopping cart, attach
a customer, fill billing and shipping details and use any of the payment
methods and currencies activated on his website from his CMS back
office.

The merchant can specify a specific HiPay Fullservice account (API
credentials) for offline transactions.

Recurring payments {#recurring-payments .Chapter2}
------------------

The merchant must have the possibility to offer two types of recurring
payments that will be added (if chosen by the merchant) as new payment
methods on the checkout page:

1.  **Split payment:** Split a payment up in two or more transactions
    and capture smaller parts of the amount one at a time. When using
    split payment, the full amount is not authorised, which is
    standard procedure. Instead, at each payment only the amount of the
    part of the total amount will be authorised.\
    - Merchant must choose the frequency and the quantity of parts of
    the split payment method.\
    - Merchant must choose if the first payment is accepted
    automatically or manually after a review.

2.  **Subscriptions:** Merchant must have the possibility to manage
    subscription periods: create, edit and delete them.\
    Merchant will select the title, billing period (day, week, month),
    billing frequency and promotion time and amount (e.g., first week
    free then 10€/month, first month 5€ then 15€month).

Merchant must have the possibility to link a recurring payment method to
a product or list of products.

Merchant must have the possibility to manage existing subscribers (stop
subscription, pause subscriptions).

Merchant must have the possibility to setup a “retry” of refused
transactions, automatically or manually.

Customer must have the possibility to ask for an unsubscription “only”
for subscriptions, never for split payments.

\
3-D Secure {#d-secure .ChapterTitle}
==========

The merchant can activate 3-D Secure for each transaction, to enable
this; the module must propose a set of rules.

If 3-D Secure if activated for a transaction and the customers’ card is
enrolled, the customer must be redirected to an external “**full page**”
to finish the 3DS authentication, independently of the integration mode
chosen by the merchant.

Is the integration mode is “API mode” the URL to forward the customer
will be returned on the answer XML from HiPay.

Please refer to “*HiPayTPP-GatewayAPI documentation, chapter 4 3-D
Secure Integration”* for more details.

 3-D Secure rules {#d-secure-rules .Chapter2}
-----------------

In merchants’ CMS back office there must be the possibility to choose
the configuration to activate 3-D Secure authentication under the
following criteria:

-   By total amount of the order

-   By country

-   By currency

-   By customer profile:

    -   First purchase?

    -   Guest customer?

    -   Billing address different from shipping address?

-   By product type

-   By product ID

-   By time of purchase (according to the date and payments hours.):

    -   Midnight - 8:00 a.m.

    -   8:00 a.m. - 3:00 p.m.

    -   3:00 p.m. - 8:00 p.m.

    -   8:00 p.m. - 11:59 p.m.

-   By shipping method

-   By shopping cart (suspect quantity of articles on shopping cart)

> Following this criteria, the merchant must have the possibility to
> choose between “and” or “or”

-   Example:

    -   Ask 3DS for all the orders “+ 300 EUR” from “midnight to 8:00
        a.m.”

    -   Ask 3DS for all the orders “+ 300 EUR” and all those from
        “midnight to 8:00 a.m.”

\
CMS Back Office {#cms-back-office .ChapterTitle}
===============

Mockups {#mockups .Chapter2}
-------

Following mockups show generic configuration windows, each CMS can have
their own configuration flows if the results are as desired.

These mockups are based on multi-accounts or multi-sites configuration
(*please refer to chapter 1.1 API credentials*).

Payment methods mockups are based on a “one page configuration” base, if
the CMS allows a parallel configuration for each payment method the best
method should be used.

![](images/media/image9.png){width="4.559027777777778in"
height="9.42986111111111in"}

 {#section-14 .ListParagraph .Chapter2}

![](images/media/image10.png){width="6.688888888888889in"
height="8.0625in"}

![](images/media/image11.png){width="6.688744531933509in"
height="9.075in"}

![](images/media/image12.png){width="6.688888888888889in"
height="6.929861111111111in"}

![](images/media/image13.png){width="6.688755468066492in"
height="8.496527777777779in"}

Living data {#living-data .Chapter2}
-----------

The following lists must be available for HiPay to update them at any
time with the following parameters: (e.g. just changing an XML file)

-   Payment products:

    -   HiPay must be able to add or delete payment products from the
        list at any moment.

    -   Payment products must have the following details:

        -   Product name

        -   Product code

        -   Product category

        -   3DS (yes/no)

        -   One click (yes/no)

        -   Currencies list

        -   Countries list

-   Templates

    -   HiPay must be able to add or delete hosted page templates from
        the list at any moment.

Field details {#field-details .Chapter2}
-------------

API Credentials

  Field Name                 Description
  -------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Activated                  Activate or deactivate the payment module on merchants’ CMS.
  Notification URL           URL that the merchant must enter on his HiPay Fullservice TPP back office. Please refer to *“HiPayTPP-GatewayAPI”* documentation chapter *“Server-to-Server Notifications”.*
  IP address of the server   IP address that the merchant must authorize on his HiPay Fullservice TPP back office to call his API. Please refer to *“HiPayTPP-GatewayAPI”* documentation chapter *“1.1 Security considerations”.*
  Production Username        Merchant production account HiPay TPP API username. *See HiPayTPP-GatewayAPI documentation, chapter “2.1 Submitting a request - Authentication”.*
  Production Password        Merchant production account HiPay TPP API password. *See HiPayTPP-GatewayAPI documentation, chapter “2.1 Submitting a request – Authentication”*
  Production Passphrase      Merchant production account HiPay TPP API passphrase. *See HiPayTPP-GatewayAPI documentation, chapter “7 Signature verification”*
  Test account Username      Merchant test account HiPay TPP API username. *See HiPayTPP-GatewayAPI documentation, chapter “2.1 Submitting a request – Authentication*”
  Test account Password      Merchant test account HiPay TPP API password. *See HiPayTPP-GatewayAPI documentation, chapter “2.1 Submitting a request – Authentication”*
  Test account Passphrase    Merchant test account HiPay TPP API passphrase. *See HiPayTPP-GatewayAPI documentation, chapter “7 Signature verification”*
  Merchant display name      The merchant name displayed on hosted mode payment page. *See HiPayTPP-GatewayAPI documentation, chapter “3.3 Initialize a hosted payment page (merchant\_display\_name)”.*
  Platform                   Use test or production platform. *See HiPayTPP-GatewayAPI documentation, chapter “2.1 Submitting a request – Authentication”*

Configuration

  -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Field Name                     Description
  ------------------------------ ----------------------------------------------------------------------------------------------------------------------------------------------------
  Integration mode               See *“Chapter 2 – Payment Gateway integration modes”*

  Display card selector          Enable/disable the payment products selector.
                                 
                                 Possible values:
                                 
                                 0 = No selector available for basic and iframe template
                                 
                                 1 = The selector is displayed
                                 
                                 *See HiPayTPP-GatewayAPI documentation, chapter “3.3 Initialize a hosted payment page (display\_selector)”.*

  Title of card payment method   Title of credit card payment method in checkout page. Local payment methods will show their own name.

  Image of payment method        Image to show on credit card payment method in checkout page. HiPay will provide a default image. Local payment methods will show their own logos.

  Payment action                 Indicates how you want to process the payment. *See HiPayTPP-GatewayAPI documentation, chapter “3.1 Request a New Order – operation”*

  Hosted page / iFrame CSS       URL to customer style sheet.
                                 
                                 Important, **HTTPS** protocol is required. *See HiPayTPP-GatewayAPI documentation, chapter “Initialize a hosted payment page (CSS)”.*

  Hosted page template           The template name.
                                 
                                 Possible values:
                                 
                                 basic-js = Basic template, for a full page redirection.
                                 
                                 iframe-js = Iframe template, dedicated to customer iframe integration.
                                 
                                 *See HiPayTPP-GatewayAPI documentation, chapter “Initialize a hosted payment page (template)”.*

  Use One click                  Activate the use of card tokens. *See chapter 3.6 Credit card token*

  Enable debug log               Activate the use of debug logs. *See chapter 3.7 Logs*
  -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Payment methods

  Field Name   Description
  ------------ ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Position     Order of the payment method to be showed on the checkout page. For credit card payment methods using hosted and iframe integration, the list of methods sent on “payment\_product\_list” must be sent on the desired order. *See HiPayTPP-GatewayAPI documentation, chapter “3.3 Initialize a hosted payment page.*
  Name         Payment method name.
  Status       Payment product activation.
  Currencies   List of available currencies on website. If the payment method if available only in exclusive currencies, only this currencies must be showed. One or more currencies can be selected.
  Countries    List of available countries. If the payment method if available only in exclusive countries, only this countries must be showed.

3-D Secure

  Field Name   Description
  ------------ ------------------------------
  3-D Secure   *See Chapter 4 – 3-D Secure*

Pending transactions

  Field Name                                     Description
  ---------------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------
  Pending transaction redirection                If transaction result is pending *(See chapter 3.3 Challenge status)* the customer can be redirected to a pending, success, failure or specific page.
  Pending transaction customer email: Template   Merchant can choose the desired template for transaction pending email or disable it.
  Pending transaction customer email: Content    Merchant can choose the desired content for transaction pending email.
  Denied transaction customer email: Template    Merchant can choose the desired template for transaction denied email or disable it.
  Denied transaction customer email: Content     Merchant can choose the desired content for transaction denied email or disable it.

####### **\
**<span id="_Toc248223043" class="anchor"><span id="_Toc275705535" class="anchor"></span></span>**Payment Products Details**

Credit Cards (Category code: credit-card)

  Product Code       Brand Name         3-DS   Refund Available?   One click / Recurring Available?   Comment
  ------------------ ------------------ ------ ------------------- ---------------------------------- ---------------------------
  american-express   American Express   No     Yes                 Yes                                
  cb                 Carte Bancaire     Yes    Yes                 Yes                                Available only in France.
  mastercard         MasterCard         Yes    Yes                 Yes                                
  visa               Visa               Yes    Yes                 Yes                                

Debit Cards (Category code: debit-card)

  Product Code   Brand Name                 3-DS     Refund Available?   One click / Recurring Available?   Comment
  -------------- -------------------------- -------- ------------------- ---------------------------------- -------------------------------------------------------------------------------------------------------------------------
  bcmc           Bancontact / Mister Cash   Always   No                  No                                 Available only in Belgium.
  maestro        Maestro                    Yes      Yes                 No                                 MasterCard requires that your website implements 3-D Secure authentication in order for you to accept Maestro payments.

Real-Time Banking (Category code: realtime-banking)

  ------------------------------------------------------------------------------------------------------------------------------------------------
  Product Code         Brand Name           Refund Available?   One click / Recurring Available?   Comment
  -------------------- -------------------- ------------------- ---------------------------------- -----------------------------------------------
  ideal                iDEAL                No                  No                                 

  ing-homepay          ING Home'Pay         No                  No                                 Available only in Belgium.

  sofort-uberweisung   Sofort Überweisung   No                  No                                 

  dexia-directnet      Belfius / Dexia\     No                  No                                 Available only in Belgium.
                       Direct Net                                                                  

  giropay              Giropay              No                  No                                 Available only in Germany.

  przelewy24           Przelewy24           Yes                 No                                 Available only in PLN currency.

  sisal                Sisal                No                  No                                 Available only in Italy. Max amount 1000 EUR.

  bank-transfer        TrustPay Banking     No                  No                                 
  ------------------------------------------------------------------------------------------------------------------------------------------------

E-Wallet (Category code: ewallet)

  Product Code        Brand Name          Refund Available?   One click / Recurring Available?   Comment
  ------------------- ------------------- ------------------- ---------------------------------- ---------------------------------
  qiwi-wallet         VISA QIWI Wallet    No                  No                                 Available only in RUB currency.
  webmoney-transfer   WebMoney Transfer   No                  No                                 Available only in RUB currency.
  yandex              Yandex.Money        No                  No                                 Available only in RUB currency.

####### **\
****Payment Products Integration**

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Product Code         Hosted Page Integration   iFrame Integration   API Integration   APi Integration details                  Currencies                            Countries
  -------------------- ------------------------- -------------------- ----------------- ---------------------------------------- ------------------------------------- ----------------------------------------
  american-express     Yes                       Yes                  Yes               Card num length: 15                      ALL                                   ALL
                                                                                                                                                                       
                                                                                        CVV length: 4                                                                  
                                                                                                                                                                       
                                                                                        firstname and lastame fields mandatory                                         

  bank-transfer        No                        No                   Yes               Direct redirection to forward URL        EUR,CZK,HUF,BGN,RON,HRK,LTL,TRY       SK,CZ,HU,SI,BG,RO,EE,FI,GR,HR,LT,LV,TR

  bcmc                 Yes                       Yes                  Yes               Card num length: 16                      EUR                                   BE
                                                                                                                                                                       
                                                                                        CVV length: No CVV                                                             
                                                                                                                                                                       
                                                                                        3-D Secure mandatory                                                           

  cb                   Yes                       Yes                  Yes               Card num length: 16                      ALL                                   FR
                                                                                                                                                                       
                                                                                        CVV length: 3                                                                  

  dexia-directnet      No                        No                   Yes               Direct redirection to forward URL        EUR                                   BE

  giropay              No                        No                   Yes               Direct redirection to forward URL        EUR                                   DE

  ideal                Yes                       Yes                  No                Hosted page or iFrame mandatory          EUR                                   NL, BE

  ing-homepay          No                        No                   Yes               Direct redirection to forward URL        EUR                                   BE

  maestro              Yes                       Yes                  Yes               Card num length: 16                      ALL                                   ALL
                                                                                                                                                                       
                                                                                        CVV length: 3                                                                  
                                                                                                                                                                       
                                                                                        3-D Secure mandatory                                                           

  mastercard           Yes                       Yes                  Yes               Card num length: 16                      ALL                                   ALL
                                                                                                                                                                       
                                                                                        CVV length: 3                                                                  

  przelewy24           No                        No                   Yes               Direct redirection to forward URL        PLN                                   PL

  qiwi-wallet          Yes                       Yes                  No                Hosted page or iFrame mandatory          RUB,AMD,AZN,BYR,KZT,KGS,UZS,TJS,MDL   RU,AM,AZ,BY,KZ,KG,MD, UZ,TJ

  sisal                No                        No                   Yes               Direct redirection to forward URL        EUR                                   IT

  sofort-uberweisung   No                        No                   Yes               Direct redirection to forward URL        EUR, CHF, GBP                         AU,BE,DE,HU,IT,NL,PL,SK,ES,CH,GB

  visa                 Yes                       Yes                  Yes               Card num length: 16                      ALL                                   ALL
                                                                                                                                                                       
                                                                                        CVV length: 3                                                                  

  webmoney-transfer    No                        No                   Yes               Direct redirection to forward URL        RUB,AMD,AZN,BYR,KZT,KGS,UZS,TJS,MDL   RU,AM,AZ,BY,KZ,KG,MD, UZ,TJ

  yandex               No                        No                   Yes               Direct redirection to forward URL        RUB,AMD,AZN,BYR,KZT,KGS,UZS,TJS,MDL   RU,AM,AZ,BY,KZ,KG,MD, UZ,TJ
  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


