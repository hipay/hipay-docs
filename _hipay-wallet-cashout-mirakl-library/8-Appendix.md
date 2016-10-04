# Appendix

All operation statuses are constants from the class Cashout/Model/Operation/Status.

| Name               | Value | Description                                    |
|--------------------|-------|------------------------------------------------|
| `CREATED`            | 1     | The operation has been created                      |
| `TRANSFER_SUCCESS`   | 3     | The transfer has been executed with success    |
| `TRANSFER_FAILED`    | -9    | The transfer has been executed and failed      |
| `WITHDRAW_REQUESTED` | 5     | The withdrawal has been requested with success |
| `WITHDRAW_SUCCESS`   | 6     | The withdrawal has been executed with success  |
| `WITHDRAW_FAILED`    | -7    | The withdrawal has been executed and failed    |
| `WITHDRAW_CANCELED`  | -8    | The withdrawal has been canceled              |

Comprehensive list of the interfaces to implement to use the library:

| Classpath                                  | Needed for                           | Description                                                                                  |
|--------------------------------------------|--------------------------------------|----------------------------------------------------------------------------------------------|
| `Api\HiPay\ConfigurationInterface`           | All                                  | Represents the data for a HiPay connection                                                   |
| `Api\Mirakl\ConfigurationInterface`          | All                                  | Represents the data for a Mirakl connection                                                  |
| `Vendor\Model\VendorInterface`               | All                                  | Represents an entity to send or receive money                                                |
| `Vendor\Model\ManagerInterface`              | All                                  | Manages vendor entities                                                                      |
| `Cashout\Model\Operation\OperationInterface` | `CashoutInitializer`, `CashoutProcessor` | Represents a transfer from the technical account and a withdrawal to the vendor bank account |
| `Cashout\Model\Operation\ManagerInterface`   | `CashoutInitializer`, `CashoutProcessor` | Manages the operation entities                                                               |