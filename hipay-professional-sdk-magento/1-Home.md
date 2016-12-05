# HiPay Professional extension for Magento 1

# Prerequisites

## HiPay Professional account

In order to process payments with this module, you have to create a HiPay Professional account beforehand. To do so, go to the [HiPay Professional website](https://www.hipaydirect.com/).

You may need to use a test account in order to test the payment page and the module configuration. You can create a test account on [the HiPay Professional test website](https://test-www.hipaydirect.com/).

## Magento store

This module is compatible with Magento version 1.x. 

# Installation
Log into the administration area of your Magento online store.

In the main menu click on "Magento Connect" and then on "Magento Connect Manager".

Login with "Magento Connect Manager".

Depending on the version of Magento, you must use an extension key with Magento Connect Version 1 or 2:

- The extension key for Magento 1.4.1.1 and 1.4.2.0 (Magento Connect Version 1) is `magento-community/Hipay`
- The extension key for Magento 1.5+ (Magento Connect Version 2) is
`http://connect20.magentocommerce.com/community/Hipay`

Enter the extension key in the "Install New Extensions" field and click on the "Install" button.

Once the installation is successful, click on the "Return to admin" button in order to go back to the administration area.

To force Magento to use the new configuration, the cache has to be cleared.
To do so, go to the main menu, select "System" and click "cache management". Eventually, select the "Update" action on the right side and click on the "Run" button.

# Configuration

Log into the administration area of your Magento online store.
From the main menu, go to the "System" section, and click on "Configuration".

On the left side, you will now find the HiPay module in the "Sales" category.

In the HiPay module configuration section, you can:

- Register a new HiPay Professional merchant account
- Read installation instructions
- Enable the module for your shop
- Configure your payment countries (default setting being "All Allowed Countries")

In order for the module to work, please provide the module with your HiPay Professional merchant account information, including your HiPay Professional website ID.

You may also select the appropriate category for the products in your online shop in the "Order category" section.

Please note that if your shop items are age-restricted, you have to define the "age rating" option.

Finally, you may enter your email address in order to receive payment notifications.

When the configuration is done, click on "Save Configuration".

# Options 

## Test mode

To use the test mode, you have to use a HiPay Professional test account. 
Test transactions are charged with virtual amounts, so you can easily test as many transactions as necessary.

You can create a test account on [the HiPay Professional test website](https://test-www.hipaydirect.com/).
If you need help with the test mode, please submit a support ticket.

## Customization

You can customize the payment page with your online shop logo. To do so, enter the URL of your logo in the appropriate field. The size of your logo should not exceed 100x100 pixels.

## Sub accounts

You can receive transactions on different HiPay sub accounts depending on transaction parameters. To do so, just provide your HiPay Professional sub account IDs.