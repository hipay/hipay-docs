# Module configuration

Before using the HiPay Enterprise module for Magento 2, the following settings are required and must be configured:

1. Credentials configuration,
2. Anti-fraud emails configuration.

In your Magento Admin Panel, select:  
```nohighlight
Stores => Configuration => Sales [HiPay Enterprise]
```

Then, you need to enter your credentials, provided by HiPay.

![legend](images/enterprise_configuration.png)


## Credentials

### HiPay Enterprise credentials configuration  
HiPay Enterprise API credentials are required to use the HiPay Enterprise module for Magento 2.

|Field name|Description|
|-----|-----|
|Api username (production account)|Retrieve it from your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Api password (production account)|Retrieve it from your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Secret passphrase (production account)|Enter the same value as in your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Api username (test account)|Retrieve it from your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Api password (test account)|Retrieve it from your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Secret passphrase (test account)|Enter the same value as in your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |

![legend](images/credentials_conf.png)

### HiPay Enterprise credentials for Direct Post configuration

Generated in your HiPay Enterprise back office ("Integration" => "Security Settings" => "Api credentials" => "Credentials accessibility": Public), these newly created HiPay Enterprise API credentials are required to use the HiPay Enterprise module for Magento 2.

|Field name|Description|
|-----|-----|
|Api username (production account)|Retrieve it from your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Api password (production account)|Retrieve it from your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Api username (test account)|Retrieve it from your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Api password (test account)|Retrieve it from your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |

![legend](images/credentials_js.png)

### HiPay Enterprise credentials for MO/TO configuration  
MO/TO API credentials are optional.  
They are required only if you need to pay an order created in your Magento Admin Panel.

