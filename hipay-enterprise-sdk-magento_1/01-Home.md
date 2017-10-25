# HiPay Enterprise – Magento 1 – Integration guide


**DISCLAIMER**
While every effort has been made to ensure the accuracy of the information contained in this publication, the information is supplied without representation or warranty of any kind, is subject to change without notice and does not represent a commitment on the part of HiPay.
HiPay, therefore, assumes no responsibility and shall have no liability, consequential or otherwise, of any kind arising from this material or any part thereof, or any supplementary materials subsequently issued by HiPay.

**TECHNICAL SUPPORT** If you need any complementary information concerning the technical implementation of HiPay Enterprise, please don’t hesitate to visit our Support Center ([*https://support.hipay.com/hc/en-us*](https://support.hipay.com/hc/en-us)) or submit a request ([*https://support.hipay.com/hc/en-us/requests/new*] (https://support.hipay.com/hc/en-us/requests/new)) to our Support team.

# About this guide

## Purpose

This document is designed to provide you with details on how to integrate your business to the HiPay Enterprise Payment Gateway. 
It gives you step-by-step instructions on how to simply and quickly get up and running with our services as well as detailed reference material.

Where applicable, this document refers to the related documentation with further details.

## Intended audience

This document is designed for the merchants' technical staff or system integrator.

## Copyright

The information contained in this guide is a property of HiPay. This material shall not be duplicated, published, or disclosed, in whole or in part, without the prior written permission of HiPay.

## Legal notice

This document contains the proprietary and confidential information of HiPay. Such information may not be used for any unauthorized purpose and may not be published or disclosed to third parties, in whole or part, without the express written permission of HiPay. 
You acknowledge and agree that this document and all portions thereof, including, but not limited to, any copyright, trade secret and other intellectual property rights relating thereto, are and at all times shall remain the sole property of HiPay and that title and full ownership rights in the information contained herein and all portions thereof are reserved to and at all times shall remain with HiPay. You agree to safeguard the confidentiality of the information contained herein using the same standard you employ to safeguard your own confidential information of like kind, but in no event less than a commercially reasonable standard of care. 
If you do not agree with the foregoing conditions, you are required to return this document immediately to HiPay.

# How to read this guide

## Document conventions

To ease readability and improve understanding, this document uses a number of conventions.

This guide uses several typographic conventions to highlight certain words and phrases and draw attention to specific pieces of information.
The following table clarifies the conventions used across this guide.

*Table 1: Typographic conventions that apply across this guide*

| Convention   |      Meaning      |
|----------|:-------------:|
| Mono-space |  Indicates source code, code examples, input to the command line, application output, code lines embedded in text, variables and code elements. |
| *Italics* |  Italicized regular text is used for emphasis or to indicate a term that is being defined or will be defined shortly. |
| ... |  Horizontal ellipsis points in sample code indicate the omission of part of the code (i.e. when you would normally expect additional code to appear, but such code would not be related to the example). |
| $ |  At the beginning of a command, indicates an operating system shell prompt. |
| &lt;placeholder&gt; |  Indicates placeholders, most often method or function parameters; these placeholders represent information that must be supplied by the implementation or the user. For command-line input, indicates parameter values.|

**Abbreviations used in tables**

The tables of this document describe data elements, which are equivalent to parameters in a query or to fields of a response
message. The following table helps understanding the different attributes (columns) that define a data element.

*Table 2: Description of data elements*

| Column   |      Meaning    |
|----------|:-------------:|
| Field name |  Name of the data element |
| Format |  Format of the element |
| Length |  Maximum size of the element.<br/>For example, a size of 6 means that the data value cannot exceed six characters.<br/>When no size is specified, it means that there is no limitation on the length of the field. |
| Req. |  Specifies whether an element is required or not. <br/>**M** = Mandatory; must always be provided.<br/>**C** = Conditional; requirement depends on the value or presence of other elements. |
| Description |  Brief description of the data element and, where applicable, a list of valid values and element dependencies. |

*Table 3: Available formats of data elements*

| Format abbreviation   |      Description    |     Example    |
|----------|:-------------:|:-------------:|
| AN |  Alphanumeric characters (a-z, A-Z, 0-9)  ||
| A |  Alphabetic characters only (a-z, A-Z)  ||
| N |  Numeric characters only ||
| R |  Decimal number with explicit decimal point, signed |12.34|
| DT |  Date in the format YYYY-MM-DD |2016-12-31|
| TM |  Time in the format HH:MM with optional seconds (HH:MM:SS) |15:30|

**Acronyms and abbreviations**

The following acronyms and abbreviations are used in this guide.

*Table 4: Acronyms and abbreviations*

|Acronym or abbreviation |   Full name/meaning |
|----------|:-------------:|
|  BIN |                       Bank Identification Number
|  PAN  |                     Primary Account Number
|  PCI DSS  |                 Payment Card Industry Data Security Standard
|  REST   |                   Representational State Transfer
|  TPP   |                   Third Party Processing


# Introduction

This document describes how to install and use the HiPay Enterprise module for Magento webshop.

## Table of contents
1. [Release notes](#release-notes)
2. [Module features](#module-features)
2. [Platform configuration](#platform-configuration)
3. [Module installation](#module-installation)
4. [Module configuration](#module-configuration)
5. [Payment configuration](#payment-configuration)
6. [Front-end payment](#front-end-payment-examples)


## Please note

-   You will need to configure your HiPay Enterprise account                                                                             
    before installing your Magento module in your HiPay Enterprise back office
    ([*https://merchant.hipay-tpp.com/*](https://merchant.hipay-tpp.com/)).
-   The HiPay Enterprise module is not offered natively on Magento,                                                                      
    you must contact HiPay to get it.
-   This module requires valid HiPay Enterprise credentials to be able to use it;
    please contact HiPay to get them.

# Release notes

Please find here all the latest versions of the Magento module:

| Date |Version| Level| Main feature | 
|----|----|----| -----------|
| 06-12-2017| 1.7.0 |Major version | Supports Oney Facily Pay  
| 05-05-2017| 1.6.2 |  Minor version | TokenJS Domain
| 04-13-2017| 1.6.1 |  Minor version | Security
| 02-23-2017| 1.6.0 |  Major version | Supports MO/TO


[See details of all versions](#release-notes-details)
