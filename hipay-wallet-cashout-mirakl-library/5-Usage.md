# Processor
The main functionalities of the library are contained in the **process** method for the classes extending the `Common/AbstractProcessor` class. Although the abstract class does not require the implementation of the process method, its child classes all have one. This is the method you should call in your specific application to process data.

## Interfaces implementation

Therefore, most of the work of the integrator will be to create concrete implementation of the interfaces present in the library, then to construct the processors thanks to the previous objects, and finally to call the **process** method.

### Entity interfaces
These interfaces represent entities used in the code.

#### VendorInterface
The `VendorInterface` represents a vendor or an operator. More generally speaking,                  the `VendorInterface` represents an entity which will either send or receive money through HiPay. The six methods it imposes for implementation, paired with two getters and setters, indicate the three properties the concrete class should have:

|Property Name|Description|
|-------------|-----------|
|`miraklId`|The shop identifier present in the view of the Mirakl shop.|
|`hipayId`|The id of the HiPay wallet account.|
|`email`|E-mail used to create the HiPay Wallet account. The email used is from the Contact information section of the shop parameters as it is the only one available.|

All of these should be unique within the storage of your choice.

#### OperationInterface
The `OperationInterface` represents an operation to be executed (transfer and withdrawal) and its current state. Likewise, the methods indicate the properties the concrete class should have: 
|Property name|Description
|-------------|-----------|
|`miraklId`|The shop identifier present in the view of the Mirakl shop.
|`hipayId`|The ID of the HiPay Wallet account.
|`withdrawId`|Withdrawal ID that will be completed after the withdrawal step of the operation has been successful. Should be unique.
|`withdrawnAmount`|The actual withdrawn amount. Will be completed after the Withdrawal has been successful
|`transferId`|Transfer ID that will be completed after the transfer step of the operation has been successful. Should be unique.
|`status`|Current status of the operation.
|`amount`|Amount transferred from and withdrawn of the technical account.
|`updatedAt`|Time of the last update

After creation, properties should not change (except for Status and UpdatedAt). Therefore, even though the setters must be implemented, you should not use these in your code. As they are interfaces, do not hesitate to implement them into existing entity classes.

### Entity manager interfaces
The concrete implementation of these interfaces will be used to create, find (read) and update the entities they are related to. There are one for each of the entities.

#### Vendor/ManagerInterface
The ManagerInterface under the vendor namespace is related to the vendor entity. Below some explanation for the methods implementation :

| Method name | Implementation advice |
| --- | --- |
| `create` | This method should simply create the VendorObject. It must not save the vendor in the storage. It may be empty (i.e. properties have not been set) as properties are set afterwards anyway. |
| `update` | This method should update the vendor for another payment cycle than its creation one.  It must not save the vendor in the storage. |
| `save` | This method should be used to save one vendor in the storage. |
| `saveAll` | This method should be used to save an array of vendors. When receiving a batch of vendors, it may include new vendors and already saved ones. |
| `findBy*` | This method should be used to find a vendor (and only one, as all mandatory vendor properties are unique) by the property provided in the method name. Should return null if nothing is found. |
| `isValid` | This method should be used to implement custom validation before save. |

#### Operation/ManagerInterface
Likewise, the `ManagerInterface` below the Cashout/Operation will handle operations. The `create`, `save`, `saveAll` and `isValid` methods are the same as previously, applied to operations.

|Method name|Implementation advice|
|-----------|---------------------|
| `generate*label` | This method should return a string which will be used as a label for the HiPay transfer and withdrawal API calls. Avoid using non-alphanumeric characters. |
| `findByWithdrawalId` | This method should return one and only one operation. Returns null if nothing is found. |
| `findByMiraklIdAndPaymentVoucher` | This method should return at most one operation. Returns null if nothing is found. This method is used to check if an operation already existed for the given miraklId and paymentVoucher. It follows the mandatory unicity of the couple miraklId and paymentVoucher. |
| `findByStatus` | This method should return all the operations in the given status. |
| `findByStatusAndBeforeUpdatedAt` | This method should return all the operations in the given status with the updatedAt property on or before the given date. |

### Configuration Interfaces

These interfaces must be implemented to create the instance of the classes which communicate with an external system (HiPay and Mirakl). There are only getters. However, all must be implemented. They correspond to the parameters needed to instantiate these classes.

### Model interfaces

There is only one interface in this category. The `TransactionValidator` is used to validate the transaction before computing it in an operation to be recorded. The sole method to implement, `isValid`, should do the validation (returning true if it passes).

## Event

You can easily interact with the processor execution flow thanks to the event system used throughout the process method. Based on the Symfony component Event Dispatcher, you can customize the behavior of the program when an event is dispatched.  The `AbstractProcessor` class expose two method `addListener` and `addSubscriber`. These two methods reflect the ones from the `EventDispatcherInterface`. This system avoid to have to create a new class inheriting the previous one.

