# Prerequisites and recommendations

## Objective
The **HiPay Wallet cash-out library for Mirakl** intends to facilitate cash-out operations between HiPay and the Mirakl marketplace solution.

Keep in mind that **unless you have specific needs, do not integrate this library, but install the [standalone turnkey integration][repo-integration] instead**.
At least, check out the HiPay Wallet cash-out integration for Mirakl and make sure it doesn't fit your needs before integrating this library.

## Prerequisites and recommendations

### System
- You must have at least PHP 5.3.9. The library has dependencies that need at least this version of PHP.
- You should have and use Composer. It is the best way to install the library.

### HiPay
- Some operations require HiPay's assistance: please contact HiPay's Business IT Services for technical support on the [HiPay Support Center][hipay-help].
- A HiPay Wallet account for the operator must be created beforehand. An email address that will not be used for another shop on the marketplace is required.
- A good understanding of APIs for cash-out transactions may be useful: please refer to the *HiPay Marketplace - APIs overview* guide.

### Mirakl
- Each shop must have a unique email address, as a HiPay Wallet account is created with and tied to an email address.
- Even though Mirakl does not require filling in the phone number section, it must be completed to create a HiPay Wallet account.
- Even though Mirakl only requires filling in the IBAN and the BIC in the banking information section, all form fields must be completed to provide banking information to HiPay.
- Only alphanumeric characters should be used for filling in the fields, especially for banking information, as HiPay only accepts this category of characters.
- The version of the API should be at least 3.\*.\*.
- Should the API version be higher, as long as the concerned API being called remains backwards compatible, there won't be any problem.
