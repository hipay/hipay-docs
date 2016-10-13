# HiPay Marketplace cash-out integration for Mirakl

## Preamble
The **HiPay Marketplace cash-out integration for Mirakl** intends to facilitate cash-out operations between HiPay and the Mirakl marketplace solution. Please note that this software is a turnkey integration of the [HiPay Marketplace cash-out library for Mirakl][repo-lib], which is useless alone and needs to be integrated. In most cases, you won't need to integrate the library yourself. So unless you have specific needs, you're in the right place.

## Objective
This integration is a standalone application software that will, once set up, handle cash-out operations. This is done by automatically retrieving the payment orders from Mirakl and sending them to HiPay for withdrawal both to the merchants' and the marketplace operator's bank accounts. 

[repo-lib]: https://developer.hipay.com/doc/hipay-marketplace-cashout-mirakl-library