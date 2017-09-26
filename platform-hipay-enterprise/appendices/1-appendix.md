# Transaction statuses

##Sent by server-to-server notification

Here is a list of the transaction statuses sent by server-to-server notification.

| **Status** | **Message** | **Description** |
| --- | --- | --- |
| `109` | Authentication Failed | The cardholder&#39;s authentication failed.The authorization request should not be submitted._An authentication failure may be a possible indication of a fraudulent user._ |
| `110` | Blocked | The transaction has been rejected for reasons of suspected fraud. |
| `111` | Denied | The merchant denied the payment attempt. After reviewing the fraud screening result, the merchant decided to decline the payment. |
| `112` | Authorized and Pending | The payment has been challenged by the fraud rule set and is pending. |
| `113` | Refused | The financial institution refused to authorize the payment.The refusal reasons can be: an exceeded credit limit,      an incorrect expiry date or insufficient balance, or many others, depending on the selected payment method. |
| `114` | Expired | The validity period of the payment authorization has expired.This happens when no capture request is submitted for an authorized payment typically within 7 days after authorization._Note: Depending on the customer&#39;s issuing bank,           the authorization validity period may last from 1 to 5 days for a debit card and up to 30 days for a credit card._ |
| `115` | Cancelled | The merchant has cancelled the payment attempt.Only payments with status &quot;Authorized&quot; that have not yet reached the status &quot;Captured&quot; can be cancelled. In case of a credit card payment, cancelling the transaction consists in voiding the authorization. |
| `116` | Authorized | The financial institution has approved the payment.In the case of a credit card payment, funds are &quot;held&quot; and deducted from the customer&#39;s credit limit (or bank balance, in the case of a debit card), but are not yet transferred to the merchant. In the case of bank transfers and some other payment methods, the payment immediately reaches the &quot;Captured&quot; status after being set to &quot;Authorized&quot;. |
| `117` | Capture Requested | A capture request has been sent to the financial institution. |
| `118` | Captured | The financial institution has processed the payment.The funds will be transferred to HiPay Enterprise before being settled to your bank account. Authorized payments can be captured as long as the authorization has not expired. Some payment methods, like bank transfers or direct debits, reach the &quot;Captured&quot; status straight away after being authorized. |
| `119` | Partially Captured | The financial institution has processed part of the payment.If only part of the order can be shipped, it is allowed to capture an amount equal to the shipped part of the order. This is called a partial capture._Please note: As dictated by all credit card companies, it is not allowed for a merchant to capture a payment before shipping has been completed. Merchants should start shipping the order once the status &quot;Authorized&quot; has been reached!_ |
| `124` | Refund Requested | A refund request has been sent to the financial institution. |
| `125` | Refunded | The payment was refunded.A payment reaches the &quot;Refunded&quot; status when the financial institution has processed the refund and the amount has been transferred to the customer&#39;s account. The amount will be deducted from the next total amount to be paid out to the merchant. |
| `126` | Partially Refunded | The payment was partially refunded. |
| `129` | Charged Back | The payment was charged back.The cardholder has reversed a capture processed by their bank or credit card company. For instance, the cardholder has contacted their credit card company and denied having made the transaction. The credit card company has then revoked the payment already captured. Please note the legal difference between the customer (who ordered the goods) and the cardholder (who owns the credit card and ends up paying for the order).Generally, chargebacks only occur incidentally. When they do, contacting the customer can often solve the situation. Occasionally, it is an indication of credit card fraud. |
| `142` | Authorization Requested | The payment method used requires an authorization request; the request was sent and the system is waiting for the approval of the financial institution. |
| `143` | Authorization Cancelled | The authorization has been cancelled |
| `165` | Refund Refused | The refund operation was refused by the financial institution. |
| `166` | Issuer Credited | The issuer&#39;s card has been credited. |
| `173` | Capture Refused | The capture was refused by the financial institution. |
| `200` | Pending Payment | The transaction request was submitted to the acquirer but the response is not yet available. |

##Not sent by server-to-server notification

The table below lists all informational transaction statuses (that are not sent by server-to-server notification).

