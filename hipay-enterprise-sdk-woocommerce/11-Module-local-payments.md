# Local payment methods configuration

Local payments include all payments other than bank cards.

## Activate Method 

To activate a payment method, refer to this [example](https://docs.woocommerce.com/document/cheque/#section-1) of the WooCommerce official documentation.

## Access to configuration

To configure your HiPay Enterprise plugin, click on "_WooCommerce -> Settings -> Payments_” in your Wordpress back office. Then click on the name of payment method you want to configure ("_HiPay Enterprise Bnppf-3xcb_" for example).

![legend](images/plugin-configuration.png)

## Local payment method settings

Configuration is done as for [credit cards](#global-plugin-and-credit-card-configuration-payment-methods-credit-card), except for certain local payments: "currencies" and "countries" cannot be modified.

You can also define the following elements for each payment method:

   | Name          | Description | Value |
   |:--------------|:------------|:-----|
   | Display name  |  Name displayed on the PrestaShop checkout page | String  |
