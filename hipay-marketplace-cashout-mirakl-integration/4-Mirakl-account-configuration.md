# Mirakl account configuration

Before starting the installation, please read all instructions and make sure you've gone through the [Prerequisites and recommendations](https://developer.hipay.com/doc/hipay-marketplace-cashout-mirakl-integration/#prerequisites-and-recommendations) section.
 
## Preamble

In order for us to validate the HiPay accounts, the connector needs to upload KYC documents allowing us to identify your merchants.

Thus, you need to configure your Mirakl back office in order to be able to upload these documents for each of your shops. Then, these documents will be retrieved by the HiPay Marketplace cash-out integration for Mirakl and sent to the HiPay platform.

Furthermore, you need to specify which shops should be processed.

## Document configuration

Go to the **Manage document types** section by following these steps.

- Log in to your Mirakl operator account.
- In the **Settings** tab, click on **Stores** (in the "Advanced parameters" column).
- Click on **Documents**.

You need to provide the following list of document types by clicking on *Add a document type*:

| Label | Description | Code | 
|-------|-------|------|
| Bank | Bank account details (“RIB”/IBAN) / Account statement /… (for all professionals) | `ALL_PROOF_OF_BANK_ACCOUNT` |
| Identity card | Copy of the front of a valid identification document of the legal representative (professionals — **only for corporations**) | `LEGAL_IDENTITY_OF_REPRESENTATIVE` | 
| Identity card rear | Copy of the back of a valid identification document of the legal representative (professionals — **only for corporations**) | `LEGAL_IDENTITY_OF_REP_REAR` | 
| Company Registration | Document certifying company registration issued within the last three months (Kbis extract) (professionals — **only for corporations**) | `LEGAL_PROOF_OF_REGISTRATION_NUMBER` | 
| Distribution of power | Signed Articles of Association with the division of powers (professionals — **only for corporations**)| `LEGAL_ARTICLES_DISTR_OF_POWERS` |  
| ID proof | Copy of the front of a valid identification document of the legal representative (professionals — **only for persons**) | `SOLE_MAN_BUS_IDENTITY` |  
| ID proof rear | Copy of the back of a valid identification document of the legal representative (professionals — **only for persons**) | `SOLE_MAN_BUS_IDENTITY_REAR` |  
| Company Registration  | Document certifying registration issued within the last three months (Kbis extract) (professionals — **only for persons**)  | `SOLE_MAN_BUS_PROOF_OF_REG_NUMBER` |  
| Tax status | Document certifying tax status (“auto-entrepreneur” / independent /…) (professionals — **only for persons**) | `SOLE_MAN_BUS_PROOF_OF_TAX_STATUS` |  

You can put whatever you want in the description field. In any case, **the codes must be exactly as displayed in the table**.

Then, you will need to upload the documents on each of the shop pages.

Documents must be image (jpg, jpeg, png...) or PDF files: otherwise, the HiPay Marketplace cash-out integration for Mirakl will not process them.

Please note: when uploading your files (on a shop page), **only upload files corresponding to the type of your shop**. For example, if the shop is managed by a person, you can only upload the required documents flagged as being "only for persons" as well as the bank account details, but not the documents flagged as being "only for corporations".

If your marketplace only has corporations as shops and no shops managed by persons (most probable scenario), **you should not upload the document types flagged as being "only for persons"**.

For more information on the types of KYC documents, please check <a href="https://developer.hipay.com/getting-started/platform-hipay-marketplace/upload_kyc_doc/" target="_blank">HiPay Marketplace - REST API to upload KYC documents</a>.

## Custom fields configuration

Go to the **Manage custom fields** section by following these steps.

- Log in to your Mirakl operator account.
- In the **Settings** tab, click on **Stores** (in the "Advanced parameters" column).
- Click on **Custom Fields**.

You need to provide the following custom field by clicking on *Add a Custom Field*:

| Code | Label | Description | Type | Stores permission | Required | Default Value |
|-------|-------|------|------|------|------|------|
| `hipay-process` | HiPay Process | Shop that will be processed | Yes/No | Read write | true | Yes |

Please note: If the custom field `hipay-process` is not set, no shop will be processed by the HiPay Marketplace cash-out integration for Mirakl.
