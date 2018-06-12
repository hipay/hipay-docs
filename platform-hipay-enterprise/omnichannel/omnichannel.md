# Omnichannel APIs

## Objectives 

This document aims to describe the omnichannel strategy behind the HiPay Enterprise platform and how to leverage its features through our APIs.  
Among other features, the POS capabilities of the HiPay Enterprise platform are outlined hereafter.

## Acronyms and abbreviations

The following acronyms and abbreviations are used in this document.

| Acronym   | Full name |
|----------|-------------|
| POS | Point of sale |
| mPOS | Mobile point of sale |
| API | Application programming interface |

## Overview

First of all, we invite you to visit our [corporate website](https://hipay.com/en/omnichannel-payment-services) for an overview of HiPay's omnichannel solutions.

The HiPay Enterprise platform centralizes payment management both for your online and physical stores. The HiPay Gateway allows you to process electronic payments from your online store as well as to initialize transactions on in-store payment terminals, through a single platform.

![Omnichannel API](images/omnichannel-platform.jpg)

Basically, the HiPay Gateway covers three scenarios:

- processing standard e-commerce payment transactions,
- initializing transactions on a payment terminal provided by HiPay or on any terminal embedding the HiPay POS software,
- storing data related to your in-store transactions that have been processed by a third party (in-store gateways).

These scenarios and their integration methods are described below.

# Scenarios

## E-commerce processing

Standard e-commerce processing of payment transactions is described in the [HiPay Enterprise platform overview](/getting-started/platform-hipay-enterprise/overview/). Please refer to this documentation in order to integrate your online shop with HiPay for e-commerce processing (i.e., when customers provide their debit or credit card details through a payment form).

## Initializing transactions on payment terminals (POS)

### Use cases

HiPay provides you with APIs allowing to initialize transactions on payment terminals, including mPOS terminals. For instance, these APIs are useful for:

- store-to-web scenarios, where customers place e-commerce orders on an in-store device (such as a tablet) and pay on a physical payment terminal;
- mobile point of sale scenarios, where customers confirm their in-store order on a device combining the cash register application and the payment terminal.

### Overview

The workflow for managing transactions on physical payment terminals and for charging your customers is the same as the e-commerce one. In fact, in-store transactions are just payment transactions, with basically the same behavior than e-commerce transactions.

The HiPay Enterprise Gateway makes publicly available its omnichannel `/order` API, which allows you to both process e-commerce payments and initialize in-store transactions on payment terminals.

![Omnichannel API](images/omnichannel-pos-api.jpg)

If your system is already integrated with the HiPay Enterprise Gateway for e-commerce payments, you may re-use or refactor your integration in order to leverage it for omnichannel purposes. Please see below to get all the technical details of the POS integration.

### Provisioning payment terminals

To be able to initialize transactions on payment terminals through the HiPay platform, you need to get provisioned payment terminals from HiPay. Depending on your needs, you can get such terminals in different ways:

- Payment terminals configured with the proper HiPay software can be shipped directly from our partners network,
- Your existing payment terminals can be updated by your fleet maintainer in order to embed the HiPay POS software.

Please [submit a request](https://support.hipay.com/hc/en-us/requests/new) on our Support Center to get provisioned payment terminals.

### Initializing a transaction

#### Requirements

Before going through this section, we recommend you to have payment terminals properly configured to be managed through the HiPay Enterprise platform. Once your terminals are configured, HiPay will provide you with a list of unique IDs for your payment terminals.

Moreover, this section refers to APIs which are not specific to in-store payments but generally used in HiPay's transactional workflow, featuring some specific parameters related to POS and physical payment terminals. 

Implementing POS scenarios through HiPay is thus easier if you are already familiar with the [HiPay Enterprise platform overview](/getting-started/platform-hipay-enterprise/overview/) and if you have already implemented and tested the [HiPay Enterprise Gateway APIs](/doc-api/enterprise/gateway/).

#### Creating transactions

To initialize a transaction, you need to send a request to the **`POST /order`** service endpoint. You can find the full list of parameters for this endpoint as well as live testing tools in the [HiPay Enterprise Gateway API section](/doc-api/enterprise/gateway/#!/payments/requestNewOrder). This API is the same as the one used to process e-commerce payments.

In order to initialize transactions on payment terminals, you:

- must send complementary parameters,
- must set some parameters to specific values,
- should provide complementary parameters.

These parameters are listed below.

| Field name | Format | Req. | Specific to POS | Description |
| --- | :---: | --- | :---: | --- |
| `eci` | Numeric | Yes | No | Electronic Commerce Indicator. The ECI indicates the security level at which the payment information is processed between the cardholder and the merchant. In this case, **its value must be set to `10` (Point of sale payment)**.
| `payment_product` | Alphanumeric | No | No | Payment method used to proceed with checkout. In the case of POS payments, you don't know in advance the brand of the card which will be used with the payment terminal. Therefore, **this parameter must not be sent for POS transactions.**
| `initialize_payment_terminal` | Numeric | Yes | Yes | Tells the platform if a payment terminal should be initialized with the related transaction. As this section describes how to initialize payment terminals, **its value must be set to `1`**.
| `payment_terminal_id` | Numeric | No | Yes | ID of the payment terminal on which you need to initialize the transaction. These IDs are supplied by HiPay for each provisioned payment terminal. **You must provide a value if you want to send the transaction to a specific payment terminal.**
| `store_id` | Numeric | No | Yes | ID of the store in which customers process their payment. These IDs are supplied by HiPay when we register your stores in our system. **Providing a value is strongly recommended.**
| `order_point` | Alphabetic | No | No | Order point used by customers to place orders. This parameter accepts two values: `web` or `store`. If your customer is ordering from a tablet in your store, you must provide `store` (the `web` value is only useful for e-reservation orders). **Providing a value is strongly recommended.**
| `pos_transaction_lifetime` | Numeric | No | Yes | Lifetime of the transaction on the payment terminal, in seconds. If the transaction has not been processed within this timeframe, it will transition to the *expired* status. We recommend you to provide a value of `60`, which equals to 1 minute.

You also need to send other global parameters related to the order, such as the amount, the customer's e-mail address, etc. All these parameters are documented in the [HiPay Enterprise Gateway API section](/doc-api/enterprise/gateway/#!/payments/requestNewOrder).

#### Call example

Here is an example of a cURL call for initializing a transaction on a payment terminal in the test environment:

```json
curl -X POST \
--header 'Content-Type: multipart/form-data' \
--header 'Accept: application/json' \
--header 'Authorization: Basic OTQ2NTgzNjUuc3RhZ2Utc2VjdXJlLWdhdGV3YXkuaGlwYXktdHBwLmNvbTpUZXN0X1JoeXBWdktpUDY4VzNLQUJ4eUdoS3Zlcw==' \
-F orderid=1502198470978 \
-F eci=10 \
-F description='Blue shirt' \
-F currency=EUR \
-F amount=10.90 \
-F language=fr_FR \
-F email=customer@example.com \
-F firstname=John \
-F lastname=Doe \
-F initialize_payment_terminal=1 \
-F pos_transaction_lifetime=60 \
-F payment_terminal_id=62 \
-F store_id=1 \
-F order_point=store \
'https://stage-secure-gateway.hipay-tpp.com/rest/v1/order'
```

You can also perform test API calls from [our live testing tool](/doc-api/enterprise/gateway/#!/payments/requestNewOrder).

### Payment on mobile point of sale (mPOS)

For making payments on a mPOS payment terminal plugged with an iPhone or iPod, then you need to use our iOS SDK. In order for the mPOS terminal to receive transactions, you need to call the `connect` method of the iOS SDK beforehand.

Once a transaction has been initialized through the API mentioned in the previous sections, call the `wakeUp` method of the iOS SDK to force the mPOS payment terminal to get the transaction.

For using the HiPay Enterprise SDK for iOS in your application to process transactions on a mPOS, please read [its documentation](/doc/hipay-enterprise-sdk-ios/).

### Transaction lifecycle

When the transaction is initialized on the payment terminal, you may display a waiting screen on your application, indicating that the customer must pay on the terminal.

Once the transaction is paid, you need to have a technical callback in place allowing you to know the transaction status (whether it is paid or declined). This will enable you to confirm the order and display the proper feedback screen on your cash register application.

Transaction status updates are delivered through *HTTP server-to-server notifications*. These notifications are exactly the same for both e-commerce and payment terminal transactions. **Please refer to the [*server-to-server notifications* section of the HiPay Enterprise platform overview](/getting-started/platform-hipay-enterprise/overview/#server-to-server-notifications)** to learn how to manage notifications and status updates.

Regarding the *status* and *state* parameters described in the server-to-server notifications section mentioned above, here are some basic scenarios that you will encounter.

#### The transaction is properly paid on the terminal

In that case, the workflow would be:

1. The transaction is initialized, its state is set to **`pending`** and its status to **`174`** (Awaiting Terminal).
2. The customer pays the transaction on the payment terminal.
3. Your server receives a HTTP notification with a status update. The transaction *state* is set to **`completed`** and its *status* to **`118`** (Captured).

#### The transaction is declined on the terminal

In that case, the workflow would be:

1. The transaction is initialized, its state is set to **`pending`** and its status to **`174`** (Awaiting Terminal).
2. The customer encounters an error and the payment is refused.
3. Your server receives a HTTP notification with a status update. The transaction *state* is set to **`declined`** and its *status* to **`113`** (Refused).

Please note that "Refused" is just an example and you may receive other statuses depending on the error. However, in case of an error, the *state* value will always be either `declined` or `error`.

#### The transaction has expired

In that case, the workflow would be:

1. The transaction is initialized with a lifetime of 60 seconds, its state is set to **`pending`** and its status to **`174`** (Awaiting Terminal).
2. The customer waits one minute and does not make the payment.
3. Your server receives a HTTP notification with a status update. The transaction *status* is set to **`114`** (Expired).

#### List of statuses

In order to get the complete list of statuses, please refer to the [HiPay Enterprise platform overview appendices](https://developer.hipay.com/getting-started/platform-hipay-enterprise/appendices/).
