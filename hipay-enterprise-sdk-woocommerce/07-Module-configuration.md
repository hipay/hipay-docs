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

