# Capture and refund

## Capture

### "Automatic" mode

When making a purchase in "automatic" mode, the capture is automatically requested right after authorization. For more information about requesting a new order (operation), please refer to our [Developer Portal](/doc-api/enterprise/gateway/#!/payments/requestNewOrder).

   - If the payment fails, the customer is redirected to an error page and the status is defined as "_Cancelled_".
   - If the payment is successful, the customer is redirected to the success page and the status is defined as "_Completed_".

### "Manual" mode

When making a purchase in "manual" mode, the transaction status will be "_On-Hold_" until you ask for the capture.
The customer is not charged directly: you have seven days to "capture" the order and charge the customer. Otherwise, the order is cancelled.

  - If the authorization fails, the customer is redirected to an error page and the status is defined as **"_Cancelled_"**.
  - If the authorization is successful, the customer is redirected to the success page and the status is defined as **"_On-Hold_"**.

#### Manual capture

To capture a transaction in your WooCommerce back office, go to your list of orders and select any one. 
The order status should be **“On-Hold”**.

Manual and partial captures are not native WooCommerce features.
If you are familiar with the WooCommerce system for captures, the principle of capture is based on manual capture.

If the order is in the correct status and if the amount already captured does not reach the total of the order, you should have a  "Capture" button to the right of the "Refund" button in the panel for the item.

Click on the "_Capture_" button.
Specify the quantity of product(s) to be captured in the text box(es) displayed for each item line.
The capture amount will be automatically adjusted based on the product(s) captured.


You can also capture delivery costs: make sure to fill in the tax.

Then click on "_Capture €XX via HiPay Enterprise Credit Card_".

![legend](images/capture-item.png)

Once the capture is made, a line appears for each item, indicating "Captured with HiPay" or the
quantities and amounts captured.
The status of the order will evolve to:
- **Partially captured** if you have not captured all of the items of the order,
- **Processing** if you have captured them all.

"Partially captured" is a status added by the HiPay extension that allows you to easily identify
orders partially captured.
It is similar to WooCommerce's "On-Hold" status. Therefore, all the actions relating to a change of
status will be performed when the order changes to be processed are complete.

![legend](images/captured-item-step2.png)

## Refund

The plugin supports the WooCommerce native feature for refunds. 
To make a refund: 

1. Go to: "_WooCommerce > Orders_".
2. Select the order you wish to refund.
3. Click on the grey Refund button.
4. Specify the quantity of product(s) to be refunded in the text box(es) appearing for each item line. The refund amount will be automatically adjusted based on the products refunded. If inventory levels are not managed, you can also simply enter the Refund amount, without adjusting the product quantity. If the quantities of items are not set when issuing a refund, the order will not be marked as refunded and the email that is sent will say “partial refund.”
5. Add refund notes, if desired.
6. Click on "Refund $X via HiPay".

The status of the order will evolve to:
- **Partially refunded** if you have not refunded all of the items of the order,
- **Refunded** if you have refunded them all.

"Partially refunded" is a status added by the HiPay extension that allows you to easily identify
orders partially refunded.

You can also make a refund directly from your HiPay Enterprise back office. The order will then be automatically updated in your WooCommerce back office. 
PLEASE NOTE: It only works for total refunds (not partial).

For more information, please refer to the WooCommerce documentation on [Refunds](https://docs.woocommerce.com/document/woocommerce-refunds/).