| **Status** | **Message** | **Description** |
| --- | --- | --- |
| `101` | Created | The payment attempt was created. |
| `103` | Cardholder Enrolled | The card is enrolled in the 3-D Secure program. The merchant has to redirect the cardholder to the authentication pages of the card issuer. |
| `104` | Cardholder Not Enrolled | The card is not enrolled in the 3-D Secure program. |
| `105` | Unable to Authenticate | The card issuer is unable to complete the authentication request. |
| `106` | Cardholder Authenticated | The cardholder was successfully authenticated in the 3-D Secure program. |
| `107` | Authentication Attempted | The merchant has attempted to authenticate the cardholder in the 3-D Secure program and either the card issuer or the cardholder is not enrolled. |
| `108` | Could Not Authenticate | The card issuer is not able to complete the authentication request. |
| `120` | Collected | The funds have been made available for remittance to the merchant.A payment with the &quot;Collected&quot; status is ready to be paid out. HiPay Enterprise will either transfer the amount to your bank account within the next few days (depending on your settlement frequency), or the amount has already been transferred to your bank account. |
| `121` | Partially Collected | A part of the transaction has been collected. |
| `122` | Settled | The financial operations linked to this transaction are closed. Funds have been debited or credited from your HiPay merchant account. |
| `123` | Partially Settled | A part of the financial operations linked to this transaction is closed. |
| `131` | Debited | The acquirer has informed us that a debit linked to the transaction is going to be applied. |
| `132` | Partially Debited | The acquirer has informed us that a partial debit linked to the transaction is going to be applied. |
| `140` | Authentication Requested | The payment method used requires authentication; authentication request has been sent and the system is waiting for an action from the customer. |
| `141` | Authenticated | The payment method used requires authentication and it was successful. |
| `150` | Acquirer Found | The acquirer&#39;s payment route has been found. |
| `151` | Acquirer not Found | The acquirer&#39;s payment route has not been found. |
| `160` | Cardholder Enrollment Unknown | Unable to verify if the card is enrolled in the 3-D Secure program. |
| `161` | Risk Accepted | The payment has been accepted by the fraud rule set. |
| `163` | Authorization Refused | The authorization was refused by the financial institution. |

## Transaction Life Cycle

The life cycle of a transaction processed by the HiPay Enterprise payment platform is characterised by the different events that mark a change in the status of the transaction.These events and the resulting changes in transaction status play a crucial role in the payment process. All the financial reporting is based on the status of transactions and any possible action for a transaction, whether performed by the merchant, the financial institution or the payment platform, depends on the actual status.

The following diagram shows the typical flow of a transaction through the different main transaction statuses.

*Typical transaction workflow*

![alt text](images/diagram.png "Logo Title Text 1")

--------

#Address Verification Service

The Address Verification Service (AVS) allows e-commerce merchants to check a cardholder&#39;s billing address. AVS provides merchants with a key indicator that helps verify whether or not a transaction is valid.
The table below lists the available result codes as returned in the API response messages.

| **Code** | **Message** | **Description** |
| --- | --- | --- |
| _Blank_ | Not applicable | No AVS response was obtained (Default value). |
| `Y` | Exact Match | Street addresses and postal codes match. |
| `A` | Partial Match | Street addresses match; but postal codes don&#39;t.Either the request does not include the postal codes or postal codes are not verified due to incompatible formats. |
| `P` | Partial Match | Postal codes match; but street addresses don&#39;t.Either the request does not include the street addresses or street addresses are not verified due to incompatible formats. |
| `N` | No Match | Neither the street addresses nor the postal codes match. |
| `C` | Not Compatible | Street addresses and postal codes are not verified due to incompatible formats. |
| `E` | Not Allowed | AVS data is invalid or AVS is not allowed for this card type. |
| `U` | Unavailable | Address information is unavailable for that account number, or the card issuer does not support AVS.|
| `R` | Retry | The issuer&#39;s authorization system is unavailable; please try again later. |
| `S` | Not Supported | The card issuer does not support AVS. |

--------

#Card Verification Code

