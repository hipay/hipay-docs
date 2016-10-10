# HiPay MP alors c'est ok ?

## Preamble
The **HiPay Wallet cash-out library for Mirakl** intends to facilitate cash-out operations between HiPay and the Mirakl marketplace solution. This library needs to be integrated in order to have a comprehensive cash-out workflow. Unless you want to integrate this library yourself, please check out the **[HiPay Wallet cash-out integration for Mirakl][repo-integration]**, which is a software that does integrate this library.

**Unless you have specific needs, do not integrate this library**, but install [the standalone integration][repo-integration] which integrates it.

## Important notice

As stated in the previous section, this library needs to be integrated. In most cases, you won't need to integrate it yourself and will rather install the [turnkey Silex integration][repo-integration]. You may want to integrate this library yourself if you already have a back-end application and want to manage the cash-out workflow in it. 

## Objective
This library is an implementation of both the HiPay and the Mirakl APIs. It allows you to retrieve payment operations from Mirakl, to transfer funds and to perform withdrawals to bank accounts by using the relevant HiPay Wallet API. In other words, this library allows you to perform withdrawals corresponding to Mirakl's payment operations by leveraging the HiPay Wallet payment solution.

## Table of contents
1. [Disclaimer](#disclaimer)
2. [Prerequisites and recommendations](#prerequisites-and-recommendations)
3. [Installation](#installation)
4. [Usage](#general-usage)
5. [Tests](#tests)
6. [Versioning](#versioning)
7. [Appendix](#appendix)
