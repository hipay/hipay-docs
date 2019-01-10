# HiPay Enterprise Javascript SDK

The HiPay Javascript SDK allows you to tokenize credit or debit cards using the HiPay Enterprise payment platform, directly from your web browser. It enables you to offer a unified payment workflow to your customers while remaining PCI compliant.

## Hostedfields

[HiPay Hostedfields](https://hipay.com/fr/hosted-fields) help you to integrate easily your payment forms. It handle the collect of sensitive information and tokenize them without having it touch your server.

Hostedfields main features: 

* Reduce PCI requirements
* SAQ-A compliant
* Fully customizable
* Real-time field validation
* Formatting and Masking
* Browser autofill support
* Responsive
* Translated error messages
* Card brand identification
* Easy integration
* Automatic updates

[Hostedfields integration guide](#hipay-hostedfields-integration-guide)


[//]: <> (### Hostedfields examples)

[//]: <> (Hostedfields are fully customizable to match perfectly your style guides.)

[//]: <> (Let's have a look at our [Demonstration page]() to see different payment form integrations.)


## Direct POST

Direct POST allows you to tokenize manually your sensitive information with the [tokenize function](#hipay-sdk-js-reference-the-hipay-instance-hipaytokenizeparams).
If you are looking for reducing PCI requirements or incrinsing your security, you may prefer [Hostedfields](#hostedfields)
