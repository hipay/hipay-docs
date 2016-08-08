![](images/media/image1.jpg){width="8.286897419072616in"
height="4.4375in"}![](images/media/image2.png){width="3.5388593613298336in"
height="0.4847222222222222in"}![](images/media/image3.png){width="8.334722222222222in"
height="1.9951388888888888in"}![](images/media/image2.png){width="3.5388593613298336in"
height="0.4847222222222222in"}

Settlement file fields 3

Operation types 4

<span id="_Toc453598093" class="anchor"></span>

<span id="_Toc453590076" class="anchor"><span id="_Toc453693193"
class="anchor"></span></span>Settlement file fields

The following table lists and describes all the fields included in each
settlement file.

  **Field name**           **Description**
  ------------------------ ---------------------------------------------------------------
  Account                  HiPay’s account number
  Account name             HiPay’s account name
  Date                     Date of collect
  Date value               Transaction date value
  Invoice reference        HiPay’s invoice reference
  Settlement ID            Settlement ID
  Transaction ID           HiPay’s transaction ID
  Order                    Unique identifier of the order as provided by the merchant
  Product name             Payment method
  Invoiced                 Invoiced transaction (0/1)
  Settled                  Settled transaction (0/1)
  Original amount          Original amount of the operation
  Original currency        Original currency of the operation
  Amount (Excl. Tax)       Amount of the operation excluding tax
  Tax amount               Tax amount of the operation
  Amount (Incl. Tax)       Amount of the operation including tax
  Currency                 Invoice currency
  Operation                Operation type. Please refer to “*Operation types*” below.
  Settled amount           Settled amount
  Settlement currency      Settlement currency
  Original settlement ID   Original settlement ID (if applicable)
  Customer ID              Unique identifier of the customer as provided by the merchant
  Merchant operation ID    Operation ID sent in maintenance operation

### 

<span id="_Toc453590077" class="anchor"><span id="_Toc453693194"
class="anchor"></span></span>Operation types

Here are the different operation types that can appear on a list of
operations.

  **Operation type**                    **Description**
  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------
  **Adjustment in favor of merchant**   Amount credited on the merchant’s balance (E.g.: adjustment of commission billing errors, commercial incentives)
  **Adjustment in favor of TPP**        Amount deducted from the merchant’s balance (E.g.: adjustment for fees not charged)
  **Already settled**                   Statement of an amount already paid by HiPay to the merchant
  **Asset settlement**                  Operation following a negative balance, indicating that it has been paid off on the merchant’s account
  **Chargeback**                        Credit/debit card charge paid by the merchant to a customer after he/she successfully disputed an item on his/her bank statement

  --------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  **Chargeback refund**                      Amount of a chargeback credited back to the merchant after giving evidence that the product or service was duly delivered
  ------------------------------------------ -------------------------------------------------------------------------------------------------------------------------------
  **Deferred settlement**                    Settlement file issued in case the merchant’s HiPay Fullservice account shows a negative balance

  **Fixed fee**                              Commission charged on transactions when collecting amounts

  **Fixed reserve capture**                  Amount transferred by the merchant or debited from the merchant’s balance and withheld for protection against potential risks

  **Fixed reserve release**                  Fixed reserve amount released back to the merchant 180 days after its capture

  **IFF (Fraud Financial Impact)**           Fraudulent blocked transactions fee on the Carte Bancaire’s network paid by the merchant

  **Invoice carry-over **                    Operation created to transfer the merchant’s remaining invoice amount to the next billing cycle
                                             
  **(formerly Invoice asset) **              

  **Invoice to be paid by TPP**              Not applicable
                                             
                                             (old status)

  **Invoice payment**                        Not applicable (old status)

  **Deferred invoice **                      Invoice deferred, in whole or in part, to the next billing cycle
                                             
  **(formerly Invoice report)**              

  **Offsetting entry **                      Entry on a balance sheet that sets another entry to zero. Does not impact billing.
                                             
  **(formerly Merchant’s balance credit)**   

  **Monthly fee**                            Fee charged every month

  **Refund**                                 Reimbursement of a transaction

  **Rolling reserve capture**                Amount withdrawn from every payout and withheld for protection against potential risks

  **Rolling reserve release**                Amount released back to the merchant after being held for 180 days

  **Sale**                                   Sale or transaction amount

  **Settlement**                             Payout amount on D-day

  **Setup fee**                              Fee for the implementation of the project and account. It is billed on the first HiPay Fullservice invoice.

  **Split settlement**                       Amount to be paid out on a specific bank account when allocating payouts among several bank accounts

  **Splitting settlement**                   Total amount to be paid out when allocating payouts among several bank accounts

  **Transferred from merchant**              Transfer sent from a HiPay Fullservice account to another HiPay Fullservice account
                                             
                                             (between accounts of a same merchant).
                                             
                                             This operation is displayed on the sending account’s operations.

  **Transferred to merchant**                Transfer sent from a HiPay Fullservice account to another HiPay Fullservice account
                                             
                                             (between accounts of a same merchant).
                                             
                                             This operation is displayed on the receiving account’s operations.

  **Variable**                               Commission based on a percentage and calculated on amounts collected on the HiPay Fullservice account.
  --------------------------------------------------------------------------------------------------------------------------------------------------------------------------


