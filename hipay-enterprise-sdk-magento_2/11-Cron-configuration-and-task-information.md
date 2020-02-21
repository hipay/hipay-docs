# Cron configuration and task information

## Configuration

Several HiPay Enterprise features require at least one cron job, which schedules activities to occur in the future.  

A partial list of these activities follows:
- clear pending orders,
- pay installments for split payments.

Therefore, the Magento cron:run command must be added to your crontab.

To do so, please refer to the [Magento 2 Configuration Guide](http://devdocs.magento.com/guides/v2.0/config-guide/cli/config-cli-subcommands-cron.html).

## Clear pending orders

This cron job runs every 15 minutes.
If configuration is enabled for any payment method, this cron job cancels all the orders which are paid with HiPay Enterprise and are in `Pending` status (API mode) or `Pending Payment` status (HOSTED mode) for more than 30 minutes.

## Pay installments for split payments

This cron job runs once a day.
For each split payment in `Pending` status, this cron job sends an API query to pay each installment.
