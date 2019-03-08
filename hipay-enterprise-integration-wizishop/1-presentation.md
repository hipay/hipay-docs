# HiPay Enterprise integration for WiziShop

## Preamble
HiPay Enterprise is fully integrated into WiziShop, allowing you to accept payments from your WiziShop online store. You don't need any technical knowledge in order to leverage this turnkey integration.

# Prerequisites

## HiPay Enterprise account

You need a HiPay Enterprise account. The HiPay Enterprise platform will process your customers' payments with a wide variety of payment methods (major credit and debit cards such as Visa, MasterCard and American Express as well as local payment methods).

If you don't have an account yet, please create a test account by clicking on the "Sign Up" button on the [HiPay website](https://hipay.com/en/enterprise) and [contact us](https://hipay.com/en/contact-us).

## WiziShop account

You also need a WiziShop account. If you don't have one yet, you can create a store directly on the [WiziShop website](https://www.wizishop.fr/).

# Objective

The HiPay Enterprise integration into WiziShop allows you to accept payments in your WiziShop store using either credit or debit cards (MasterCard, Visa, etc.) or local payment methods such as SOFORT, Multibanco, iDEAL, Przelewy24, Carte Bancaire, Bancontact, etc.

# Configuration

You don't need to install any software in order to make this integration work. You just need to configure it through the WiziShop back office.

Please follow the steps below in order to plug HiPay Enterprise to WiziShop.

## 1. WiziShop back office payment section

Log in to the WiziShop back office and go to the payment section.

Once logged in, move your cursor over "*Configuration*" in order to display the sub-menu and click on the "*Paiement*" tab:

![WiziShop back office - Payment](images/wizishop_payment.png)

Then look for the HiPay integration and click on it:

![WiziShop back office - Add HiPay](images/wizishop_hipay_enterprise.png)

## 2. Configuration

Please follow the configuration steps described on the WiziShop back office's HiPay Enterprise integration page. You will need to log in to the [HiPay Enterprise back office](merchant.hipay-tpp.com) to retrieve your Api Login and Password as well as your Secret PassPhrase in order to configure the integration.

At the end of the configuration, you should have filled in the form as follows:

![WiziShop back office - Add HiPay](images/wizishop_config.png)

## 3. Payment workflow customization (optional)

You may customize the payment page by uploading your own CSS (cascading style sheet) file. For more information, please refer to the [HiPay Enterprise platform documentation](/getting-started/platform-hipay-enterprise/overview/). You will be able to upload your CSS file by using the appropriate button on the WiziShop back office.

Optionally, you can customize the payment workflow by clicking on the link allowing you to do so in the WiziShop back office. For example, these options allow you to change the payment logo as well as its label. These options are visible by clicking on the link at the bottom of the page, above the "Sauvegarder" button.

## 4. Save

When your integration is configured, click on the "Sauvegarder" button at the bottom of the page.

And that's it! Your payment workflow is all set on your WiziShop store. Please make a test order on your store in order to confirm that everything works well.