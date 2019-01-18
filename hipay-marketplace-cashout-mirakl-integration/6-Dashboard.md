# Dashboard

The HiPay Marketplace cash-out integration for Mirakl provides you with a dashboard, which aims to:

- Inform you of your vendors' statuses, transfers and withdrawals,
- Give you the possibility to rerun a previously failed action,
- Centralize all information relating to interactions between HiPay and Mirakl.

You can access the dashboard at the following URL: `https://cashout.merchant-example.com/index.php/dashboard` (the URL depends on your web server software configuration).

## Login

Use the HiPay web service credentials for the technical account to log in to the dashboard.

## Vendor management

Each processed vendor is saved in the HiPay Marketplace cash-out integration for Mirakl database.

Vendor logs are displayed with the following information:

|Name       | Description  |
|-----------|--------------|
| Mirakl ID  | Store ID in the Mirakl back office |
| Login  | Login for this store in the HiPay back office |
| Status  | Status of the vendors in the HiPay Marketplace cash-out integration for Mirakl. Possible values are `success` or `warning`. |
| Wallet account status  | Status of the HiPay account. Possible values are `created`, `not created`, `identified` or `not identified`.  |
| HiPay ID  | ID of the wallet account |
| Date  | Date of log creation |
| Documents sent  | Click on `Show details` to see the status of sent documents. |

You can filter vendor logs by `status`, `wallet account status` and `time period`.

If the `status` of a vendor is `warning`, you can get the details of the error by clicking on the `Show message` tooltip.

![vendor-warning](images/vendor-warning.png)

## Transfer & withdrawal management

Each processed transfer and withdrawal is saved in the HiPay Marketplace cash-out integration for Mirakl database.

Operation logs are displayed with the following information: 

|Name       | Description  |
|-----------|--------------|
| Mirakl ID  | Store ID in the Mirakl back office |
| HiPay ID  | ID of the wallet account |
| Payment order Id  | ID of the voucher in the Mirakl back office |
| Amount | Amount of the operation |
| Origin amount | Amount of the operation before adjustment of negative operations |
| Transfer status  | Status of the transfer for this operation. Possible values are `OK`, `KO (insufficient funds)` or `KO`. |
| Withdraw status  | Status of the withdrawal for this operation. Possible values are `OK (requested)`, `OK`, `KO (failed)`, `KO (insufficient funds)` or `KO (canceled)`.  |
| Balance  | Balance of the wallet account |
| Date  | Date of the last update for this operation |

You can filter transfer & withdrawal logs by `Transfer status` and `Withdraw status`.

If the `Transfer status` or `Withdraw status` of an operation is `KO`, you can get the details of the error by clicking on the `Show message` tooltip.

## Logs

Each application log is saved in the HiPay Marketplace cash-out integration for Mirakl database.

Logs are displayed with the following information: 

|Name       | Description  |
|-----------|--------------|
| Date  | Date of log creation |
| Information type  | Log level. Possible values are `DEBUG`, `INFO`, `NOTICE`, `WARNING`, `ERROR`, `CRITICAL`, `ALERT` or `EMERGENCY`. |
| Action  | Action linked to this log line |
| Mirakl ID  | Store ID in the Mirakl back office |
| Message  | Log message |

You can filter logs by `Information type` and `Time period`.

You can export logs to a CSV file.

## Settings

On the `Settings` page, you will be able to: 

- Run commands,
- Retrieve lists of executed batches,
- Get technical information,
- Update settings,
- Update the application.

### Run commands
You can run the following commands through the `Fix errors` form:

- `Wallet account creation` corresponding to the [vendor:process](#general-usage-available-commands-vendor-processing) command,
- `Generate operations` corresponding to the [cashout:generate:process](#general-usage-available-commands-cash-out-generation) command,
- `Transfer` corresponding to the [cashout:transfer](#general-usage-available-commands-transfer-processing) command,
- `Withdraw` corresponding to the [cashout:withdraw](#general-usage-available-commands-withdrawal-processing) command.

For `Wallet account creation` and `Generate operations` commands, you can specify a date from which vendors or transactions will be retrieved from the Mirakl platform.

If you leave it empty, the default behaviour will be as follows:

- For `Wallet account creation`, the batch will retrieve all vendors from Mirakl;
- For `Generate operations`, the batch will retrieve transactions with arguments from your parameters.yml file ([please see this section](#parameters-mirakl-cycles)).

### Retrieve lists of executed batches

This section displays executed commands with their starting date and status.

If an error occurs during the command, an error message can be displayed in this list.

The list is refreshed every 30 seconds.

### Update settings

You can update some of the settings located in the parameters.yml file directly through the "Settings" form.

You can update the following parameters:

|Name       | Description  |
|-----------|--------------|
| Github token  | GitHub personal access token, necessary for the self-update feature. You can find instructions on how to create your GitHub token [here](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/#creating-a-token). Please follow steps 1 to 9 in the "Creating a token" section. |
| Minimal log level alert  | Minimum logging level at which an email will be sent to a specified email address |

### Update the application

This section allows you to update your application.

If a new version of the HiPay Marketplace cash-out integration project has been released, you will be able to update your application directly from your browser.
 
If a new security patch for the HiPay Marketplace cash-out library project has been released, you will be able to update your library directly from your browser. 

You will be redirected to a page describing actions performed by the update process. This process may take a while: please do not refresh the web page.

Please note:
- This update process requires specific rights on your [install folders](#installation). If you don't want to set these rights, please see the [update section](#update).
- This update process requires that the "Github token" setting has been set. Please refer to [this section](#dashboard-settings-update-settings) to set your GitHub token.
