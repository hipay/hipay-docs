# Module configuration

Before using the HiPay Enterprise module for Magento 2, the following settings are required and must be configured:

1. Credentials configuration,
2. Fraud emails configuration.

In your Magento Admin Panel, select:  
```
Stores => Configuration => Sales [HiPay Enterprise]
```

Then, you need to enter your credentials, provided by HiPay.

![legend](images/enterprise_configuration.png)


## Credentials

### HiPay Enterprise credentials configuration  
HiPay Enterprise API credentials are required to use the HiPay Enterprise module for Magento 2.

|Field|Description|
|-----|-----|
|Api username (production account)|Retrieve it from your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Api password (production account)|Retrieve it from your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Secret passphrase (production account)|Enter the same value as in your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Api username (test account)|Retrieve it from your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Api password (test account)|Retrieve it from your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Secret passphrase (test account)|Enter the same value as in your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |

![legend](images/credentials_conf.png)

### HiPay Enterprise credentials Direct Post configuration

Generated in your HiPay Enterprise back office ("Integration" => "Security Settings" => "Api credentials" => "Credentials accessibility": Public), these newly created HiPay Enterprise API credentials are required to use the HiPay Enterprise module for Magento 2.

|Field|Description|
|-----|-----|
|Api username (production account)|Retrieve it from your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Api password (production account)|Retrieve it from your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Api username (test account)|Retrieve it from your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Api password (test account)|Retrieve it from your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |

![legend](images/credentials_js.png)

### HiPay Enterprise credentials MO/TO configuration  
MO/TO API credentials are optional.  
They are required only if you need to pay an order created in your Magento Admin Panel.

|Field|Description|
|-----|-----|
|Api username (production account)|Retrieve it from your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Api password (production account)|Retrieve it from your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Secret passphrase (production account)|Enter the same value as in your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Api username (test account)|Retrieve it from your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Api password (test account)|Retrieve it from your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Secret passphrase (test account)|Enter the same value as in your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |

![legend](images/credentials_moto.png)

## Email templates

Based on the screening results of the HiPay Fraud Protection Service, emails can be sent to final customers.
There are 3 email templates:  

*  **Fraudulent**: This email is sent to the customer if the payment is fraudulent.
*  **Accepted**: This email is sent to the customer if the payment is approved by the merchant.
*  **Denied**: This email is sent to the customer if the payment is denied by the merchant.

They all have the same configuration:

|Field|Description|
|-----|-----|
|Enabled|Enables/Disables sending|
|Payment Fraud Email Sender|Sets the email sender|
|Payment Fraud Template|Sets the email template. You can **customize it** in your Magento 2 Admin Panel. To do so, go to *"Marketing" => "Communications" [Email Templates]*. Click on *"Add New Template"*, then load the HiPay email template you want and modify it.|
|Send Payment Fraud Email Copy To|Email addresses you want to add in copy|
|Send Payment Fraud Email Copy Method|Selects email copy method (Cc or Bcc)|

![legend](images/fraud_email_review.png)

### Other configurations

|  Name    | Description|
|----------|:-------------:|
|  Device fingerprint    | Defines if a fingerprint is sent with the transaction ("YES" by default)
| Url of Hipay's javascript  |Technical parameter not to be modified
| Send cart  | Activates  customer's cart items sending or not ("NO" by default)
| Ean attribute | EAN is not a Magento attribute by default: you must define your custom attribute if you want to send it in the basket

### Customer's cart items configuration

This section addresses customer's cart items sending to the HiPay Enterprise back office during the transaction.

Enabling this option applies to all enabled payment methods on your site.
The information of the customer's basket, containing the method of delivery, the discounts and each product with the quantity,
as well as the SKU and the tax, is sent with the transaction.

For some payment methods, sending this information is mandatory. This option is therefore ignored if the transactions
are made with this payment method. The customer's line items will be sent whether the option is activated or not.
The payment methods in question are **Klarna Invoice** and **Oney Facily Pay**.

Oney's Fraud system requires additional configuration for shipping method and product categories.
The configuration is explained in the following paragraph.

Please note that this feature is still in beta version. For questions relating to installation and configuration, please don’t hesitate to visit our Support Center ([*https://support.hipay.com/hc/en-us*](https://support.hipay.com/hc/en-us)) or submit a request ([*https://support.hipay.com/hc/en-us/requests/new*] (https://support.hipay.com/hc/en-us/requests/new)) to our Support team.

Please assume that  **"Adjustment Fee"** or **"Adjustment Refund"** are not supported with baskets for refunds.

#### Categories and shipping methods mapping

To enable sending relevant information about delivery methods and product categories, mapping
is required between your data and HiPay's data.

##### Categories mapping

Go to the setup screen `HiPay Enterprise` => `Mapping Categories`.

Only the top level categories are displayed and must be mapped. If the mapping is not done, then the transaction will be refused
by Oney. It is therefore important to check your mapping regularly when adding or modifying a category.

![](images/mapping_categories.png)

##### Shipping methods mapping

Go to the setup screen `HiPay Enterprise` => `Mapping Shipping method`.

A list of all the delivery methods activated on the site is displayed.
This mapping is necessary to indicate a match between your delivery methods and the delivery methods defined by HiPay.
For each customer's order, depending on the chosen configuration, this information is sent as a supplement to the customer's basket.

For each mapping, you have to fill out the following information:

   *   "Preparation delay": Estimated day time for order preparation
   *   "Delivery delay": Estimated day time for delivery

From this information, an estimated delivery day is calculated and sent with the transaction.
Non-working days are not taken into account in this calculation.

![](images/mapping_shipping_methods.png)

As with the mapping of categories, **it is important that all payment methods be mapped**. Therefore, it is important
to see your list if you change the configuration of your payment methods.