The CVC is available on the following credit and debit cards: Visa (Card Verification Value CVV2), MasterCard (Card Validation Code CVC2), Maestro, Diners Club, Discover (Card Identification Number CID), and American Express (Card Identification Number CID).

When the acquirer enables you to perform a CVC check, a result code (returned along with the response to the authorization request) informs you on the CVC check status.You then evaluate the CVC result code that you received with the transaction authorization and take appropriate action based on all transaction characteristics.

**Warning**: Only a few acquirers return specific CVC check results. For most acquirers, the CVC is assumed to be correct                          if the transaction is successfully authorized.

The table below lists the available result codes as returned in the API response messages.

| **Code** | **Message** | **Description** |
| --- | --- | --- |
| _Blank_ | Not applicable | The CVC check was not possible. |
| `M` | Match | The CVC matches. |
| `N` | No Match | The CVC does not match. |
| `P` | Not processed | The CVC request was not processed. |
| `S` | Missing | The CVC should be on the card, but the cardholder has reported that it is not. |
| `U` | Not supported | The card issuer does not support CVC. |

--------

#Payment products

##Credit cards

| **Product code** | **Brand name** | **3DS** | **Refund** | **Recurring** | **Comment** |
| --- | --- | --- | --- | --- | --- |
| `american-express` | American Express | No | Yes | Yes |   |
| `cb` | Carte Bancaire | Yes | Yes | Yes | Accepted cards:Visa, MasterCardOnly available in France |
| `mastercard` | MasterCard | Yes | Yes | Yes |   |
| `visa` | Visa | Yes | Yes | Yes |   |
| `3xcb` | 3x Carte Bancaire | Yes | Yes | No | Only available in France |
| `4xcb` | 4x Carte Bancaire | Yes | Yes | No | Only available in France |
| `3xcb-no-fees` | 3x Carte Bancaire sans frais | Yes | Yes | No | Only available in France |
| `4xcb-no-fees` | 4x Carte Bancaire sans frais | Yes | Yes | No | Only available in France |

##Debit cards

| **Product code** | **Brand name** | **3DS** | **Refund** | **Recurring** | **Comment** |
| --- | --- | --- | --- | --- | --- |
| `bcmc` | Bancontact | Yes | No | No | Only available in Belgium |
| `maestro` | Maestro | Yes | Yes | No | MasterCard requires that your website implements 3-D Secure authentication to accept Maestro payments. |

Debit cards (Category code: debit-card)

##Real-time banking

| **Product code** | **Brand name** | **Refund available?** | **Recurring available?** | **Comment** |
| --- | --- | --- | --- | --- |
| `bank-transfer` | TrustPay Banking | No | No |   |
| `dexia-directnet` | Belfius Direct Net | No | No |   |
| `giropay` | Giropay | No | No | Only available in Germany |
| `ideal` | iDEAL | No | No |   |
| `ing-homepay` | ING Home&#39;Pay | No | No |   |
| `klarna` | Klarna | No | No |   |
| `przelewy24` | Przelewy24 | Yes | No | Only available in PLN currency |
| `sisal` | Sisal | No | No | Max amount EUR 1,000 |
| `sofort-uberweisung` | SOFORT Überweisung | No | No |   |
| `sdd` | SEPA Direct Debit | No | Yes | &nbsp; |

##E-wallets

| **Product code** | **Brand name** | **Refund available?** | **Recurring available?** | **Comment** |
| --- | --- | --- | --- | --- |
| `qiwi-wallet` | VISA QIWI Wallet | No | No | Only available in RUB currency |
| `webmoney-transfer` | WebMoney Transfer | No | No | Only available in RUB currency |
| `yandex` | Yandex.Money | No | No | Only available in RUB currency |
| `paypal` | PayPal | Yes | No | &nbsp; |

--------

#Decline reasons and error codes

Should an error occur, here is a list of all the possible related codes and messages as well as an indication on how to correct it.

##Configuration errors

