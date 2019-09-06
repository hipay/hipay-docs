# Global plugin and Credit card configuration

## Configuration interface

To configure the HiPay Enterprise plugin, click on "_WooCommerce -> Settings -> Payments_" in your WordPress dashboard. Then click on "_HiPay Enterprise Credit Card_":

![legend](images/plugin-configuration.png)


First, you must activate the payment methods by clicking on the toggle.

The new configuration interface of the plugin is comprised of four tabs.

- **Plugin settings**: Configure API IDs to run HiPay services. You can also configure whether the plugin is in Production or Test mode. 
- **Payment methods**: Configure the payment methods to be activated and how payments must be processed: Hosted Fields or Hosted Page.
- **Fraud**: Configure recipients' email addresses for "challenged" transaction notifications.
- **FAQ**: Find answers to frequently asked questions on how to use the plugin.

## Plugin settings

This tab allows you to configure the API IDs required to run HiPay services.

It is comprised of the following components that you must set up first and foremost.

#### Plugin mode

This setting is very important as it allows you to define whether payments will be processed on the HiPay Test or Production platform.
By default, the plugin is in Test mode, which means that payments will not be actually charged.

We strongly advise you to run tests before launching your site in Production mode.

![legend](images/plugin-mode.png)

#### Production configuration

Use this interface to specify the credentials linked to your HiPay account.
**These identifiers are used if your plugin is configured in Production mode.**

Generated in your [HiPay Enterprise back office](https://merchant.hipay-tpp.com) (go to "Integration” => “Security Settings” => “Api credentials” => “Credentials accessibility”), these API credentials are required to use the HiPay Enterprise plugin.

For more information about credentials generation, please refer to the section on [Credentials](#prerequisites-and-recommendations-credentials).

**Account (Private)**

Private credentials are used to process payments through the HiPay APIs. These identifiers are mandatory.


| Name               | Description |
|:------------|:------------|
| **Username**                      | Your HiPay Enterprise production account API username      |
| **Password**                      | Your HiPay Enterprise production account API password     |
| **Secret passphrase**               | Your HiPay Enterprise secret passphrase   |


**Tokenization (Public)**

Public credentials are used as part of the JavaScript tokenization. These identifiers are to be specified only if you want to use the plugin in Hosted Fields mode.


| Name               | Description |
|:------------|:------------|
| **Username**                      | Your HiPay Enterprise production account API username      |
| **Password**                      | Your HiPay Enterprise production account API password    |

#### Test configuration

This interface is similar to the Production configuration.

**These identifiers are used if your plugin is configured in Test mode.**

# PSD2 and Strong Customer Authentication

Given the strong growth of the e-commerce in Europe, the Payment Services Directive (PSD2) redefines the security standards for online payments aiming to increase the security during the payment process, while fighting more actively against fraud attempts. For more details on the regulations, we invite you to read the [documentation provided by Hipay](https://developer.hipay.com/psd2-and-strong-customer-authentication-3-d-secure-2-compliance-and-guidance/).

As of September 14, 2019, the issuer will decide if a payment is processed depending on the analysis of more than 150 data collected during the purchasing process. Thanks to our Woocommerce module we handle most of the data without you having to develop anything. You can see all the new parameters on [our explorer API](https://developer.hipay.com/doc-api/enterprise/gateway/).

## Adding or overriding PSD2 data

The accuracy of the information sent is key for making sure that your customers have a frictionless payment process. That’s why we provide you the possibility to add or override all the information related to the DSP2.

We provide you the “hipay_wc_before_request” filter that can be hooked by calling [add_filter()](https://developer.wordpress.org/reference/functions/add_filter/).

It is called with two parameters: the request sent to the HiPay API and the current customer order.
 
You can either implement the filter handler in your own modules/theme or use the module that we provide. You can find this additional module on our [github repository](https://github.com/hipay/hipay-enterprise-sdk-woocommerce-data). 
 
You can install this module in a classic way, then directly modify the file “class-wc-hipayenterprise-data.php” and the method “beforeMapRequest” to add your information.
 
Be careful ! If there is an update of the data module, remember to save your data so that it is not overwritten.

Finally, although we do our best to retrieve all relevant data for you, we are not capable of getting the following data as it depends on the modules installed on your CMS or your workflow. 

### Customer

| Field | Comment |
| --- | --- |
| **password_change** | By default, magento does not save the password change dates for a user.<br/>However, you can implement this feature and add this information in the observer. |

### Merchant risk statement

| Field |  | Comment |
| --- | --- | --- |
| **delivery_time_frame** |  | The field is managed but only for virtual products. <br/>We send “1 = Electronic delivery” if it is downloadable or  virtual product.<br/><br/>Depending on your carrier and delivery methods, you can refine this data. <br/><br/>Possible values: <br/><br/>1 = Electronic delivery<br/>2 = Same day shipping<br/>3 = Overnight shipping<br/>4 = Two-day or more shipping |
| **shipping_indicator** |  | This fields is managed but you can refine the value for dematerialized products.<br/><br/>If the basket contains only virtual products, please provide:<br/><br/>5 = Digital goods<br/>6 = Travel and event tickets, no shipping<br/>7 = Other (gaming, digital services without shipping, e-media subscription) |
| **gift_card** |  | **If you sell gift card products** |
|  | **amount** | Collect the amount of gift card type gift cards purchased. |
|  | **count** | Collect the count of gift card type gift cards purchased. |
|  | **currency** | Collect the currency of gift card type gift cards purchased. |

If you want to see all the parameters you are able to override, please refer to the [SDK PHP Reference](https://developer.hipay.com/doc/hipay-enterprise-sdk-php/#psd2-and-strong-customer-authentication).
