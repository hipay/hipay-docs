---
title: Customize your payment page
description: This document is designed to provide you details on how to integrate your business to the HiPay Direct payment gateway.
platform: enterprise
---

## Preamble
The **HiPay Fullservice SDK for Android** is a library that allows you to **accept payments in your Android application** by leveraging the HiPay Fullservice payment platform. The library is written in Java and is based on Android framework. This repository contains the SDK as well as a demo application allowing you to generate a simple payment screen and demonstrating how to use the SDK.

## Objective
This documentation describes how to install the HiPay Fullservice SDK for Android in order to accept payments in your own Android application. You will be provided with several use cases including the usage of the built-in payment screen as well as implementation guides for custom integrations.

## Prerequisites and recommentations

### Build settings

The project *minSdkVersion* is 14 (Ice Cream Sandwich), which means that the SDK won't build on applications targeting a lower version of Android.

### Credentials

You need to generate *API credentials* in order for the SDK to send requests to the HiPay Fullservice platform. To do so, go in the "Integration" section of the HiPay Fullservice back office and then go to "Security Settings".

**Warning: credentials included in the Android SDK must have a public accessibility, NOT a private accessibility.**  
To be sure that your credentials have the proper accessibility:

- Go to the "Integration" section of your HiPay Fullservice merchant back office
- Then, click on "Security Settings"
- Scroll down to "Api credentials"
- Click on the edit icon next to the credentials you want to use 
- **Finally, make sure that the credentials are set up as displayed on the screenshot below**:

![Credentials accessibility](https://raw.githubusercontent.com/hipay/hipay-docs/master/hipay-fullservice-sdk-android/images/prerequisites/credentials_accessibility.png)

Your credentials must be granted to:

- Tokenize a card
- Get transaction details
- Process an order 
- Create a payment page

### Checkout workflow integration

This section lets you know about the scope of the HiPay Fullservice SDK for iOS. Some parts of the checkout are integrated on the client side (that's to say your mobile app, thanks to the SDK) and some other parts must be integrated on the server side.

#### Authentication

When you redirect your user to the HiPay iOS SDK's payment page, the SDK will process transactions with user's payment details againt the HiPay Fullservice payment platform. In order to let our iOS SDK process payments to the HiPay Fullservice platform, you must provide the SDK with credentials (more info in the Configuration section).

However, the credentials are not sufficient. For each order processed by the iOS SDK, the HiPay Fullservice platform must authenticates the call and validates that the merchant allowed it. To do so, HiPay Fullservice leverages a signature mechanism. Once your app needs to process a payment, it needs to contact your own server in order to get a signature, specific to the order to be treated.

To know how to generate a signature on the server side, check the ["Generating a signature" section](#generating-a-signature-server-side).

Check out the diagram below (Mobile checkout workflow) to see when the signature should be generated in the checkout workflow.

### Order processing

Order information should not be generated on the client side. Most of the time, orders are generated on the server side (on your servers), following actions performed by your users on your mobile app. The HiPay Fullservice SDK for iOS allows you to present a payment page to your user and to process transactions directly in your mobile app. 

Thus, your mobile app will receive a confirmation with a transaction status indicating that the payment was successfully executed. You may display a confirmation screen upon this confirmation, but you must not process order on the server-side following this confirmation. For security reasons, orders must always be confirmed on your systems following the server-to-server notification.

Check out the diagram below (Mobile checkout workflow) to see when the server-to-server notification is triggered in the checkout workflow and therefore, when you should validate orders.

#### Mobile checkout workflow

Below is a diagram presenting the integration scope of the Mobile SDK and the necessary server parts.

![HiPay Fullservice Mobile SDK checkout workflow](https://raw.githubusercontent.com/hipay/hipay-docs/master/hipay-fullservice-sdk-android/images/prerequisites/workflow.png)