For more information, please refer to [this component documentation](http://symfony.com/doc/current/components/event_dispatcher/).

### VendorProcessor

| Name | Event object class | Description |
| --- | --- | --- |
| `before.vendor.get` | N/A | Before the call to Mirakl to fetch the vendors |
| `after.vendor.get` | N/A | After the successful call to Mirakl to fetch the vendors |
| `before.availability.check` | `CheckAvailability` | Before the call to HiPay to check the availability of the email address |
| `after.availability.check` | `CheckAvailability` | After the successful call to HiPay to check the availability of the email address |
| `before.wallet.create` | `CreateWallet` | Before the call to HiPay to create the HiPay Wallet account |
| `after.wallet.create` | `CreateWallet` | After the successful call to HiPay to create the HiPay Wallet account |
| `before.bankAccount.add` | `AddBankAccount` | Before the call to HiPay to send the bank account information |
| `after.bankAccount.add` | `AddBankAccount` | After the successful call to HiPay to send the bank account information |
| `check.bankInfos.synchronicity` | `CheckBankInfos` | Used to check custom bank information |

### CashoutProcessor

| Name | Event object class | Description |
| --- | --- | --- |
| `before.transfer` | `OperationEvent` | Before the call to HiPay to transfer the amount from the technical account to the vendor/operator account |
| `after.transfer` | `OperationEvent` | After the successful call to HiPay to transfer the amount from the technical account to the vendor/operator account. The transferId will be set on the event object. |
| `before.withdraw` | `OperationEvent` | Before the call to HiPay to withdraw the amount from the HiPay Wallet account to the actual bank account |
| `after.withdraw` | `OperationEvent` | After the call to HiPay to withdraw the amount from the HiPay Wallet account to the actual bank account.The withdrawId will be set on the event object. |                                                                                                                

### Exception

Most of the custom exceptions dispatch an event with a specific name.

Remember: you do not need to log in as there is always a call to the logger before dispatching the event. The event object class will always be the same: `ThrowException`.

It is nothing but a wrapper object to contain the exception.

|  Event name                       |Exception class                      |
|-----------------------------------|-------------------------------------|
|  `operation.already.created`      |`AlreadyCreatedOperationException`   |
|  `bankAccount.creation.failed`    |`BankAccountCreationFailedException` |
|  `invalid.bankInfo`               |`InvalidBankInfoException`           |
|  `invalid.operation`              |`InvalidOperationException`          |
|  `invalid.vendor`                 |`InvalidVendorException`             |
|  `not.enough.funds`               |`NotEnoughFunds`                     |
|  `no.wallet.found`                |`NoWalletFoundException`             |
|  `transaction.exception`          |`TransactionException`               |
|  `unauthorized.property.modified` |`UnauthorizedModificationException`  |
|  `wallet.unidentified`            |`UnidentifiedWalletException`        |
|  `bank.account.unconfirmed`       |`UnconfirmedBankAccountException`    |
|  `validation.failed`              |`ValidationFailedException`          |
|  `wrong.wallet.balance`           |`WrongWalletBalance`                 |

# Notification handler

This class intends to handle server-to-server notifications sent by HiPay by `POST`.

There is one main function: `handleHiPayNotification`.

This is the method to call in the web entry point. The entry point must be made accessible to HiPay and its address must be provided to HiPay. Mainly, it gets a string of valid XML, parses it, validates the MD5 and invokes a function according to the operation field of the notification.

This method also uses the event system to allow easy modification of the behavior for the action to take according to the notification sent.

However, there is a default behavior for the withdrawal notification. It sets the status of the operation with the sent `withdrawID` to success or failure according to the content of the request.


| Name | Event object class | Description |
| --- | --- | --- |
| `bankInfos.validation.notification.success` | `BankInfoNotification` | Sent when operation is `bank_info_validation` and its status is `OK` |
| `bankInfos.validation.notification.failure` | `BankInfoNotification` | Sent when operation is `bank_info_validation` and its status is `NOK` |
| `identification.notification.success` | `IdentificationNotification` | Sent when operation is identification and its status is `OK` |
| `identification.notification.failure` | `IdentificationNotification` | Sent when operation is identification and its status is `NOK` |
| `other.notification.success` | `OtherNotification` | Sent when operation is `other_transaction` and its status is `OK` |
| `other.notification.failure` | `OtherNotification` | Sent when operation is `other_transaction` and its status is `NOK` |
| `withdraw.notification.success` | `WithdrawNotification` | Sent when operation is `withdraw_validation` and its status is `OK` |
| `withdraw.notification.failure` | `WithdrawNotification` | Sent when operation is `withdraw_validation` and its status is `NOK` |

You must handle the fetching of the XML from the request and the different exceptions that may arise.
