## Custom data

The Magento HiPay module allows you to send custom data with transactions and see this information in your HiPay Enterprise back office.

If you want to develop your own custom data, please take the *CustomData.php* file in the "extra" folder and put it in the HiPay module sources in the "/AlloPass/Hipay/Helper" folder.
You have to implement the "getCustomData" method and return an array with your data.

## Frequent issues

If you have problems with authorization and capture notifications, please check if an observer is listening to the events **sales_order_save_after** or **sales_order_invoice_save_after**, and make sure that there is no exception in them.

If an exception is raised during the chain of observers, the HiPay notification will be in error and the order status will not change.
