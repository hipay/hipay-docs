# HiPay Enterprise JavaScript SDK

The HiPay Enterprise JavaScript SDK allows you to tokenize credit or debit cards using the HiPay Enterprise payment platform, directly from your web browser. It enables you to offer a unified payment workflow to your customers while remaining PCI DSS compliant.

## Hosted Fields

The [HiPay Hosted Fields](https://hipay.com/fr/hosted-fields) help you to easily integrate your payment forms by collecting sensitive data and tokenizing them without going through your server.

Hosted Fields' main features: 

* Easy and secure integration
* Simplified PCI DSS requirements/SAQ A eligibility
* Fully customizable and responsive solution
* Card brand identification
* Real-time field validation
* Data formatting and masking
* Browser autofill support
* Translated error messages
* Automatic updates

Go to: [HiPay Hosted Fields integration guide](#hipay-hostedfields-integration-guide)


[//]: <> (### Hosted Fields examples)

[//]: <> (Hosted Fields are fully customizable to perfectly match your style guides.)

[//]: <> (Let's have a look at our [Demonstration page]() to see different payment form integrations.)


## Direct POST

Direct POST allows you to manually tokenize sensitive information with the [tokenize function](#hipay-sdk-js-reference-the-hipay-instance-hipaytokenizeparams).
If you aim at reducing PCI DSS requirements or increasing your security, you may prefer [Hosted Fields](#hostedfields).
