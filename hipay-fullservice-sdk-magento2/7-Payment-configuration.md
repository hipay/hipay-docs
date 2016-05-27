# "Sales" mode (direct capture)

When making a purchase with the "sale" mode, the capture is automatically requested right after authorization. Please refer to the HiPayTPP-GatewayAPI documentation, chapter 3.1 "Request a New Order" (operation).

If the payment fails, the customer is redirected to an error page and the status is defined as "_CANCELED_".

If successful, the customer is redirected to the success page and the status is defined as "_CAPTURE REQUESTED_".

# "Authorization" mode

When making a purchase with the "authorization" mode, the transaction status will be "_AUTHORIZED_" until you ask for the capture. Please refer to the HiPayTPP-GatewayAPI documentation, chapter 3.1 "Request a New Order" (operation).

The customer is not charged directly: you have 7 days to "capture" this order and charge the customer. Otherwise, the order is cancelled.

If the authorization fails, the customer is redirected to an error page and the status is defined as "_CANCELED_".

If the authorization is successful, the customer is redirected to the success page and the status is defined as "_AUTHORIZED_".

To capture the transaction, go to "_Back Office -> Sales -> Orders -> view order_" to see the details of the order and choose to make an "invoice".  
On invoice edititon, select "Capture online" and save your invoice.  
NOTE: if you select custom items quantity, you can capture partially.



You can also do the “capture” directly in your HiPay Fullservice back office. The order will be updated automatically in your Magento2 back office.  
NOTE: It's work only for total capture (not partially)!

# Refund

Some HiPay Fullservice payment methods allow for a refund. To do it, go to "_Back Office -> Sales -> Orders -> view order_" to see the details of the order and choose an invoice to refund and click on "_Credit memo_" button.  
On Credit memo edititon, select "Refund online" and save your credit memo.  
NOTE: if you select custom items quantity, you can refund partially.

You can also do the "refund" directly in your HiPay Fullservice back office. The order will be updated automatically in your Magento2 back office.  
NOTE: It's work only for total capture (not partially)!

# One-Click (only available for credit card payment methods)

If the One-Click option is enabled, your system will create an "alias" for the credit card. Customers will thus be able to use a saved credit card for their second transaction and won't need to fill in all the payment data again.