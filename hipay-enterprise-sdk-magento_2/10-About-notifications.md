# Payment notifications

During the payment workflow, the order status is updated only through HiPay Enterprise notifications.   
The endpoint for notifications is `http://yourawesomewebsite.com/hipay/notify/index`.  
It is protected by an encrypted passphrase: don’t forget to enter it in your Magento Admin Panel ([Module configuration](#module-configuration)) and HiPay Enterprise back office ([Platform configuration](#platform-configuration)).  

For more information, please see the process in the [Notify](https://github.com/hipay/hipay-fullservice-sdk-magento2/blob/master/Model/Notify.php) model.

## Transaction statuses

All HiPay Enterprise transaction statuses are processed, but not all of them interact with Magento order statuses.  
This process only occurs when a notification is received.  

When a status is in process, a Magento payment transaction is created.  
Otherwise, we just add a new order history record with notification information.  

### Actions according to statuses

#### BLOCKED (110) and DENIED (111)

- The transaction order **“Denied”** status is created.
- The transaction order is closed.
- Any invoice in *Pending* status is cancelled.
- The order status changes to the status selected in configuration for the current payment method.


#### AUTHORIZED AND PENDING (112) and PENDING PAYMENT (200) 

- The transaction order **“Authorization”** status is created.
- The transaction order is *Pending*.
- The order status changes to `Pending Review`.
- The invoice is not created.

#### AUTHORIZATION REQUESTED (142)
- The transaction order is not created.
- The order status changes to `Authorization Requested`.
- Notification details are added to the order history.


#### REFUSED (113), AUTHORIZATION_REFUSED (163), CAPTURE_REFUSED (163)  

- The transaction order is *not created*. 
- The order is set to the configured status.
- If an invoice exists, it is cancelled.

#### CANCELLED (115)

- The transaction order is *not created*. 
- The order is set to the configured status.
- If an invoice exists, it is cancelled.

#### EXPIRED (114)

- The transaction order **“Void”** status is created if a parent transaction exists and is in status “Authorization”.
- The order is set to status `Processing` by Magento 2.


#### AUTHORIZED (116)  
- The transaction order **“Authorization”** status is created.
- The order status changes to `Authorized`.
- The invoice is not created.


#### CAPTURE REQUESTED (117)  

If the validation status is set to `Capture`:  
- the transaction order is not created,
- the order status changes to `Capture requested`,
- notification details are added to the order history.

Otherwise, if the validation status is set to `Capture requested`, please see the `Captured` status related actions below.


#### CAPTURED (118) and PARTIALLY CAPTURED (119)  

- The transaction order **“Captured”** status is created.
- The transaction order is closed.
- The order status changes to `Processing` or `Partially captured`.
- A complete/partial invoice is created.
- In case of a split payment, a new transaction is created.


#### REFUND REQUESTED (124)  

- The transaction order is not created.  
- The order status changes to `Refund requested`.
- Notification details are added to the order history.


#### REFUNDED (125) and PARTIALLY REFUNDED (126)  

  - The transaction order **“Captured”** status is created.
  - The transaction order is closed.
  - The order status changes to `Processing` or `Partially refunded`.
  - A complete/partial invoice is created.

#### REFUND REFUSED (117)  

- The transaction order is not created.  
- The order status changes to `Refund refused`.
- Notification details are added to the order history.

#### Other statuses
- *CREATED* (`101`)
- *CARD HOLDER ENROLLED* (`103`)
- *CARD HOLDER NOT ENROLLED* (`104`)
- *UNABLE TO AUTHENTICATE* (`105`)
- *CARD HOLDER AUTHENTICATED* (`106`)
- *AUTHENTICATION ATTEMPTED* (`107`)
- *COULD NOT AUTHENTICATE* (`108`)
- *AUTHENTICATION FAILED* (`109`)
- *COLLECTED* (`120`)
- *PARTIALLY COLLECTED* (`121`)
- *SETTLED* (`122`)
- *PARTIALLY SETTLED* (`123`)
- *CHARGED BACK* (`129`)
- *DEBITED* (`131`)
- *PARTIALLY DEBITED* (`132`)
- *AUTHENTICATION REQUESTED* (`140`)
- *AUTHENTICATED* (`141`)
- *ACQUIRER FOUND* (`150`)
- *ACQUIRER NOT FOUND* (`151`)
- *CARD HOLDER ENROLLMENT UNKNOWN* (`160`)
- *RISK ACCEPTED* (`161`)  
  - The transaction order is not created.
  - The order status *does not change*.
  - Notification details are added to the order history.