| **Code** | **Message** | **Description** | **How to correct this error** |
| --- | --- | --- | --- |
| `1000001` | Incorrect Credentials | Incorrect username and/or password | This error can be caused by an incorrect API username or an incorrect API password. Make sure that all these values are correct. For your security, HiPay Enterprise does not report exactly which value might be erroneous. |
| `1000002` | Incorrect Signature | The signature that was sent does not match the requested format. |   |
| `1000003` | Account Not Active | The account is inactive. |   |
| `1000004` | Account Locked | The account is locked. |   |
| `1000005` | Insufficient Permissions | You do not have permission to make this API call. |   |
| `1000006` | Forbidden Access | The API access is disabled for this account. |   |
| `1000007` | Unsupported Version | The API version is not supported. |   |
| `1000008` | Temporarily Unavailable | The gateway is temporarily unavailable. | Please try again later. |
| `1000009` | Not Allowed | The request was rejected due to IP restriction. | Make sure that the IP addresses of your servers are configured for your account. |

##Validation errors

| **Code** | **Message** | **Description** |
| --- | --- | --- |
| `1010001` | Method Not Allowed | The specified HTTP method is not allowed for this API call. |
| `1010101` | Required Parameter Missing | A required parameter is missing. |
| `1010201` | Invalid Parameter | A parameter is in an invalid format. |
| `1010202` | Invalid Parameter | A parameter value exceeds the maximum number of characters allowed. |
| `1010203` | Invalid Parameter | A parameter contains non-alphabetic characters. |
| `1010204` | Invalid Parameter | A parameter contains non-numeric characters. |
| `1010205` | Invalid Parameter | The specified parameter is expected to be in decimal format, but does not appear to be a valid decimal value. |
| `1010206` | Invalid Date | The specified parameter does not seem to be a valid date. |
| `1010207` | Invalid Time | The specified parameter does not seem to be a valid time. |
| `1010208` | Invalid IP Address | The merchant entered an IP address that was in an invalid format. The IP address must be in a format such as 123.456.123.456. |
| `1010209` | Invalid Email Address | The merchant entered an email address that was in an invalid format. |
| `1010301` | Invalid Soft Descriptor | The soft descriptor contains invalid characters. |
| `1010401` | UTF-8 Encoding error | The merchant sent parameters which are not UTF-8 encoded. |

##Error codes relating to the checkout process

| **Code** | **Message** | **Description** | **How to correct this error** |
| --- | --- | --- | --- |
| `1020001` | No route to acquirer | The requested payment product is not configured for your account. | Please contact your account manager to solve this issue. |
| `1020002` | Unsupported ECI | The specified ECI is not supported by the gateway. |   |
| `1020003` | Unsupported Payment Product | The specified payment product is not valid. | Please make sure that you have specified a valid product code. |

| **Code** | **Message** | **Description** | **How to correct this error** |
| --- | --- | --- | --- |
| `3000001` | Unknown Order | The order was not found. |   |
| `3000002` | Unknown Transaction | The transaction was not found. |   |
| `3000003` | Unknown Merchant | The merchant account does not exist. |   |
| `3000101` | Unsupported Operation | The operation is not supported. | Please retry the request with a supported operation. |
| `3000102` | Unknown IP Address | The IP address cannot be detected. The transaction cannot be processed without a valid IP address. |   |
| `3000201` | Suspicion of fraud | The transaction has been rejected by the financial institution due to suspected fraud. |   |
| `3030001` | Fraud Suspicion | The transaction has been rejected by HiPay due to suspected fraud. |   |
| `3040001` | Unknown Token | The specified token was not found in the Secure Vault. |   |
| `3010001` | Unsupported Currency | The currency is not supported. | Please retry the request with a supported currency. |
| `3010002` | Amount Limit Exceeded | The amount exceeds the maximum amount allowed for a single transaction. | Please retry the request with a lower amount. |
| `3010003` | Max Attempts Exceeded | You have exceeded the maximum number of payment attempts for this order. |   |
| `3010004` | Duplicate Order | The order has already been processed. |   |
| `3010005` | Checkout Session Expired | This session has expired. The order is no longer valid. |   |
| `3010006` | Order Completed | The order has already been completed. |   |
| `3010007` | Order Expired | The order has expired. |   |
| `3010008` | Order Voided | The order has been voided. | &nbsp; |

##Error codes relating to maintenance operations

