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

Chapter 1. Hosted Payment Page 7

1.1. Request a new Order on a Hosted Payment Page 7

Payment page Workflow 7

Order on a Hosted Payment Page Parameters 7

Customer Parameters 12

Response Fields 13

Payment page example: 15

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

<span id="_Toc241310459" class="anchor"><span id="_Toc253404951" class="anchor"></span></span>**How to Read this Guide** {#how-to-read-this-guide .ListParagraph}
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
Hosted Payment Page {#hosted-payment-page .ChapterTitle}
===================

At the time of payment, cardholders are redirected to a secured payment
page hosted by HiPay TPP. This page can be personalized with merchants’
CSS stylesheet to fit his website look and feel.

Please refer to “*HiPayTPP-GatewayAPI*” documentation for a complete and
detailed guide about all HiPay TPP API possibilities.

<span id="_Ref247964875" class="anchor"><span id="_Toc253404953" class="anchor"></span></span>Request a new Order on a Hosted Payment Page {#request-a-new-order-on-a-hosted-payment-page .Chapter2}
------------------------------------------------------------------------------------------------------------------------------------------

To perform a hosted payment page, first step consist to make an HTTP
POST request to the following resource:

POST /rest/v1/hpayment

### Payment page Workflow {#payment-page-workflow .ListParagraph .Chapter3}

This resource creates an order and returns a forward URL. This forward
URL is dedicated to display a payment page with customers’ CSS and
validated payment products. After payment form validation, the checkout
is processed.

According to transaction state and “*authentication\_indicator*”
parameter (*please refer to “3-D Secure” chapter on “GatewayAPI”
documentation* ), main window is redirect to accept or decline page.

### Order on a Hosted Payment Page Parameters {#order-on-a-hosted-payment-page-parameters .ListParagraph .Chapter3}

+----------------+----------------+----------------+----------------+----------------+
| Field Name     | Format[^1]     | Length         | Req.[^2]       | Description    |
+================+================+================+================+================+
| orderid        | AN             | 32             | M              | Unique order   |
|                |                |                |                | id             |
+----------------+----------------+----------------+----------------+----------------+
| operation      | AN             | 32             | M              | Transaction    |
|                |                |                |                | type.          |
|                |                |                |                |                |
|                |                |                |                | Indicates how  |
|                |                |                |                | you want to    |
|                |                |                |                | process the    |
|                |                |                |                | payment. The   |
|                |                |                |                | default        |
|                |                |                |                | transaction    |
|                |                |                |                | type is set in |
|                |                |                |                | the Merchant   |
|                |                |                |                | Interface      |
|                |                |                |                | (*Default      |
|                |                |                |                | payment        |
|                |                |                |                | procedure* in  |
|                |                |                |                | the            |
|                |                |                |                | *Integration   |
|                |                |                |                | section*). A   |
|                |                |                |                | transaction    |
|                |                |                |                | type sent      |
|                |                |                |                | along with the |
|                |                |                |                | transaction,   |
|                |                |                |                | will overwrite |
|                |                |                |                | the default    |
|                |                |                |                | payment        |
|                |                |                |                | procedure.     |
|                |                |                |                |                |
|                |                |                |                | -   **Sale**   |
|                |                |                |                |     > indicate |
|                |                |                |                | s              |
|                |                |                |                |     > transact |
|                |                |                |                | ion            |
|                |                |                |                |     > is sent  |
|                |                |                |                |     > for      |
|                |                |                |                |     > authoriz |
|                |                |                |                | ation,         |
|                |                |                |                |     > and if   |
|                |                |                |                |     > approved |
|                |                |                |                | ,              |
|                |                |                |                |     > is       |
|                |                |                |                |     > automati |
|                |                |                |                | cally          |
|                |                |                |                |     > submitte |
|                |                |                |                | d              |
|                |                |                |                |     > for capt |
|                |                |                |                | ure.           |
|                |                |                |                |                |
|                |                |                |                | -   **Authoriz |
|                |                |                |                | ation**        |
|                |                |                |                |     > indicate |
|                |                |                |                | s              |
|                |                |                |                |     > this     |
|                |                |                |                |     > transact |
|                |                |                |                | ion            |
|                |                |                |                |     > is sent  |
|                |                |                |                |     > for      |
|                |                |                |                |     > authoriz |
|                |                |                |                | ation only.    |
|                |                |                |                |     > The      |
|                |                |                |                |     > transact |
|                |                |                |                | ion            |
|                |                |                |                |     > will not |
|                |                |                |                |     > be sent  |
|                |                |                |                |     > for      |
|                |                |                |                |     > settleme |
|                |                |                |                | nt             |
|                |                |                |                |     > until    |
|                |                |                |                |     > the      |
|                |                |                |                |     > transact |
|                |                |                |                | ion            |
|                |                |                |                |     > is       |
|                |                |                |                |     > submitte |
|                |                |                |                | d              |
|                |                |                |                |     > for      |
|                |                |                |                |     > capture  |
|                |                |                |                |     > manually |
|                |                |                |                |     > by the   |
|                |                |                |                |     > Merchant |
+----------------+----------------+----------------+----------------+----------------+
| eci            | N              | 1              |                | Electronic     |
|                |                |                |                | Commerce       |
|                |                |                |                | Indicator      |
|                |                |                |                | (ECI).         |
|                |                |                |                |                |
|                |                |                |                | The ECI        |
|                |                |                |                | indicates the  |
|                |                |                |                | security level |
|                |                |                |                | at which the   |
|                |                |                |                | payment        |
|                |                |                |                | information is |
|                |                |                |                | processed      |
|                |                |                |                | between the    |
|                |                |                |                | cardholder and |
|                |                |                |                | merchant.      |
|                |                |                |                |                |
|                |                |                |                | Possible       |
|                |                |                |                | values:        |
|                |                |                |                |                |
|                |                |                |                | 1 = MO/TO      |
|                |                |                |                | (Card Not      |
|                |                |                |                | Present)\      |
|                |                |                |                | 2 = MO/TO -    |
|                |                |                |                | Recurring\     |
|                |                |                |                | 3 =            |
|                |                |                |                | Installment    |
|                |                |                |                | Payment\       |
|                |                |                |                | 4 = Manually   |
|                |                |                |                | Keyed (Card    |
|                |                |                |                | Present)\      |
|                |                |                |                | 7 = E-commerce |
|                |                |                |                | with SSL/TLS   |
|                |                |                |                | Encryption\    |
|                |                |                |                | 9 = Recurring  |
|                |                |                |                | E-commerce     |
|                |                |                |                |                |
|                |                |                |                | A default ECI  |
|                |                |                |                | value can be   |
|                |                |                |                | set in the     |
|                |                |                |                | preferences    |
|                |                |                |                | page. An ECI   |
|                |                |                |                | value sent     |
|                |                |                |                | along in the   |
|                |                |                |                | transaction,   |
|                |                |                |                | will overwrite |
|                |                |                |                | the default    |
|                |                |                |                | ECI value.     |
|                |                |                |                | Refer to the   |
|                |                |                |                | appendices —   |
|                |                |                |                | "*Electronic   |
|                |                |                |                | Commerce       |
|                |                |                |                | Indicator*” —  |
|                |                |                |                | on             |
|                |                |                |                | “*GatewayAPI*” |
|                |                |                |                | documentation  |
|                |                |                |                | to get further |
|                |                |                |                | information.   |
+----------------+----------------+----------------+----------------+----------------+
| authentication | N              | 1              |                | Indicates if   |
| \_indicator    |                |                |                | the 3-D Secure |
|                |                |                |                | authentication |
|                |                |                |                | should be      |
|                |                |                |                | performed for  |
|                |                |                |                | this           |
|                |                |                |                | transaction.   |
|                |                |                |                | Can be used to |
|                |                |                |                | overrule the   |
|                |                |                |                | merchant level |
|                |                |                |                | configuration. |
|                |                |                |                |                |
|                |                |                |                | 0 = Bypass     |
|                |                |                |                | authentication |
|                |                |                |                | \              |
|                |                |                |                | 1 = Continue   |
|                |                |                |                | if possible    |
|                |                |                |                | (Default)      |
+----------------+----------------+----------------+----------------+----------------+
| payment\_produ | AN             | 255            |                | The payment    |
| ct\_list       |                |                |                | product list   |
|                |                |                |                | separated by a |
|                |                |                |                | “,” (e.g.,     |
|                |                |                |                | *visa*,*master |
|                |                |                |                | card*,*america |
|                |                |                |                | n-express*).   |
|                |                |                |                |                |
|                |                |                |                | Refer to the   |
|                |                |                |                | appendices —   |
|                |                |                |                | "*Appendix A.* |
|                |                |                |                | *Payment       |
|                |                |                |                | Products*” —   |
|                |                |                |                | on             |
|                |                |                |                | “*GatewayAPI*” |
|                |                |                |                | documentation  |
|                |                |                |                | for the full   |
|                |                |                |                | list of        |
|                |                |                |                | available      |
|                |                |                |                | payment        |
|                |                |                |                | products.      |
+----------------+----------------+----------------+----------------+----------------+
| payment\_produ | AN             | 255            |                | The payment    |
| ct\_category\_ |                |                |                | product        |
| list           |                |                |                | category list  |
|                |                |                |                | separated by   |
|                |                |                |                | “,”.(e.g.,     |
|                |                |                |                | *credit-card,e |
|                |                |                |                | wallet*).      |
|                |                |                |                |                |
|                |                |                |                | Refer to the   |
|                |                |                |                | appendices —   |
|                |                |                |                | "*Appendix A.* |
|                |                |                |                | *Payment       |
|                |                |                |                | Products*” —   |
|                |                |                |                | on             |
|                |                |                |                | “*GatewayAPI*” |
|                |                |                |                | documentation  |
|                |                |                |                | for the full   |
|                |                |                |                | list of        |
|                |                |                |                | available      |
|                |                |                |                | payment        |
|                |                |                |                | categories.    |
+----------------+----------------+----------------+----------------+----------------+
| css            | AN             | 255            |                | URL to         |
|                |                |                |                | merchant       |
|                |                |                |                | stylesheet.    |
|                |                |                |                | Important:     |
|                |                |                |                | **HTTPS**      |
|                |                |                |                | protocol is    |
|                |                |                |                | required.      |
+----------------+----------------+----------------+----------------+----------------+
| template       | AN             | 32             |                | The template   |
|                |                |                |                | name.          |
|                |                |                |                |                |
|                |                |                |                | Possible       |
|                |                |                |                | values:        |
|                |                |                |                |                |
|                |                |                |                | basic = Basic  |
|                |                |                |                | template, for  |
|                |                |                |                | a full page    |
|                |                |                |                | redirection.   |
|                |                |                |                |                |
|                |                |                |                | basic-js =     |
|                |                |                |                | Advanced JS    |
|                |                |                |                | template, for  |
|                |                |                |                | a full page    |
|                |                |                |                | redirection.   |
|                |                |                |                |                |
|                |                |                |                | iframe =       |
|                |                |                |                | Iframe         |
|                |                |                |                | template,      |
|                |                |                |                | dedicated to   |
|                |                |                |                | customer       |
|                |                |                |                | iframe         |
|                |                |                |                | integration.   |
|                |                |                |                |                |
|                |                |                |                | iframe-js =    |
|                |                |                |                | Advanced JS    |
|                |                |                |                | iframe         |
|                |                |                |                | template,      |
|                |                |                |                | dedicated to   |
|                |                |                |                | customer       |
|                |                |                |                | iframe         |
|                |                |                |                | integration.   |
+----------------+----------------+----------------+----------------+----------------+
| merchant\_disp | AN             | 32             |                | The merchant   |
| lay\_name      |                |                |                | name displayed |
|                |                |                |                | on payment     |
|                |                |                |                | page,          |
|                |                |                |                | otherwise the  |
|                |                |                |                | name is        |
|                |                |                |                | retrieved from |
|                |                |                |                | order.         |
+----------------+----------------+----------------+----------------+----------------+
| display\_selec | N              | 1              |                | Enable/disable |
| tor            |                |                |                | the payment    |
|                |                |                |                | products       |
|                |                |                |                | selector.      |
|                |                |                |                |                |
|                |                |                |                | Possible       |
|                |                |                |                | values:        |
|                |                |                |                |                |
|                |                |                |                | 0 = The        |
|                |                |                |                | selector is    |
|                |                |                |                | not displayed  |
|                |                |                |                |                |
|                |                |                |                | 1 = The        |
|                |                |                |                | selector is    |
|                |                |                |                | displayed      |
+----------------+----------------+----------------+----------------+----------------+
| multi\_use     | N              | 1              |                | Indicates the  |
|                |                |                |                | tokenization   |
|                |                |                |                | module if the  |
|                |                |                |                | credit card    |
|                |                |                |                | token should   |
|                |                |                |                | be generated   |
|                |                |                |                | either for a   |
|                |                |                |                | single-use or  |
|                |                |                |                | a multi-use.   |
|                |                |                |                |                |
|                |                |                |                | Possible       |
|                |                |                |                | values:        |
|                |                |                |                |                |
|                |                |                |                | 1 = Generate a |
|                |                |                |                | multi-use      |
|                |                |                |                | token          |
|                |                |                |                |                |
|                |                |                |                | 0 = Generates  |
|                |                |                |                | a single-use   |
|                |                |                |                | token          |
|                |                |                |                |                |
|                |                |                |                | While a        |
|                |                |                |                | single-use     |
|                |                |                |                | token is       |
|                |                |                |                | typically      |
|                |                |                |                | generated for  |
|                |                |                |                | a short time   |
|                |                |                |                | and for        |
|                |                |                |                | processing a   |
|                |                |                |                | single         |
|                |                |                |                | transaction,   |
|                |                |                |                | multi-use      |
|                |                |                |                | tokens are     |
|                |                |                |                | generally      |
|                |                |                |                | generated for  |
|                |                |                |                | recurrent      |
|                |                |                |                | payments.      |
+----------------+----------------+----------------+----------------+----------------+
| description    | AN             | 255            | M              | The order      |
|                |                |                |                | short          |
|                |                |                |                | description.   |
+----------------+----------------+----------------+----------------+----------------+
| long\_descript | AN             |                |                | Additional     |
| ion            |                |                |                | order          |
|                |                |                |                | description.   |
+----------------+----------------+----------------+----------------+----------------+
| currency       | A              | 3              | M              | Base currency  |
|                |                |                |                | for this order |
|                |                |                |                | (Default to    |
|                |                |                |                | EUR).          |
|                |                |                |                |                |
|                |                |                |                | This           |
|                |                |                |                | three-characte |
|                |                |                |                | r              |
|                |                |                |                | currency code  |
|                |                |                |                | complies with  |
|                |                |                |                | ISO 4217.      |
+----------------+----------------+----------------+----------------+----------------+
| amount         | R              |                | M              | The total      |
|                |                |                |                | order amount.  |
|                |                |                |                | It should be   |
|                |                |                |                | calculated as  |
|                |                |                |                | a sum of the   |
|                |                |                |                | items          |
|                |                |                |                | purchased,     |
|                |                |                |                | plus the       |
|                |                |                |                | shipping fee   |
|                |                |                |                | (if present),  |
|                |                |                |                | plus the tax   |
|                |                |                |                | fee (if        |
|                |                |                |                | present).      |
+----------------+----------------+----------------+----------------+----------------+
| shipping       | R              |                |                | The order      |
|                |                |                |                | shipping fee   |
|                |                |                |                | (Default to    |
|                |                |                |                | zero).         |
|                |                |                |                |                |
|                |                |                |                | It can be      |
|                |                |                |                | omitted if the |
|                |                |                |                | shipping fee   |
|                |                |                |                | value is zero. |
+----------------+----------------+----------------+----------------+----------------+
| tax            | R              |                |                | The order tax  |
|                |                |                |                | fee (Default   |
|                |                |                |                | to zero).      |
|                |                |                |                |                |
|                |                |                |                | It can be      |
|                |                |                |                | omitted if the |
|                |                |                |                | order tax      |
|                |                |                |                | value is zero. |
+----------------+----------------+----------------+----------------+----------------+
| cid            | AN             |                | M              | Unique         |
|                |                |                |                | customer id.   |
|                |                |                |                |                |
|                |                |                |                | *For fraud     |
|                |                |                |                | detection      |
|                |                |                |                | reasons.*      |
+----------------+----------------+----------------+----------------+----------------+
| ipaddr         | AN             |                | M              | The IP address |
|                |                |                |                | of your        |
|                |                |                |                | customer       |
|                |                |                |                | making a       |
|                |                |                |                | purchase.      |
+----------------+----------------+----------------+----------------+----------------+
| accept\_url    | AN             |                | M              | The URL to     |
|                |                |                |                | return your    |
|                |                |                |                | customer to    |
|                |                |                |                | once the       |
|                |                |                |                | payment        |
|                |                |                |                | process is     |
|                |                |                |                | completed      |
|                |                |                |                | successfully.  |
+----------------+----------------+----------------+----------------+----------------+
| decline\_url   | AN             |                | M              | The URL to     |
|                |                |                |                | return your    |
|                |                |                |                | customer to    |
|                |                |                |                | after the      |
|                |                |                |                | acquirer       |
|                |                |                |                | declines the   |
|                |                |                |                | payment.       |
+----------------+----------------+----------------+----------------+----------------+
| pending\_url   | AN             |                | M              | The URL to     |
|                |                |                |                | return your    |
|                |                |                |                | customer to    |
|                |                |                |                | when the       |
|                |                |                |                | payment        |
|                |                |                |                | request was    |
|                |                |                |                | submitted to   |
|                |                |                |                | the acquirer   |
|                |                |                |                | but response   |
|                |                |                |                | is not yet     |
|                |                |                |                | available.     |
+----------------+----------------+----------------+----------------+----------------+
| exception\_url | AN             |                | M              | The URL to     |
|                |                |                |                | return your    |
|                |                |                |                | customer to    |
|                |                |                |                | after a system |
|                |                |                |                | failure.       |
+----------------+----------------+----------------+----------------+----------------+
| cancel\_url    | AN             |                | M              | The URL to     |
|                |                |                |                | return your    |
|                |                |                |                | customer to    |
|                |                |                |                | when he or her |
|                |                |                |                | decides to     |
|                |                |                |                | abort the      |
|                |                |                |                | payment.       |
+----------------+----------------+----------------+----------------+----------------+
| http\_accept   | AN             |                |                | This element   |
|                |                |                |                | should contain |
|                |                |                |                | the exact      |
|                |                |                |                | content of the |
|                |                |                |                | HTTP "Accept"  |
|                |                |                |                | header as sent |
|                |                |                |                | to the         |
|                |                |                |                | merchant from  |
|                |                |                |                | the customer's |
|                |                |                |                | browser        |
|                |                |                |                | (Default to    |
|                |                |                |                | "\*/\*").      |
+----------------+----------------+----------------+----------------+----------------+
| http\_user\_ag | AN             |                |                | This element   |
| ent            |                |                |                | should contain |
|                |                |                |                | the exact      |
|                |                |                |                | content of the |
|                |                |                |                | HTTP           |
|                |                |                |                | "User-Agent"   |
|                |                |                |                | header as sent |
|                |                |                |                | to the         |
|                |                |                |                | merchant from  |
|                |                |                |                | the customer's |
|                |                |                |                | browser        |
|                |                |                |                | (Default to    |
|                |                |                |                | "Mozilla/4.0   |
|                |                |                |                | (compatible;   |
|                |                |                |                | MSIE 6.0;      |
|                |                |                |                | Windows NT     |
|                |                |                |                | 5.0)").        |
+----------------+----------------+----------------+----------------+----------------+
| device\_finger | AN             |                |                | This element   |
| print          |                |                |                | should contain |
|                |                |                |                | the value of   |
|                |                |                |                | the “ioBB”     |
|                |                |                |                | hidden field.  |
|                |                |                |                | (Refer to — "  |
|                |                |                |                | *Device        |
|                |                |                |                | fingerprint    |
|                |                |                |                | integration*”  |
|                |                |                |                | — on           |
|                |                |                |                | “*GatewayAPI*” |
|                |                |                |                | documentation. |
+----------------+----------------+----------------+----------------+----------------+
| language       | AN             |                |                | Locale code of |
|                |                |                |                | your customer  |
|                |                |                |                | (Default to    |
|                |                |                |                | **en\_GB** –   |
|                |                |                |                | English –      |
|                |                |                |                | Great          |
|                |                |                |                | Britain).      |
|                |                |                |                |                |
|                |                |                |                | This will be   |
|                |                |                |                | used to        |
|                |                |                |                | display        |
|                |                |                |                | payment page   |
|                |                |                |                | in correct     |
|                |                |                |                | language.      |
|                |                |                |                |                |
|                |                |                |                | Examples:      |
|                |                |                |                |                |
|                |                |                |                | -   **en\_GB** |
|                |                |                |                |                |
|                |                |                |                | -   **fr\_FR** |
|                |                |                |                |                |
|                |                |                |                | -   **es\_ES   |
|                |                |                |                |     > **       |
|                |                |                |                |                |
|                |                |                |                | -   **it\_IT   |
|                |                |                |                |     > **       |
|                |                |                |                |                |
|                |                |                |                | -   …          |
+----------------+----------------+----------------+----------------+----------------+
| cdata1\        | AN             |                |                | Custom data.   |
| cdata2\        |                |                |                | You may use    |
| cdata3\        |                |                |                | these          |
| cdata4         |                |                |                | parameters to  |
|                |                |                |                | submit values  |
|                |                |                |                | you wish to    |
|                |                |                |                | receive back   |
|                |                |                |                | in the API     |
|                |                |                |                | response       |
|                |                |                |                | messages or in |
|                |                |                |                | the            |
|                |                |                |                | notifications, |
|                |                |                |                | e.g. you can   |
|                |                |                |                | use these      |
|                |                |                |                | parameters to  |
|                |                |                |                | get back       |
|                |                |                |                | session data,  |
|                |                |                |                | user info,     |
|                |                |                |                | etc.           |
+----------------+----------------+----------------+----------------+----------------+

### Customer Parameters {#customer-parameters .ListParagraph .Chapter3}

The merchant can/must send the following customer information along with
the transaction details.

Table 5: Customer-related parameters

+----------------+----------------+----------------+----------------+----------------+
| Field Name     | Format[^3]     | Length         | Req.[^4]       | Description    |
+================+================+================+================+================+
| email          | AN             |                | M              | The customer's |
|                |                |                |                | e-mail         |
|                |                |                |                | address.       |
+----------------+----------------+----------------+----------------+----------------+
| phone          | AN             |                |                | The customer's |
|                |                |                |                | phone number.  |
+----------------+----------------+----------------+----------------+----------------+
| birthdate      | N              | 8              |                | Birth date of  |
|                |                |                |                | the customer   |
|                |                |                |                | (YYYYMMDD).    |
|                |                |                |                |                |
|                |                |                |                | *For fraud     |
|                |                |                |                | detection      |
|                |                |                |                | reasons.*      |
+----------------+----------------+----------------+----------------+----------------+
| gender         | A              | 1              |                | Gender of the  |
|                |                |                |                | customer       |
|                |                |                |                | (M=male,       |
|                |                |                |                | F=female,      |
|                |                |                |                | U=unknown).    |
+----------------+----------------+----------------+----------------+----------------+
| firstname      | AN             |                | M              | The customer's |
|                |                |                |                | first name.    |
+----------------+----------------+----------------+----------------+----------------+
| lastname       | AN             |                | M              | The customer's |
|                |                |                |                | last name.     |
+----------------+----------------+----------------+----------------+----------------+
| recipientinfo  | AN             |                |                | Additional     |
|                |                |                |                | information    |
|                |                |                |                | about the      |
|                |                |                |                | customer       |
|                |                |                |                | (e.g., quality |
|                |                |                |                | or function,   |
|                |                |                |                | company name,  |
|                |                |                |                | department,    |
|                |                |                |                | etc.).         |
+----------------+----------------+----------------+----------------+----------------+
| streetaddress  | AN             |                |                | Street address |
|                |                |                |                | of the         |
|                |                |                |                | customer.      |
+----------------+----------------+----------------+----------------+----------------+
| streetaddress2 | AN             |                |                | Additional     |
|                |                |                |                | address        |
|                |                |                |                | information of |
|                |                |                |                | the customer   |
|                |                |                |                | (e.g.,         |
|                |                |                |                | building,      |
|                |                |                |                | floor, flat,   |
|                |                |                |                | etc.).         |
+----------------+----------------+----------------+----------------+----------------+
| city           | AN             |                |                | The customer's |
|                |                |                |                | city.          |
+----------------+----------------+----------------+----------------+----------------+
| state          | AN             |                |                | The USA state  |
|                |                |                |                | or the Canada  |
|                |                |                |                | state of the   |
|                |                |                |                | customer       |
|                |                |                |                | making the     |
|                |                |                |                | purchase. Send |
|                |                |                |                | this           |
|                |                |                |                | information    |
|                |                |                |                | only if the    |
|                |                |                |                | address        |
|                |                |                |                | country of the |
|                |                |                |                | customer is US |
|                |                |                |                | (USA) or CA    |
|                |                |                |                | (Canada).      |
+----------------+----------------+----------------+----------------+----------------+
| zipcode        | AN             |                |                | The zip or     |
|                |                |                |                | postal code of |
|                |                |                |                | the customer.  |
+----------------+----------------+----------------+----------------+----------------+
| country        | A              | 2              | M              | The country    |
|                |                |                |                | code of the    |
|                |                |                |                | customer.\     |
|                |                |                |                | This           |
|                |                |                |                | two-letter     |
|                |                |                |                | country code   |
|                |                |                |                | complies with  |
|                |                |                |                | ISO 3166-1     |
|                |                |                |                | (alpha 2).     |
+----------------+----------------+----------------+----------------+----------------+

Table 6: Parameters specific to shipping information

  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Field Name               Format   Length   Description
  ------------------------ -------- -------- ---------------------------------------------------------------------------------------------------------------------------------------------------
  shipto\_firstname        AN                The first name of the order recipient.

  shipto\_lastname         AN                The last name of the order recipient.

  shipto\_recipientinfo    AN                Additional information about the order recipient (e.g., quality or function, company name, department, etc.).

  shipto\_streetaddress    AN                Street address to which the order is to be shipped.

  shipto\_streetaddress2   AN                The additional information about address to which the order is to be shipped (e.g., building, floor, flat, etc.).

  shipto\_city             AN                The city to which the order is to be shipped.

  shipto\_state            AN                The USA state or Canada state to which the order is being shipped. Send this information only if the shipping country is US (USA) or CA (Canada).

  shipto\_zipcode          AN                The zip or postal code to which the order is being shipped.

  shipto\_country          A        2        Country code to which the order is being shipped.\
                                             This two-letter country code complies with ISO 3166-1 (alpha 2).
  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Response Fields  {#response-fields .ListParagraph .Chapter3}

The following table lists and describes the response fields.

Field Name Description

**forwardUrl *(json)***

**forward\_url *(xml)*** The hosted payment page URL.

**test** true if the transaction is a testing transaction, otherwise
false.

**mid** your merchant account number (issued to you by HiPay TPP).

**cdata1** Custom data.

**cdata2** Custom data.

**cdata3** Custom data.

**cdata4** Custom data.

**order** information about the customer and his order.

**id** unique identifier of the order as provided by Merchant.

**dateCreated *(json)***

**date\_created *(xml)*** time when order was created.

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

**customerId *(json)***

**customer\_id *(xml)*** unique identifier of the customer as provided
by Merchant.

**language** language code of the customer.

**email** email address of the customer.

### \
 {#section-5 .Chapter3}

### ![](images/media/image4.jpeg){width="5.75625in" height="3.348611111111111in"} Payment page example: {#payment-page-example .ListParagraph .Chapter3}

###  {#section-6 .ListParagraph .Chapter3}

[^1]: The format of the element. Refer to "Table 3: Available formats of
    data elements” for the list of available formats.

[^2]: Specifies whether an element is required or not.

[^3]: The format of the element. Refer to "Table 3: Available formats of
    data elements” for the list of available formats.

[^4]: Specifies whether an element is required or not.
