# Analytics through custom data

## Objectives

Custom data sent to HiPay can be used for analyses in our Analytics Platform. The tables below describe the fields to set up and the reportings in which custom data may be available.

* Transactional reporting refers to Payment page and Payment performance analyses.
* Marketing reporting refers to acquisition, RFM and churn rate analyses.
* Specific reporting refers to dedicated reportings developed for instance on shipping, usage of discount codes...
* Machine learning refers to our "Fraud Advisor" application. These additional fields sent to our machine learning algorithms strengthen the prediction when analyzing if a transaction is fraudulent or not.

# Custom data fields

* "Label" is the key name sent in the custom data.
* "Other labels accepted" defines additional key names that can be used for a given label.

##  Store and delivery information
| Label |Other labels accepted|Comment| Transactional reporting | Marketing reporting | Specific reporting | Machine learning
| :---- |: ------------------ | :---- | :--------------------:   | :-----------------: | :---------------: | :---------------: |
| delivery_type| Shipping Method, Livraison|  Click and Collect, Home delivery, Pick-up & Go... | X | | X | |
| carrier | shipping|E.g., DHL, Chronopost, La Poste... | X | | X | |
| pick_up_go | commande_point_relais | E.g., Name of Pick-up & Go location | X | | X | |
| pick_up_go_zip_code |None | E.g., 75012 | X | | | X
| shipping_option | envoi | Additional options for delivery | X | | | X
| store | enseigne, CodeMagasinVendeur | E.g., Champs-Élysées store - Paris | X | | X | |
| delivery_cost | None | 4.99 | | | X | | |

##  Discount information
| Label |Other labels accepted|Comment| Transactional reporting | Marketing reporting | Specific reporting | Machine learning
| :---- |: ------------------ | :---- | :--------------------:   | :-----------------: | :---------------: | :---------------: |
| discount_code  | discounts, Promo Code| E.g., Delivery10, New50...  | | X | | | |

## Risk and marketing information
| Label |Other labels accepted|Comment| Transactional reporting | Marketing reporting | Specific reporting | Machine learning
| :---- |: ------------------ | :---- | :--------------------:   | :-----------------: | :---------------: | :---------------: |
| is_first_order  | first_order, first_time_buyer, customer_first_order| Format: 1/0 (1 for Yes, 0 for No)  | | X | | X
| customer_registration_date  | client_date_inscr| Format: YYYYMMDD | | X | | X
| is_suspicious  | suspicious_order, suspicious | Format: 1/0 (1 for Yes, 0 for No)  | | | | X
| has_high_risk_product  | None | Format: 1/0 (1 for Yes, 0 for No)  | | | | X
| has_sponsor  | None | Format: 1/0 (1 for Yes, 0 for No)  | | | | X
| acquisition_period  | campagne | Format: YYYYMM | | | | X
| acquisition_channel  | canal | E.g., Web, Store, Campaign | | | X | |
| affiliate  | None | Name of affiliate | | X | X | |
| customer_id  | numCli  | E.g., 123456789 | | X | X | |
| campaign  | None | E.g., Facebook_Ad654AZ | | | X | |
| campaign_type  | None | E.g., Facebook campaign | | X | X | | |

## Purchase and product information
| Label |Other labels accepted|Comment| Transactional reporting | Marketing reporting | Specific reporting | Machine learning
| :---- |: ------------------ | :---- | :--------------------:   | :-----------------: | :---------------: | :---------------: |
| product_family  | None | E.g., Clothes, T-shirts...  | | | X | |
| product_quantities  | quantity, quantities, product_quantity| Quantity of products in basket/order   | | | X | X
| product_collection  | club | E.g., Fall 2016, Book series | X | | X |  ||

## Marketplace and invoice information
| Label |Other labels accepted|Comment| Transactional reporting | Marketing reporting | Specific reporting | Machine learning
| :---- |: ------------------ | :---- | :--------------------:   | :-----------------: | :---------------: | :---------------: |
| invoice_reminder  | relance | E.g., Yes, 2nd, No... |X | | |  |
| merchant_name  | None | E.g., DOE John (ABC NETWORK S.L.)  | | | X | | |
