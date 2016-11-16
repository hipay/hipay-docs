# Payment notifications

During the payment workflow, the order status is updated only through HiPay Fullservice notifications.   
The endpoint for notifications is `http://yourawesomewebsite.com/hipay/notify/index`.  
It is protected by an encrypted passphrase: don't forget to enter it in your Magento Admin Panel ([[Module configuration]]) and HiPay Fullservice back office ([[Platform configuration]]).  

For more information, please see the process in the [Notify](https://github.com/hipay/hipay-fullservice-sdk-magento2/blob/master/src/Model/Notify.php) model.

## Transaction statuses

All HiPay Fullservice transaction statuses are processed, but not all of them interact with Magento order statuses.  
This process only occurs when a notification is received.  

When a status is in process, a Magento payment transaction is created.  
Otherwise, we just add a new order history record with notification information.  

### Actions according to statuses:

#### BLOCKED (110) and DENIED (111)

- Transaction order **"Denied"** status is created
- Transaction order is closed
- Invoice in *pending* status, if any, is cancelled
- Order status changes to the status selected in configuration for the current payment method


#### AUTHORIZED AND PENDING (112) and PENDING PAYMENT (200) 

- Transaction order **"Authorization"** status is created
- Transaction order is *pending*
- Transaction order is not closed
- Order status changes to `Pending Review`
- Invoice is not created

#### AUTHORIZATION REQUESTED (142)
- Transaction order is not created
- Order status changes to `Authorization Requested`
- Notification details are added to order history


#### REFUSED (113), AUTHORIZATION_REFUSED (163), CAPTURE_REFUSED (163)  

- Transaction order is *not created* 
- Order is set to the status configured
- If an invoice exists, it is cancelled

#### CANCELLED (115)

- Transaction order is *not created* 
- Order is set to the status configured
- If an invoice exists, it is cancelled

#### EXPIRED (114)

- Transaction order **"Void"** status is created if a parent transaction exists and is Authorization
- Order is set to status `Processing` by Magento 2


#### AUTHORIZED (116)  
- Transaction order **"Authorization"** status is created
- Transaction order is open
- Order status changes to `Authorized`
- Invoice is not created


#### CAPTURE REQUESTED (117)  

If the validation status is set to `Capture`:  
- Transaction order is not created
- Order status changes to `Capture requested`
- Notification details are added to order history

Otherwise, if the validation status is set to `Capture requested`, please see `Captured` Status related actions below.


#### CAPTURED (118) and PARTIALLY CAPTURED (119)  

- Transaction order **"Captured"** status is created
- Transaction order is closed
- Order status changes to `Processing` or `Partially captured`
- Complete/partial invoice is created
- In case of a split payment, a new transaction is saved.


#### REFUND REQUESTED (124)  

- Transaction order is not created  
- Order status changes to `Refund requested`
- Notification details are added to order history


#### REFUNDED (125) and PARTIALLY REFUNDED (126)  

  - Transaction order **"Captured"** status is created
  - Transaction order is closed
  - Order status changes to `Processing` or `Partially refunded`
  - Complete/partial invoice is created

#### REFUND REFUSED (117)  

- Transaction order is not created  
- Order status changes to `Refund refused`
- Notification details are added to order history

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
  - Transaction order is not created
  - Order status *does not change*
  - Notification details are added to order history
