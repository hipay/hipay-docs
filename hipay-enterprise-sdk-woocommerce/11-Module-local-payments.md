# Local payment method configuration

Local payments include all payments other than credit cards.

## Methods activation 

To activate a payment method, please refer to this [example](https://docs.woocommerce.com/document/cheque/#section-1) from the WooCommerce official documentation.

## Configuration interface

To configure the HiPay Enterprise plugin, click on "_WooCommerce -> Settings -> Payments_" in your WordPress dashboard. Then click on the name of the payment method you want to configure ("_HiPay Enterprise Bnppf-3xcb_" for example).

![legend](images/plugin-configuration.png)

## Local payment method settings

Configuration is done as for [credit cards](#global-plugin-and-credit-card-configuration-payment-methods-credit-card), 
except for certain local payments: "currencies" and "countries" cannot be modified because they are imposed by each payment 
method. 

You can also define the following element for each payment method.

   | Name          | Description | Value |
   |:--------------|:------------|:-----|
   | Display name  |  Name displayed on the WooCommerce checkout page | String  |