| **Code** | **Message** | **Description** | **How to correct this error** |
| --- | --- | --- | --- |
| `3020001` | Authorization Expired | The authorization has expired. |   |
| `3020002` | Amount Limit Exceeded | The amount specified exceeds the allowable limit. | Please retry the request with a lower amount. |
| `3020101` | Not Enabled | The Capture feature is not enabled for the merchant. | Please contact your account manager to solve this issue. |
| `3020102` | Not Allowed | You cannot capture this type of transaction. |   |
| `3020103` | Not Allowed | You cannot partially capture this type of transaction. |   |
| `3020104` | Permission Denied | You do not have permission to capture this transaction | You are not the owner of this transaction. |
| `3020105` | Currency Mismatch | The currency must be the same for Capture and Authorization. | Make sure that currencies are the same and retry the request. |
| `3020106` | Authorization Completed | The authorization has already been completed. |   |
| `3020107` | No More | The maximum number of allowable captures has been reached.No more capture for the authorization. |   |
| `3020108` | Invalid Amount | The capture amount must be positive. | Please retry the request with a positive amount. |
| `3020109` | Amount Limit Exceeded | The capture amount must be less than or equal to the original transaction amount. | Please retry the request with a lower amount |
| `3020110` | Amount Limit Exceeded | The partial capture amount must be less than or equal to the remaining amount. | Please retry the request with a lower amount. |
| `3020111` | Operation Not Permitted | The transaction is closed. |   |
| `3020112` | Operation Not Permitted | This transaction cannot be processed because it has been denied by the Fraud Protection Service. | You cannot capture a payment after it has been denied by the Fraud Protection Service. |
| `3020201` | Not Enabled | The Refund feature is not enabled for the merchant. | Please contact your account manager to solve this issue. |
| `3020202` | Not Allowed | You cannot refund this type of transaction. |   |
| `3020203` | Not Allowed | You cannot partially refund this type of transaction. |   |
| `3020204` | Permission Denied | You do not have permission to refund this transaction. | You are not the owner of this transaction. |
| `3020205` | Currency Mismatch | The refund must be in the same currency as the original transaction. | Make sure that the currencies are the same and retry the request. |
| `3020206` | Already Refunded | This transaction has already been fully refunded. |   |
| `3020207` | No More | The maximum number of allowable refunds has been reached. No more refund for the transaction. |   |
| `3020208` | Invalid Amount | The refund amount must be positive. | Please retry with a positive amount. |
| `3020209` | Amount Limit Exceeded | The refund amount must be less than or equal to the original transaction amount. | Please retry the request with a lower amount. |
| `3020210` | Amount Limit Exceeded | The partial refund amount must be less than or equal to the remaining amount. | Please retry the request with a lower amount. |
| `3020211` | Operation Not Permitted | The transaction is closed. |   |
| `3020212` | Too Late | You are over the time limit to perform a refund on this transaction. |   |
| `3020301` | Not Enabled | The re-authorization feature is not enabled for the merchant. | Please contact your account manager to solve this issue. |
| `3020302` | Not Allowed | Re-authorization is not allowed for this type of transaction. |   |
| `3020303` | Cannot Reauthorize | You can only re-authorize the original authorization, not a re-authorization. |   |
| `3020304` | Max Limit Exceeded | The maximum number of re-authorizations allowed for the authorization has been reached. |   |
| `3020401` | Not Allowed | You cannot void this type of transaction. |   |
| `3020402` | Cannot Void | You can only void the original authorization, not a re-authorization. |   |
| `3020403` | Authorization Voided | The authorization has already been voided. | &nbsp; |

##Acquirer’s reason codes

