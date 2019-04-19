# Category and carrier mapping

The customer's cart information can be sent with the transaction if you enabled the option in the global settings.
This mapping is necessary to establish a correlation between your data and HiPay's.
It is mandatory to do all the mappings to use the **Oney Facily Pay** payment method.

<div class="alert alert-warning">
	<i class="fa fa-warning"></i>
	If you add new categories or delivery methods, make sure to match the new corresponding data.
</div>


## Category mapping

In your WordPress dashboard, go to **"_HiPay Enterprise -> Category mapping_"**.

WooCommerce product categories are displayed: you must match each of them with the corresponding HiPay category.

![legend](images/category-mapping.png)

## Carrier mapping

In your WordPress dashboard, go to "_HiPay Enterprise -> Delivery method mapping_".

All the delivery methods available on your store are listed. For each of them, you must fill in the necessary corresponding information.

This information is used to calculate an approximate delivery date.

| Name               | Value |
|:------------|:------------|:-----|
| Order preparation estimated time     |Expressed in working days|
| Delivery estimated time              |Expressed in working days|
| HiPay delivery mode              |- **store**: At the store <br /> - **carrier**: Delivery by carrier <br /> - **relaypoint**: Delivery in a pick-up location <br /> - **electronic** <br /> - **travel**
| HiPay delivery method              |- **standard** <br /> - **express** <br /> - **Priority 24H** <br /> - **Priority 2H** <br /> - **Priority 1H** <br /> - **Instant**

![legend](images/carrier-mapping.png)
