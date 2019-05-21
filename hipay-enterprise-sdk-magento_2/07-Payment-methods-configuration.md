# Payment methods configuration

In your Magento Admin Portal, select:  
```nohighlight
"Stores" => "Configuration" => "Sales" [Payment Methods]
```

You can then see all the HiPay Enterprise payment methods.

![legend](images/all_methods.png)

Before describing the configuration fields for each payment method, please note the difference between the *HOSTED* and *API* modes.

## Definitions

### HOSTED mode

A *HOSTED* payment method will redirect your customers to a hosted payment page with a payment form or will display it in iFrame (according to the configuration).  
For this type of payment method, PCI compliance is not required.

### API mode

An *API* payment method will embed a payment form directly on your website.  
For this type of payment method, your website needs to be PCI compliant.

### Hosted fields mode

An *Hosted fields* payment method will embed a payment form directly on your website but the form fields are hosted by HiPay.  
For this type of payment method, PCI compliance is not required.

More about [Hosted fields](https://hipay.com/fr/ressources/hosted-fields)

## General configuration

All methods have basic configuration like native Magento 2 payment methods configuration or general HiPay Enterprise configuration.

|Field|Description|
|-----|----|
|Enabled|Enables/Disables payment method|
|Title|As displayed during checkout|
|Payment Action|Authorization or Sale. See more configuration details below.|
|New Order Status|Status to set to the order when the order is created before payment. *Pending* by default.|
|Order status when payment accepted|Status to set to the order when the transaction is successful. *Processing* by default.|
|Order status when payment refused|Status to set to the order when the transaction fails. *On Hold* by default.|
|Order status when payment cancelled|Status to set to the order when the transaction is canceled by the user. *Canceled* by default.|
|HiPay status to validate order|By default, all orders are validated/invoiced upon notification when the *Capture* status  (118) is sent from the HiPay Enterprise platform (~10 min. after capture is requested). You can change this pattern by selecting "Capture Requested". In this case, the order is validated/invoiced directly upon *Capture Requested* (117) status.|
|Cancel pending order|Cancels orders pending because the customer did not validate the payment. For more information, please refer to the [Cron configuration and task information](#cron-configuration-and-task-information) section.|
|Payment products|Allowed payment products. E.g.: Visa, MasterCard, SisalPay...|
|Use 3D Secure|Configures 3-D Secure. See more configuration details below.|
|Rules 3D Secure|Configures custom rules to activate or not 3-D Secure mode.|
|Use Oneclick|Enable/Disables One-click mode. See more configuration details below.|
|Rules Oneclick|Configures rules to activate or not One-click mode.|
|Payment from Applicable Countries|Limit allowed countries.|
|Payment from Specific Countries|Select allowed countries.|
|Minimum Order Total| Minimum order total amount to activate payment method|
|Maximum Order Total| Maximum order total amount to activate payment method|
|Sort Order|Configures order of payment methods, as displayed during checkout.|
|Debug|Enable/Disables debug mode (which logs queries).|
|Environment|Stage or Production. In Stage mode, the test API endpoint and your test credentials are used.|

![legend](images/general_config_1.png)

![legend](images/general_config_2.png)

## HOSTED mode configuration

HOSTED configuration specific fields

|Field|Description|
|-----|----|
|Display card selector|Allows to display card selector on a hosted page.|
|Custom CSS url|Hosted pages can be customized with a CSS URL. Important: HTTPS protocol is required.|
|Template type|Hosted page template|
|Display hosted page in iFrame|By default, a hosted payment method redirects your customers to a hosted payment page, but in iFrame mode, this hosted page is displayed in an iFrame directly on your website.|
|iFrame Width|Width of the iFrame (if iFrame mode is enabled)|
|iFrame Height|Height of the iFrame (if iFrame mode is enabled)|
|iFrame Style|*Style* attribute's value of the iFrame (if iFrame mode is enabled)|
|Wrapper iFrame Style|*Style* attribute's value of the iFrame wrapper (if iFrame mode is enabled)|

![legend](images/hosted_config.png)

## API mode configuration

API configuration specific field

|Field|Description|
|-----|----|
|Display card owner|Displays field form for card owner's full name|

![legend](images/api_config.png)

## Hosted fields mode configuration

Hosted fields configuration specific field

|  Field    |
|----------|
|  color    |
|  fontFamily |
| fontSize | 
| fontWeight |
| placeholder Color|
| caretColor |
| iconColor |

![legend](images/hf_config.png)

Those parameters allows you to override default CSS properties in hosted form fields.

To override the default template, please refer to the magento 2 documentation ([doc.](https://devdocs.magento.com/guides/v2.3/frontend-dev-guide/templates/template-override.html)) and the HiPay SDK JS documentation ([doc.](/doc/hipay-enterprise-sdk-js_3/#hipay-enterprise-javascript-sdk)).

![legend](images/hf_form.png)

# More configuration details

## 3-D Secure activation

You can choose between 5 options:  

1. **Disabled** (to bypass 3-D Secure authentication)
2. **Try to enable for all transactions** 
3. **Try to enable for configured 3ds rules**
4. **Force for configured 3ds rules** 
5. **Force for all transactions** 

![legend](images/3dsecure_options.png)

Rules configuration follows the same process as Magento 2 price rules.

![legend](images/3dsecure_rules.png)

## "Sales" mode (direct capture)

When making a purchase with the "Sales" mode, the capture is automatically requested right after authorization. Please refer to /doc-api/enterprise/gateway/#!/payments/requestNewOrder (Response Content Type – Parameters – operation).

If the payment fails, the customer is redirected to an error page and the status is defined as "_CANCELED_".

If the payment is successful, the customer is redirected to the success page and the status is defined as "_CAPTURE REQUESTED_".

## "Authorization" mode

When making a purchase with the "Authorization" mode, the transaction status will be "_AUTHORIZED_" until you ask for the capture. Please refer to /doc-api/enterprise/gateway/#!/payments/requestNewOrder (Response Content Type – Parameters – operation).

Customers are not charged directly: you have 7 days to "capture" the order and charge the customer. Otherwise, the order is cancelled.

If the authorization fails, the customer is redirected to an error page and the status is defined as "_CANCELED_".

If the authorization is successful, the customer is redirected to the success page and the status is defined as "_AUTHORIZED_".

To capture the transaction, please see [Manual capture and refund](#manual-capture-and-refund).

## One-click (only available for credit card payment methods)

If the One-click option is enabled, your system will create an "alias" for the credit card. Customers will thus be able to use a saved credit card for their second transaction and won't need to fill in all the payment data again.

![legend](images/front_oneclick_selected.png)

![legend](images/front_oneclick_not_selected.png)

Customers can delete them from their account.  

![legend](images/customer_ccards.png)
