#HiPay Enterprise Magento 1 - Integration guide

**DISCLAIMER:**
While every effort has been made to ensure the accuracy of the information contained in this publication, the information is supplied without representation or warranty of any kind, is subject to change without notice and does not represent a commitment on the part of HiPay.
HiPay, therefore, assumes no responsibility and shall have no liability,
consequential or otherwise, of any kind arising from this material or
any part thereof, or any supplementary materials subsequently issued by
HiPay. HiPay has made every effort to ensure the accuracy of this
material.

**TECHNICAL SUPPORT** If you need any complementary information concerning the technical implementation of HiPay Enterprise TPP don’t hesitate to go to our support center ([*http://help.hipay.com*](http://help.hipay.com)) or contact our Technical Support team:

- Email: support.tpp@hipay.com
- Telephone: +33 (0)1 53 44 15 07


#About this Guide

##Purpose

This document is designed to provide you details on how to integrate
your business to the HiPay Enterprise TPP payment gateway. This
document provides step-by-step instructions on how to simply and quickly
get up and running with our services as well as detailed reference
material.

Where applicable, this document refers to the related documentation with
further details.

##Intended Audience

The intended audience is the merchant's technical staff or the
merchant's system integrator.

##Copyright

The information contained in this guide is a property of HiPay. This
material shall not be duplicated, published, or disclosed, in whole or
in part, without the prior written permission of HiPay.

##Legal Notice

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

#How to Read this Guide

##Document Conventions

To ease readability and improve understanding, this document uses a
number of conventions.

This guide uses several typographic conventions to highlight certain
words and phrases and draw attention to specific pieces of information.
The following table clarifies the conventions used across this guide.

*Table 1: Typographic conventions that apply across this guide*

| Convention   |      Purpose in this guide      |
|----------|:-------------:|
| Mono-space |  Indicates source code, code examples, input to the command line, application output, code lines embedded in text, and variables and code elements. |
| *Italic* |  Italicized regular text is used for emphasis or to indicate a term that is being defined or will be defined shortly. |
| ... |  Horizontal ellipsis points in sample code indicate the omission of part of the code. This is done when you would normally expect additional code to appear, but such code would not be related to the example. |
| $ |  At the beginning of a command, indicates an operating system shell prompt. |
| &lt;placeholder&gt; |  Indicates placeholders, most often method or function parameters; these placeholders represent information that must be supplied by the implementation or the user. For command-line input, indicates parameter values.|

**Abbreviations used in tables**

The tables of this document describe data elements. These data elements
are equivalent to parameters in a query or to fields of a response
message. The following table helps understanding the different
attributes (columns) that define a data element.

*Table 2: Description of data elements*

| Column   |      Explanation    |
|----------|:-------------:|
| Field Name |  Contains the name of the data element. |
| Format |  The format of the element. |
| Length |  Denotes maximum size of the element.<br/>For example a size of 6 means that the data value cannot exceed six characters.<br/>When no size is specified, it means there is no limitation on the length of the field. |
| Req. |  Specifies whether an element is required or not. <br/>**M** = Mandatory; must always be provided.<br/>**C** = Conditional; requirement depends on value or appearance of other elements. |
| Description |  Provides a brief description of the data element and where applicable, a list of valid values and element dependencies. |

*Table 3: Available formats of data elements*

| Format Abbreviatio   |      Description    |     Example    |
|----------|:-------------:|:-------------:|
| AN |  Alphanumeric characters (a-z, A-Z, 0-9)  ||
| A |  Alphabetic characters only (a-z, A-Z)  ||
| N |  Numerical only ||
| R |  Decimal number with explicit decimal point, signed |12.34|
| DT |  Date in the format YYYY-MM-DD |2016-12-31|
| TM |  Time in the format HH:MM with optional seconds (HH:MM:SS) |15:30|

**Acronyms and Abbreviations**

Acronyms and abbreviations are used in this guide.

The following table shows the acronym or abbreviation and the
corresponding full name.

*Table 4: Acronyms and Abbreviations*

|Acronym or Abbreviation |   Full Name |
|----------|:-------------:|
|  BIN |                       Bank Identification Number
|  PAN  |                     Primary Account Number
|  PCI DSS  |                 Payment Card Industry Data Security Standards
|  REST   |                   Representational State Transfer
|  TPP   |                   Third Party Processing


#Introduction

In this document we describe the method to parameter and use the HiPay Enterprise TPP module for Magento webshop.

##To know

-   You will need to configure your HiPay Enterprise TPP account before
    installing your Magento module under HiPay Enterprise TPP back
    office
    ([*https://merchant.hipay-tpp.com/*](https://merchant.hipay-tpp.com/)).
-   HiPay Enterprise TPP module is not offered natively on Magento, you
    must contact HiPay to get the module.
-   This module needs valid HiPay Enterprise TPP credentials to use it,
    please contact HiPay to get them.


#Platform Configuration

##Allow your servers IP addresses

When a request is sent to the HiPay Enterprise TPP servers, the IP
address or IP address range from where the connection was made is
verified. If it matches with the IP address supplied by the Merchant at
a previous stage, the request will be processed. In the case of missing
or incorrect information, the server will respond with an appropriate
error message, indicating the error in the request.

To do this, you must log in your HiPay Enterprise TPP back office
(https://merchant.hipay-tpp.com), click on the "*Integration*" menu,
then "*Security Setting*" and enter your IP(s) in "*IP restriction*"
section.

![Important](images/media/image5.png)
<br/>**Important!**

When changing your IP address, do not forget to ensure that all new IP addresses are configured for your account. If not, your server requests will be rejected.
  
![ip](images/media/image6.png)

##Choose a passphrase

It is strongly recommended that you use a signature mechanism to verify
the contents of a request or redirection made to your servers. This will
prevent customers from tampering with the data in the data exchanges
between your servers and our payment system.

A unique signature is sent each time that HiPay contact a merchant URL,
notification or redirection.

First of all you will need to set a Secret Passphrase in your HiPay
Enterprise TPP back office under “*Integration -&gt; Security Settings -&gt; Secret Passphrase*”.

![passphrase](images/media/image7.jpg)

##Configure redirection URLs

To use the HiPay Enterprise TPP module, you need to configure the
redirection URLs in your HiPay Enterprise TPP back office under
“*Integration -&gt; Redirect Pages*”.

-   Accept Page: `http://www.{your-domain.com}/index.php/hipay/cc/accept/`
-   Decline Page: `http://www.{your-domain.com}/index.php/hipay/cc/decline/`
-   Pending Page: `http://www.{your-domain.com}/index.php/hipay/cc/pending/`
-   Cancel Page: `http://www.{your-domain.com}/index.php/hipay/cc/cancel/`
-   Exception Page: `Empty`

Also check the box "*Feedback Parameters*" and apply the changes.

Then click on "*Integration*" menu, then "*Notifications*"

-   Notification URL: http://www.{your-domain.com}/index.php/hipay/notify/index
-   Request method: HTTP POST
-   I want to be notified for the following transaction statuses: ALL

Don’t forget to change {your-domain.com} by your own domain and inform
the store code if you have enabled it on your Magento configuration.


#Module Installation

##Magento connect

You can get HiPay Enterprise official Magento extension under
[*http://www.magentocommerce.com/magento-connect/hipay-fullservice-1.html*](http://www.magentocommerce.com/magento-connect/hipay-fullservice-1.html),
to install it, just get the “extension key”, then go to your Magento
administrator back office, click on “*System* -&gt; *Magento connect
-&gt; Magento connect manager*”

Paste the “extension key” under “Install New Extensions” and follow the
steps:

![magento connect](images/media/image8.png)


#Module configuration

##General configuration

To configure your HiPay Enterprise TPP API credentials, you must click
on "*HiPay Enterprise*" in Magento configuration section (*System -&gt;
Configuration -&gt; Sales*). If you have administrative rights but you
are not allowed to access to the configuration of the payment method,
please log out and log in.

![](images/media/image9.jpg)

You can find your HiPay Enterprise TPP API credentials on your HiPay
Enterprise TPP back office, under “*Integration -&gt; Security Settings
-&gt;* *API credentials*”.

Once you have them, fill them on the module configuration with your
“*Passphrase*”. (*Please refer to *2.2* section*).

When using Multi-site or Multi-store : you can use different HiPay
credentials and payment methods for each Store View using the “*Current
Configuration Scope*” selectbox. Uncheck the “Use Website” checkbox and
specify the desired valued.

##Payment Methods configuration

To configure your HiPay Enterprise TPP payment methods, you must click
on "*Payment Methods*" in Magento configuration section (*System -&gt;
Configuration*).

Once you are in payment methods list you will find all the HiPay
Enterprise installation possibilities.

![](images/media/image11.png)

### HiPay Enterprise Credit Card API
(only available for credit-card and debit-card payment methods)

In case of HiPay Enterprise Credit Card API integration (direct API
integration), the customer will fill his bank information directly on
merchants’ website, the module calls HiPay Enterprise TPP API to
validate the transaction and the merchants’ website display the
transaction confirmation / refused / pending message.

If this integration mode is selected, you are required to be compliant
with the PCI Data Security Standard, you can find more information under
“*https://www.pcisecuritystandards.org*”.

### HiPay Enterprise Credit Card Split Payment
(only available for credit-card and debit-card payment methods) 

This integration can be used to process recurring payments and split
order’s payments amount through time using API. It is a variant of the
HiPay Enterprise Credit Card API integration, therefore you are also
required to be compliant with the PCI Data Security Standard.

Prior to activating this integration, at least one recurring profile
must be created (*Please refer to section *5.6* Split payment method*).

### HiPay Enterprise Credit Card Hosted Page
(only available for credit-card and debit-card payment methods)


To process the payment, cardholders are redirected to a secured payment
page hosted by HiPay.

After payment validation customer will be redirected to merchants’
website transaction confirmation / error / pending message.

### IFrame 

You can activate the iFrame mode on your HiPay Enterprise Hosted Page
if you want that cardholders fill their payment card information on a
secured payment page hosted by HiPay displayed on an iFrame inside
merchants’ payment page.

Your website must run with **HTTPS** protocol to use IFrame Hosted
page.

This page (hosted & iFrame) can be customized with merchants’ CSS
stylesheet to fit his website look and feel.

### HiPay Enterprise Hosted Page Split Payment
(only available for credit-card and debit-card payment methods)

This integration can be used to process recurring payments and split
order’s payments amount through time using Hosted Page.

Prior to activating this integration, at least one recurring profile
must be created (*Please refer to section* **5.6** *Split payment
method*).

### HiPay Enterprise other payment methods

If you want to offer on your website other payment methods than
credit-cards or debit-cards you can choose them directly on the payment
methods list, they will be showed as “*HiPay Enterprise {payment
method}*”, for exemple: *HiPay Enterprise Sofort, HiPay Enterprise
Sisal, etc.*

### Configuration parameters

  
|  Name    | Description|
|----------|:-------------:|
|  Enabled    | Allows the activation of the module.
|  Title| Title displayed on the front-end for this payment method.
|Payment Profile | Pofiles allowed if split payment is used.<br/>*Please refer to section* **5.6** *Split payment method.*
|Order status when payment accepted|                Status to be assigned to the command.
|Order status when payment refused|                 Status to be assigned to the command.
|Order status when payment cancelled by customer|   Status to be assigned to the command.
|HiPay status to validate order| The HiPay status when you want that your Magento transaction be validated.
|Redirect page pending status| If transaction result is pending the customer can be redirected to a pending, success or failure page.
|Payment Action| Set the payment mode: *Authorization + Capture* (*Sale*) or *: Authorization Only*.<br/>*Please refer to “HiPayTPP-GatewayAPI documentation, chapter 3.1 Request a New Order – operation”.*<br/>Must be set on “*Sale*” for Split payment method.
|Credit Card Types|                                 Card types allowed in the payment form.
|Display card owner|                                Enable/disable the card holder input field in the payment form.
|Credit Card Verification|                          Enable or disable Magento credit-card verification.
|Css Url| URL to merchants’ stylesheet for iFrame or Hosted page operating modes.<br/>Important, **HTTPS** protocol is required. *See HiPayTPP-GatewayAPI documentation, chapter “3.3 Initialize a hosted payment page (css)”.*
|Page payment template|For iFrame and Hosted page operating modes you can choose your basic template to show:<br/>**Basic:** Basic responsive design.<br/>**Basic-js:** Advanced responsive design.
|Display hosted page in iframe|                     Activate iFrame mode on hosted page. *Please refer to chapter 4.2*
|iFrame Height|                                     If iFrame operating mode is choosed, you can select your iFrame width to fit with your CSS.
|iFrame Width|                                      If iFrame operating mode is choosed, you can select your iFrame height to fit with your CSS.
|Wrapper iFrame Style|                              If iFrame operating mode is choosed, you can customize the wrapper around the iframe with your CSS.
|iFrame Style|                                      If iFrame operating mode is choosed, you can select your iFrame integration style to fit with your CSS.
|Display card selector|                             Enable/disable the payment methods selector on iFrame and Hosted page.
|Enable 3D Secure|                                  Allows the activation of 3D secure if available on used card.
|Rules 3D Secure|                                   If you enabled “*3D Secure with specific rules”*, setup rules to call 3D Secure system only when you really need it with order, customer or product attributes.
|Use Oneclick|                                      Allows the use of Oneclick payment (only for credit cards).
|Rules Oneclick|                                    If Oneclick used, configure Rules to activate Oneclick
|Add product to cart|                               Update the cart when the payment is cancelled or refused.
|Cancel pending order|                              Cancel pending orders over 30 minutes.
|Send fraud payment email|                          Sends an alert to the client if a transaction with the challenge status and requires approval.
|Payment from applicable countries|                 Restricted access by country.
|Payment from Specific countries|                   List of allowed countries.
|Minimum Order Total|                               Minimum amount to display the payment method.
|Maximum Order Total|                               Maximum amount to display the payment method.
|Sort Order|                                        Sort order of the payment method in front-end.
|Enable debug log|                                  Log requests and server responses on the file« var/log/payment\_hipay\_cc.log »
|Enable test mode|                                  Test Mode: If enabled, the module will use the HiPay TPP test platform.
  
#Payment configuration

##“Sale” mode

When making a purchase with the “sale” method, the capture is requested
just after the authorization automatically. *Please refer to
“HiPayTPP-GatewayAPI documentation, chapter 3.1 Request a New Order –
operation”*

If the payment fails, the customer is redirected to the error page and
the status is defined like configured in the module configuration.

If successful, the customer is redirected to the success page, the
invoice is created if the configuration allows it and the status is
defined like this:

-   “capture_waiting”
-   “processing”

##“Authorization” mode 

When making a purchase with the "Authorization" mode, the transaction
will be on “waiting capture”. *Please refer to “HiPayTPP-GatewayAPI
documentation, chapter 3.1 Request a New Order – operation”*

The customer is not charged directly: You have 7 days to "capture" this
order to charge the customer. Otherwise the order will be cancelled.

If the payment fails, the customer is redirected to the error page and
the status is defined like configured in the module configuration.

If successful, the customer is redirected to the success page. At this
time the order status is “*Waiting for capture*” Once you’re ready to
capture the transaction:

-   If the configuration does not allow the creation of invoice, you
    must create the invoice from the order overview. You can click on
    “*Capture online*” This will send the capture information on the
    TPP server. If successful, the invoice is created. The payment is
    captured and you can create your shipment.
-   If the configuration allows the creation of invoice, click on the
    invoice and on “*capture”* button. This will send the capture
    information to HiPay Enterprise TPP server. If successful, the
    invoice status changes to “*Paid*”.

You can also do the “*capture*” directly on your HiPay Enterprise TPP
BackOffice. The invoice will be created automatically on your Magento
BackOffice.

##Refund

HiPay Enterprise TPP allows a refund online. To do this, simply create
a "*Credit memo*" on the Invoice (not from the order).

![refund](images/media/image12.png)
You will have two options: “*Refund Offline*" (we are not interested in
our case) or the "*Refund*".

Choose the amount and click on “*Refund*”. If successful, the credit
memo is created and the refund is validated.

##Oneclick (only available for credit-card payment methods) 

If the OneClick option is activated it will let your system create an
“*alias*” of the credit card. This will let the customer use a saved
credit card in his second transaction and he will no need to refill all
the payment data.

This option will be available only if the customer has created an
account in your site.

##MO/TO Payment (please contact HiPay to create a MO/TO account)

If merchant needs to create a new order and pay it using customers’
financial details received over the phone he can follow this
instructions:

### Configuration:

1.  Create a new Magento Store View called “MO/TO”
	- “*System -&gt; Manage Stores -&gt; Create Store View*”
2.  MO/TO account configuration
    - Under “*System -&gt; Configuration*” choose your “MO/TO” Store
        View and follow Chapter 4 with your MO/TO account credentials.

### Payment:

- Create a new order on your Magento back office
    1.  “*Sales -&gt; Create New Order*”.
    1.  Choose your customer.
    1.  Select your MO/TO Store View.
    1.  Add your products, Account information, Billing address,
        Shipping address, Payment method, Shipping method, etc.
    1.  Submit your order.
    1.  Complete the payment.

##Split payment method

### Payment profile

Prior to activating a split payment method you must create one or more
payment profiles under: *Sales &gt; HiPay &gt; Split Payment Profiles &gt; Add payment profile*

A payment profile defines the billing cycle of an order, for example :

![payment](images/media/image13.png)

### Configuration parameters

|Name|                     Description
|----------|:-------------:|
|Name|                     Title displayed on the front-end for this payment method.
|Billing Period Unit|      Unit of time for billing during the subscription period.
|Billing Frequency|        Number of billing periods that make up one billing cycle.
|Maximum Billing Cycles|   The number of billing cycles for payment period.

Also please note that when configuring your payment method, you must
select “*Sale”* as your “*Payment Action”*

### Scheduling a split payment (Cron)

A daily cron may be setup to launch the “*hipay_pay_split_payment”*
method.

### Manage split payments

An installment plan will be created for each new split payment when the
first payment is successful. You can follow the status and the due date
of each payment under:\
*Sales* *&gt;* *HiPay* *&gt; Split payments*

Status of the Split payments:

-   “*Complete*” payments have been processed and no further action
    is needed.
-   “*Pending*” payments are not processed yet : you may change the date
    or other parameters
-   “*Failed*” payments: you will receive an e-mail when a split payment
    is failed and an action is needed. You can retry the payment by
    switching the status to Pending.

![](images/media/image14.jpg)

### Retry a split payment

On pending or failed split payments, you can change the due date or
force the payment immediately :

![](images/media/image15.png)

#Front-end payment examples

##Payment method selection

![](images/media/image16.png)

##HiPay Enterprise Credit Card API 

![](images/media/image17.png)

##HiPay Enterprise Credit Card Hosted Page with OneClick 

![](images/media/image18.png)

##HiPay Enterprise Hosted Page Split Payment 

![](images/media/image19.png)

##HiPay Enterprise XXXX (other payment methods) 
 
![](images/media/image20.png)

##HiPay Enterprise Hosted Page with IFrame 

![](images/media/image21.jpg)