| **Code** | **Message** | **Description** |
| --- | --- | --- |
| `4000001` | Declined | The transaction has been declined by the acquirer. |
| `4000002` | Declined | The payment has been refused by the financial institution. |
| `4000003` | Insufficient Funds | The customer&#39;s account does not have sufficient funds. |
| `4000004` | Technical Problem | There was a problem processing this transaction. |
| `4000005` | Communication Failure | This transaction cannot be processed. |
| `4000006` | Acquirer Unavailable | This transaction cannot be processed because the acquirer is temporarily unavailable. |
| `4000007` | Duplicate Transaction | The transaction has already been processed. |
| `4000008` | Payment cancelled by the customer | The transaction has been cancelled by the customer. |
| `4000009` | Invalid transaction | The transaction type is not valid. |
| `4000010` | Please call the acquirer support call number | An issue occurred with the acquirer: please contact your HiPay account manager. |
| `4000011` | Authentication failed. Please retry or cancel. | The authentication requested by the payment method has failed. |
| `4000012` | No UID configured for this operation | The payment method used for this transaction is not supported on current account configuration. |
| `4010101` | Refusal (No Explicit Reason) | The transaction has been declined by the card issuer with no given explanation. |
| `4010102` | Issuer Not Available | The authorization centre of the card issuer is not operational at this time. |
| `4010103` | Insufficient Funds | The cardholder does not have enough funds to make this payment. |
| `4010201` | Transaction Not Permitted | The transaction is not permitted for this type of card. |
| `4010202` | Invalid Card Number | The transaction failed due to an invalid credit card number. |
| `4010203` | Unsupported Card | The type of card is not supported or is unknown. |
| `4010204` | Card Expired | The transaction has been declined because the expiry date on the card used for payment has already passed. |
| `4010205` | Expiry Date Incorrect | The transaction has been declined because the expiry date entered for the card used for payment is incorrect. |
| `4010206` | CVC Required | The transaction cannot be processed because no Card Verification Code was provided. |
| `4010207` | CVC Error | The transaction has been declined because the CVC entered does not match the credit card. |
| `4010301` | AVS Failed | The transaction has been refused because the AVS response returned an &quot;N&quot; value and the merchant account is not able to accept such transactions. |
| `4010302` | Retain Card | The bank put a hold on purchases due to an issue with the cardholder&#39;s account. |
| `4010303` | Lost or Stolen Card | The card has been blocked by the card issuer because the cardholder reported it as being lost or stolen (potential fraud). |
| `4010304` | Restricted Card | The credit card is blacklisted by the card association. |
| `4010305` | Card Limit Exceeded | The transaction would exceed the monthly limit of the card. |
| `4010306` | Card Blacklisted | The card has been rejected by the bank&#39;s fraud system. |
| `4010307` | Unauthorized IP address country | The IP address country used is not authorized. |
| `4010309` | Card not in authorizer&#39;s database | The credit card number is not in an authorized cards database. |
| `4010310` | 3DS required but not used | The transaction requires 3DS authentication but the credit card is not enrolled. |

--------

#Electronic Commerce Indicator

The ECI indicates the security level at which the payment information is processed between the cardholder and the merchant.

The table below lists all the available Electronic Commerce Indicators as they are supposed to be sent along with each authorization request.

| **ECI** | **Name** | **Description** |
| --- | --- | --- |
| `1` | MO/TO (Mail Order/Telephone Order) | The merchant received the customer&#39;s financial details over the phone      or via fax/mail, but does not have the customer&#39;s card at hand. |
| `2` | Recurring MO/TO | The first transaction of the customer was a Mail Order / Telephone Order transaction (financial details were given to the merchant over the phone    or via mail/fax). These details are either stored by the merchant or stored   in our system in order to use an alias to make recurring transactions for   the same customer. |
| `3` | Installment Payment | Partial payment for goods/services that have already been delivered       but that will be paid for in several spread payments. |
| `4` | Manually Keyed               (Card Present) | The customer is physically present in front of the merchant. The merchant has the customer&#39;s card within easy reach. The card details are manually entered; the card is not swiped through a machine. |
| `7` | Secure E-commerce          with SSL/TLS Encryption | The payment transaction was conducted over a secure e-commerce channel (e.g.: TLS). |
| `9` | Recurring E-commerce | The first transaction of the customer was an e-commerce transaction; which means that financial details were entered by the customer              on a secure website (either the merchant&#39;s website or our secure platform). These details are either stored by the merchant or stored in our system     in order to use an alias to make recurring transactions for the same customer. |
| `10` | In-store payment | Payment processed in a physical store, generally from a payment terminal. |