|Field name|Description|
|-----|-----|
|Api username (production account)|Retrieve it from your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Api password (production account)|Retrieve it from your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Secret passphrase (production account)|Enter the same value as in your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Api username (test account)|Retrieve it from your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Api password (test account)|Retrieve it from your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |
|Secret passphrase (test account)|Enter the same value as in your HiPay Enterprise back office (https://merchant.hipay-tpp.com) in "Integration" => "Security Settings" |

![legend](images/credentials_moto.png)

## Email templates

Based on the screening results of HiPay Sentinel, our advanced anti-fraud solution, emails can be sent to end customers.
There are 3 email templates.  

*  **Fraudulent**: This email is sent to the customer if the payment is fraudulent.
*  **Accepted**: This email is sent to the customer if the payment is approved by the merchant.
*  **Denied**: This email is sent to the customer if the payment is denied by the merchant.

They all have the same configuration:

|Field name|Description|
|-----|-----|
|Enabled|Enables/disables sending|
|Payment Fraud Email Sender|Sets the email sender|
|Payment Fraud Template|Sets the email template. You can **customize it** in your Magento 2 Admin Panel. To do so, go to *"Marketing" => "Communications" [Email Templates]*. Click on *"Add New Template"*, then upload the HiPay email template you want and modify it.|
|Send Payment Fraud Email Copy To|Email addresses you want to add in copy|
|Send Payment Fraud Email Copy Method|Selects the email copy method (Cc or Bcc)|

![legend](images/fraud_email_review.png)

### Other configurations

|  Field name    | Description|
|----------|:-------------:|
|  Device fingerprint    | Defines if a fingerprint is sent with the transaction ("YES" by default)
| URL of HiPay's JavaScript  |Technical parameter not to be modified
| Send cart  | Activates  customer's cart items sending or not ("NO" by default)
| EAN attribute | EAN is not a Magento attribute by default: you must define your custom attribute if you want to send it in the cart

### Customer's cart items configuration

This section addresses customer's cart items sending to the HiPay Enterprise back office during the transaction.

Enabling this option applies to all enabled payment methods on your site.
The information of the customer's cart, containing the shipping method, the discounts and each product with the quantity, as well as the SKU and the tax, is sent with the transaction.

For **Oney Facily Pay**, sending this information is mandatory. This option is therefore ignored if the transactions
are made with this payment method. 

Oney's anti-fraud system requires additional configuration for the shipping method and product categories.
The configuration is explained in the following paragraph.

For questions relating to installation and configuration, please don’t hesitate to visit our Support Center ([*https://support.hipay.com/hc/en-us*](https://support.hipay.com/hc/en-us)) or submit a request ([*https://support.hipay.com/hc/en-us/requests/new*](https://support.hipay.com/hc/en-us/requests/new)) to our Support team.

Please note that **"Adjustment Fee"** or **"Adjustment Refund"** are not supported with carts for refunds.

#### Mapping categories and shipping methods

To enable sending relevant information about categories and shipping methods, mapping
is required between your data and HiPay's data.

##### Mapping categories

Go to the setup screen `HiPay Enterprise` => `Mapping Categories`.

Only the top level categories are displayed and must be mapped. If the mapping is not done, transactions will be refused by Oney. It is therefore important to check your mapping regularly when adding or modifying a category.

![](images/mapping_categories.png)

##### Mapping shipping methods

Go to the setup screen `HiPay Enterprise` => `Mapping Shipping method`.

A list of all the delivery methods activated on the site is displayed.
This mapping is necessary to indicate a match between your shipping methods and the shipping methods defined by HiPay.
For each customer's order, depending on the chosen configuration, this information is sent as a supplement to the customer's cart.

For each mapping, you have to fill out the following information:

   *   "Preparation delay": Estimated time for order preparation,
   *   "Delivery delay": Estimated time for delivery.

From this information, an estimated delivery day is calculated and sent with the transaction.
Non-working days are not taken into account in this calculation.

![](images/mapping_shipping_methods.png)

As with the mapping of categories, **all payment methods must be mapped**. Therefore, it is important
to update your list if you change the configuration of your payment methods.

# PSD2 and Strong Customer Authentication

Given the strong growth of the e-commerce in Europe, the second Payment Services Directive (PSD2) redefines the security standards for online payments, aiming to increase security during the payment process, while fighting more actively against fraud attempts. For more details on the regulations, we invite you to read our [guide on PSD2 compliance](https://developer.hipay.com/psd2-and-strong-customer-authentication-3-d-secure-2-compliance-and-guidance/).

As from September 14, 2019, the issuer now decides if a payment is processed depending on the analysis of more than 150 data collected during purchasing. Thanks to our Magento 2 module, we handle most of the data without you having to develop anything. Discover all the new parameters on [our API explorer](https://developer.hipay.com/doc-api/enterprise/gateway/).

## Adding or overriding PSD2 data

The accuracy of the information sent is key for making sure that your customers have a frictionless payment process. That’s why we give you the possibility to add or override PSD2 data, using a [plugin](https://devdocs.magento.com/guides/v2.3/extension-dev-guide/plugins.html) on the "HiPay\FullserviceMagento\Model\Request\Order" class which can intercept the "mapRequest" method.
 
You can either implement the plugin in your own modules or use the one that we provide. You can find this additional plugin in our [GitHub repository](https://github.com/hipay/hipay-fullservice-sdk-magento2-data). 
 
You can install this plugin in a standard way, then directly modify the “ThreeDSPlugin.php” file and the “afterMapRequest” method to add your information.
 
Please note: If there is an update of this plugin, remember to save your information so that it is not overwritten.

Finally, although we do our best to retrieve all relevant data for you, we cannot get the following information as it depends on the modules installed on your CMS or on your workflow. 

### Merchant risk statement

| Field name |  | Description |
| --- | --- | --- |
| **delivery_time_frame** |  | By default, we send “1” (Electronic delivery) if it is a downloadable or an intangible product.<br/><br/>Depending on your carrier and delivery methods, you can refine this information. <br/><br/>Possible values: <br/><br/>1 = Electronic delivery<br/>2 = Same day shipping<br/>3 = Overnight shipping<br/>4 = Two-day or more shipping |
| **shipping_indicator** |  | We handle this field but you can override the value for intangible products.<br/><br/>If the cart contains only intangible products, please provide:<br/><br/>5 = Digital goods<br/>6 = Travel and event tickets, no shipping<br/>7 = Other (gaming, digital services without shipping, e-media subscription) |
| **pre_order_date** |  | **If you offer the possibility of pre-ordering,** you must retrieve the product availability date. | 
| **gift_card** |  | **If you sell gift cards** |
|  | **amount** | Collect the amount of gift cards purchased. |
|  | **count** | Collect the count of gift cards purchased. |
|  | **currency** | Collect the gift card currency. |

To see all the parameters you can override, please refer to the [SDK PHP Reference](https://developer.hipay.com/doc/hipay-enterprise-sdk-php/#psd2-and-strong-customer-authentication).
