# Objective

This integration intends to facilitate cash-out operations between HiPay and Mirakl. This software is based on the [Silex PHP micro-framework](http://silex.sensiolabs.org/) and integrates the [HiPay Wallet cash-out connector for Mirakl][repo-lib] PHP library.

# Stakeholders

| **Name** | **Nature** | **Description** |
| --- | --- | --- |
| HiPay | Third-party and web service provider | Payment solution handling payment processing between the operator and the different shops available on your marketplace |
| Mirakl | Third-party and web service provider | Marketplace solution handling the shops, with a read-only web service |
| Cron | System service | Unix task scheduler, which may be replaced by a Windows alternative if needed (not tested with Windows) |
| Integrator | Human | The developer, who wishes to easily interface HiPay and Mirakl |

# Prerequisites and recommendations

## General

- The `config/parameters.yml` file must be a YAML file with one top key named `parameters`. A default example can be seen in the `config/parameters.yml.dist` file.
- The `parameters.yml` file should not be committed nor transmitted to any third party.

## System

- You must have at least PHP 5.3.9. The library has dependencies that need at least this version of PHP.
- You should have Composer, which is the best way to install the library or do the integration.
- You should use MySQL. Even though the integration should function with most RDBMSs, it was only tested with MySQL 5.5 through Doctrine.
- A web server is required, with a URL accessible by HiPay with the HTTP verb POST so that server-to-server notifications can be sent by HiPay. It can be Apache, Nginx or any other choice, as long as the other mandatory requirements are met.

## Mirakl

- Each shop must have a unique email address, as a HiPay Wallet account is created with and tied to an email address.
- Even though Mirakl does not require filling in the phone number section, it must be completed to create a HiPay Wallet account.
- Even though Mirakl only requires filling in the IBAN and the BIC in the banking information section, all form fields must be completed to provide banking information to HiPay.
- Only alphanumeric characters should be used for filling in the fields, especially for banking information, as HiPay only accepts this category of characters.
- The version of the API should be at least 3.\*.\*.
- Should the API version be higher, as long as the concerned API being called remains backwards compatible, there won't be any problem.

## HiPay

- Some operations require HiPay's assistance: please contact HiPay's Business IT Services for technical support on the [HiPay Support Center][hipay-help].
- A HiPay Wallet account for the operator must be created beforehand. An email address that will not be used for another shop on the marketplace is required.
- A good understanding of APIs for cash-out transactions may be useful: please refer to the *HiPay Marketplace - APIs overview* guide.

# Next step
When you're done with this part, go to the next section: [[Mirakl account configuration]]

[repo-lib]: https://github.com/hipay/hipay-wallet-cashout-mirakl-library

[hipay-help]: http://help.hipay.com
