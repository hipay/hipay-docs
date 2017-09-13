# Cron configuration and task information

## Configuration

Several HiPay Enterprise features require at least one cron job, which schedules activities to occur in the future.  
A partial list of these activities follows:
- Clean pending orders,
- Pay installments for split payments.

Therefore, the Magento cron:run command must be added to your crontab.

To do so, please refer to the [Magento 2 guide](http://devdocs.magento.com/guides/v2.0/config-guide/cli/config-cli-subcommands-cron.html).

## Clear pending orders

Run every 15 minutes.
If configuration is enabled for any payment method, this task cancels all the orders which are paid with HiPay Enterprise and are in `Pending` status (API mode) or `Pending Payment` status (HOSTED mode) for more than 30 minutes.

## Pay installments for split payments

Run once a day.
For each split payment in pending status, this task sends an API query to pay each installment.
