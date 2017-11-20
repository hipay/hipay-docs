# Appendix

Please find below the description of each operation status listed in the `operations` table.

| Name               | Value | Description                                    |
|--------------------|-------|------------------------------------------------|
| `CREATED`            | 1     | The operation has been created.               |
| `ADJUSTED_OPERATIONS` | 2    | The operation has been adjusted (concerns past negative operations).  |
| `TRANSFER_SUCCESS`   | 3     | The transfer has been executed with success.  |
| `TRANSFER_FAILED`    | -9    | The transfer has been executed and failed.    |
| `TRANSFER_NEGATIVE`  | -10   | The transfer has not been executed because there are not enough funds in the HiPay technical account.  |
| `WITHDRAW_REQUESTED` | 5     | The withdrawal has been requested with success. |
| `WITHDRAW_SUCCESS`   | 6     | The withdrawal has been executed with success.  |
| `WITHDRAW_FAILED`    | -7    | The withdrawal has been executed and failed.   |
| `WITHDRAW_CANCELED`  | -8    | The withdrawal has been canceled.            |
| `WITHDRAW_NEGATIVE`  | -11   | The withdrawal has not been executed because there are not enough funds in the wallet account. |
