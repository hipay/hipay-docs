# Omnichannel APIs

## Objectives

This section aims to describe the omnichannel philosophy behind HiPay's products and how to leverage its features through the HiPay's APIs. Among other features, this document outlines the POS (point-of-sale) capabilities of the HiPay Enterprise platform.

# Acronyms and abbreviations

The following acronyms and abbreviations are used in this guide.

| Acronym   | Full name |
|----------|-------------|
| POS | Point of sale |
| mPOS | Mobile point of sale |
| API | Application Programming Interface |

## Overview

First of all, you can check out our [corporate website](https://hipay.com/en/omnichannel-payment-services) in order to have an outline of the [HiPay's omnichannel products](https://hipay.com/en/omnichannel-payment-services).

The HiPay Enterprise platform centralizes the management of your payments for both your online and physical stores. The HiPay's Gateway allows you to both process electronic payments from your online store or initialize transactions on in-store payment terminals, the whole through a single platform.

![Omnichannel API](images/omnichannel-platform.jpg)

Basically, the HiPay's Gateway covers three types of scenarios:

- traditional e-commerce processing,
- initializing a transaction on a payment terminal provided by HiPay or embedding the HiPay's omnichannel softwares,
- storing data related to your in-store transactions that have been processed by a third party (in-store gateways).

These scenarios and their integration methods are described below.

# Scenarios

## E-commerce processing

The tradional e-commerce processing of payment transactions is described in the [HiPay Enterprise platform overview](/getting-started/platform-hipay-enterprise/overview/). Refer to this documentation in order to integrate your online shop with HiPay for e-commerce processing only (i.e. your user provides its debit or credit card details through a payment form).

## Initializing transactions on payment terminals (POS)

### Use cases

HiPay provides APIs allowing you to initialize transactions on payment terminals, including mPOS (mobile point of sale) terminals. For instance, these APIs are useful for:

- store-to-web scenarios, in which the user places an e-commerce order on an in-store device (such as a tablet), paying it on a physical payment terminal;
- mobile point of sale scenarios, when the user confirms his in-store order on a device combining both the cash register application and the payment terminal.

### Overview

The workflow for managing transactions on physicial payment terminals and for charging your customers is the same as the e-commerce one. In fact, in-store transactions are just payment transactions which basically have the same behavior as e-commerce transactions.

The HiPay Enterprise Gateway exposes its omnichannel */order* API, which allows you to both process e-commerce payments and initialize in-store transactions on payment terminals.

![Omnichannel API](images/omnichannel-pos-api.jpg)

If your system is already integrated with the HiPay Enterprise Gateway for e-commerce payments, you may reuse or refactor your integration in order to leverage it for omnichannel purposes. See below to get all the technical details of the POS integration.

### Provisionning payment terminals

Before being able to initialize transactions on payment terminals through the HiPay platform, you need to get provisioned payment terminals from HiPay. Depending on your needs, you can get such terminals in defferent ways:

- Payment terminals configured with proper HiPay's software can directly be shipped through our partners network.
- Your existing payment terminals can be updated by your payment terminals fleet maintainer in order to install HiPay's software.

Please contact the HiPay's Business IT Services in order to get provisioned payment terminals.

### Initializing a transaction

#### Requirements

Before going through this part, we recommend you to have payment terminals properly configured to be managed through the HiPay Enterprise platform. Once you have your terminals configured, HiPay will provide you with a list of unique IDs for your payment terminals.

Moreover, this section refers to APIs which are not specific to in-store payments but which are general to the HiPay transactional workflow, presenting some specific parameters related to POS and physical payment terminals. 

Thus, implementing POS scenarios through HiPay is easier if you are already aware of the [HiPay Enterprise platform overview](/getting-started/platform-hipay-enterprise/overview/) and if you already implemented and tested the [HiPay Enterprise Gateway APIs](/doc-api/enterprise/gateway/).

#### Creating a transaction

To initialize a transaction, you need to send a request to the ***POST /order*** endpoint. You can find the full list of parameters for this endpoint as well as live testing tools in the [HiPay Enterprise Gateway API section](/doc-api/enterprise/gateway/#!/payments/requestNewOrder). This API is the same as the one used to process e-commerce payments.

In order to initialize transactions on payment terminals, you:
- must send complementary parameters,
- must define some parameters to specific values,
- should provide complementary complementary parameters.

These parameters are listed below.

| Field name | Format | Req. | Specific to POS | Description |
| --- | :---: | --- | :---: | --- |
| *eci* | N | Yes | No | Electronic Commerce Indicator (ECI). The ECI indicates the security level at which the payment information is processed between the cardholder and merchant. In this case, **its value must be set to 10 (in-store payment)**.
| *payment_product* | AN | No | No | The payment method used to proceed checkout. In case of POS payments, you don't know in advance the brand of the card which will be introduced in the payment terminal. Therefore, **its value must be blank.**
| *initialize\_payment_terminal* | AN | Yes | Yes | Tells the platform whether a payment terminal should be initialized with this transaction. As this section describes how to initialize payment terminals, **this value must be set to 1**.
| *payment\_terminal_id* | AN | No | Yes | The ID of the payment terminal on which you need to initialize the transaction. These IDs are provided to you by HiPay for each provisioned payment terminal. You must provide a value if you want to send the transaction on a specific payment terminal. 
| *store_id* | AN | No | Yes | The ID of the store in which the user processes his payment. These IDs are provided to you by HiPay when we register your stores in our system. **Providing a value is strongly recommended.**
| *order_point* | A | No | No | The order point used by the customer to place his order. This parameter takes two values: ***web*** or ***store***. If your customer is ordering from a tablet in your store, you must provide "**store**" (the "web" value would only be useful for e-reservation orders). **Providing a value is strongly recommended.**
| *pos\_transaction_lifetime* | N | No | Yes | The lifetime of the transaction on the payment terminal, in seconds. If the transaction hasn't been executed before in the timeframe, the transaction will transition to the *expired* status. For example, if you want the transaction to expire within 5 minutes if it hasn't been processed, provide "600" (600 seconds = 5 minutes).

You also need to pass other global parameters related to the order, such as the amount, the customer e-mail adress, etc. All those parameters are documented in the [HiPay Enterprise Gateway API section](/doc-api/enterprise/gateway/#!/payments/requestNewOrder).

#### Example call

Here is an example of cURL call for initializing a transaction on a payment terminal:

`````
curl -X POST \
--header 'Content-Type: multipart/form-data' \
--header 'Accept: application/json' \
--header 'Authorization: Basic OTQ2NTgzNjUuc3RhZ2Utc2VjdXJlLWdhdGV3YXkuaGlwYXktdHBwLmNvbTpUZXN0X1JoeXBWdktpUDY4VzNLQUJ4eUdoS3Zlcw==' \
-F orderid=1502198470978 \
-F eci=10 \
-F description='Blue shirt' \
-F currency=EUR \
-F amount=1.2 \
-F language=fr_FR \
-F email=customer@example.com \
-F firstname=John \
-F lastname=Doe \
-F initialize_payment_terminal=1 \
-F pos_transaction_lifetime=60 \
-F payment_terminal_id=1 \
-F store_id=1 \
-F order_point=store \
'https://stage-secure-gateway.hipay-tpp.com/rest/v1/order'
`````
