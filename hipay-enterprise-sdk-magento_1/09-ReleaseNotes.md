# Release notes


## v1.7.0

**Publish date** : 06-12-2017  
[*GitHub Release*](https://github.com/hipay/hipay-fullservice-sdk-magento1/releases/tag/1.7.0)


* New - Supports Oney Facily Pay  
*Oney Facily Pay 3X and 4X with fees or no fees*

* New - Supports AstroPay  
*We added several payment methods for Mexico and Brazil.
The module now supports:*
  * *OXXO*
  * *Aura*
  * *Caixa*
  * *BBVA Bancomer*
  * *Santander cash*
  * *Bradesco*
  * *Boleto*
  * *Itaú*


* New - Mapping your categories with HiPay  
* New - Mapping your shipping methods with HiPay and determine delivery date  
*Two new screens are available for mapping your categories and your shipping method with the HiPay data. These screens are available in the general configuration of the HiPay module.
This mapping is mandatory for Oney Facily Pay.*

##v1.6.2

**Publish date** : 05-05-2017  
[*GitHub Release*](https://github.com/hipay/hipay-fullservice-sdk-magento1/releases/tag/1.6.2)

* Fix - TokenJS domain  
*The URL for HiPay tokenization was per default on production.*

##v1.6.1

**Publish date** : 04-13-2017  
[*GitHub Release*](https://github.com/hipay/hipay-fullservice-sdk-magento1/releases/tag/1.6.1)

Fix - [SECURITY] Checks notification signature if passphrase is not empty  
Fix - X-forward-for without proxy  
Fix - Http code if signature is wrong  

##v1.6.0

**Publish date** : 02-23-2017  
[*GitHub Release*](https://github.com/hipay/hipay-fullservice-sdk-magento1/releases/tag/1.6.0)

- New - Payment MO/TO configuration
- New - Payment MO/TO sends customer the payment page link
- New - New HiPay branding
- New - New order_id nomenclature on split payment
- New - Optimization of split payment profile labels
- New - Add Request sources sent to the API request
- New - Change "HiPay's Cards'" title
- New - Basket configuration
- New - Basket: Send the basket to transaction
- New - Basket: Available for capture and refund
- New - Additional parameters: use order currency for transactions
- New - Payment method: add Klarna integration associated with the basket feature
- New - Change Payment Methods in Store View in addition to the general configuration
- Fix - Add tax rate to split payment
- Fix - Cancel management by the HiPay back office towards Magento
- Fix - Callback 142 Authorization requested
- Fix - Custom_data file in error when not used


