# Capture And Refund

## Capture

### "Automatic" mode

When making a purchase in "Automatic" mode, the capture is automatically requested right after authorization. For more information about requesting a new order (operation), please refer to our [Developer Portal] (https://developer.hipay.com/doc-api/enterprise/gateway/#!/payments/requestNewOrder).

   - If the payment fails, the customer is redirected to an error page and the status is defined as "_Cancelled_".
   - If the payment is successful, the customer is redirected to the success page and the status is defined as "_Completed_".

### "Manual" mode

When making a purchase in "Manual" mode, the transaction status will be "_On-Hold_" until you ask for the capture. For more information about requesting a new order (operation), please refer to our [Developer Portal] (https://developer.hipay.com/doc-api/enterprise/gateway/#!/payments/requestNewOrder).
Customers are not charged directly: you have 7 days to "capture" the order and charge the customer. Otherwise, the order is canceled.

  - If the authorization fails, the customer is redirected to an error page and the status is defined as "_Cancelled_".
  - If the authorization is successful, the customer is redirected to the success page and the status is defined as "_On-Hold_".

#### Manual capture

## Refund
