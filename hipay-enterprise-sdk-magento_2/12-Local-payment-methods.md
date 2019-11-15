# Local payment methods

Simply add a new local payment method based on the HiPay Enterprise `API` or `HOSTED` mode.  
To do so, please follow the steps below.

## HOSTED MODE

### Class method

You need to create a new PHP class in the `src/Model/Method` directory.  
This class must inherit [CcMethod](src/Model/CcMethod.php) or [HostedMethod](src/Model/HostedMethod.php).
The minimum requirement is to enter a code for this method.

Yet, if you need to customize payment features like 3-D Secure, you need to override the protected variables or public method.
All payment features are initially declared in abstract call [FullserviceMethod](src/Model/FullserviceMethod.php).

Please see the example below with *SisalPay* in `HOSTED` mode:  


```php
// Sisal.php in Model/Method folder.

namespace Hipay\FullserviceMagento\Model\Method;

use HiPay\FullserviceMagento\Model\HostedMethod;

class Sisal extends HostedMethod{
	
	const HIPAY_METHOD_CODE               = 'hipay_sisal';
	
	/**
	 * @var string
	 */
	protected $_code = self::HIPAY_METHOD_CODE;
	
	/**
	 * Payment Method feature
	 *
	 * @var bool
	 */
	protected $_canRefund = false;
	
	/**
	 * Payment Method feature
	 *
	 * @var bool
	 */
	protected $_canRefundInvoicePartial = false;
	
	
	
}
```

### Payment method configuration 

In Magento 2, you can import the content of a configuration file to another file.
To do so, `include` XML tags in the configuration file.  
A list of segmented configuration files is available in the [src/etc/adminhtml/system](src/etc/adminhtml/system) directory.


Then, create your payment method configuration file and save it in `src/etc/adminhtml/system`.  
Don't forget to enter your payment method code in the `id` attribute of `groups` tag and to change the `label` tag.

Example for the SisalPay method with a minimum configuration:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<include xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Config:etc/system_include.xsd">
<group id="hipay_sisal" translate="label" type="text" sortOrder="5" showInDefault="1" showInWebsite="1" showInStore="1">
				<label>HiPay Enterprise SISAL Hosted</label>
				<comment></comment>
				<!-- 
					Includes tag import configuration from another file.
					base_top.xml containing configuration fields will appear at the top
					like enabled, title, order statuses...
				-->
				<include path="HiPay_FullserviceMagento::system/method/base_top.xml"/>
                
                 <!-- custom fields or override of hosted/Cc -->
                 <field id="css_url" translate="label comment" type="text" sortOrder="80" showInDefault="1" showInWebsite="1" showInStore="0">
                    <label>Custom CSS url</label>
                    <comment>Important, the HTTPS protocol is required</comment>
                </field> 
                 <field id="template" translate="label" type="select" sortOrder="90" showInDefault="1" showInWebsite="1" showInStore="0">
                    <label>Template type</label>
                    <source_model>HiPay\FullserviceMagento\Model\System\Config\Source\Templates</source_model>
                </field>
                <field id="iframe_mode" translate="label" type="select" sortOrder="100" showInDefault="1" showInWebsite="1" showInStore="0">
                    <label>Display hosted page in Iframe</label>
                    <source_model>Magento\Config\Model\Config\Source\Yesno</source_model>
                </field>
                <!-- 
					Includes tag import configuration from another file.
					base_bottom.xml containing configuration fields will appear at the bottom
					like test mode, debug, sort order...
				-->
                <include path="HiPay_FullserviceMagento::system/method/base_bottom.xml"/>
                
			</group>
</include>
```
 
Make sure to include this configuration file in the payment section of [system.xml](src/etc/adminhtml/system.xml):

```xml
<section id="payment">
	<include path="HiPay_FullserviceMagento::system/method/hosted.xml"/>
	<include path="HiPay_FullserviceMagento::system/method/cc.xml"/>
	<!-- Sisal configuration file  -->
	<include path="HiPay_FullserviceMagento::system/method/sisal.xml"/>		
</section>
``` 

For a global declaration of your payment method, you should add a node to [payment.xml](src/etc/payment.xml):

```xml
<payment xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Payment:etc/payment.xsd">
    <methods>
        <method name="hipay_hosted">
            <allow_multiple_address>0</allow_multiple_address>
        </method>
        <method name="hipay_cc">
            <allow_multiple_address>0</allow_multiple_address>
        </method>
        <!-- Local method -->
        <method name="hipay_sisal">
            <allow_multiple_address>0</allow_multiple_address>
        </method>
    </methods>
</payment>
```

Finally, enter the payment method default configuration node in [config.xml](src/etc/config.xml):


```xml
<hipay_sisal>
	<model>HiPay\FullserviceMagento\Model\Method\Sisal</model>
	<payment_method>HiPay\FullserviceMagento\Model\Request\PaymentMethod\CardToken</payment_method>
    <active>0</active>
    <title>Sisal</title>
    <payment_action>Sale</payment_action>
    <order_status>pending</order_status>
    <iframe_mode>1</iframe_mode>
    <payment_products>sisal</payment_products> <!-- Enter payment code value, see payment products collection in PHP SDK. -->
    <payment_products_categories>realtime-banking</payment_products_categories>
    <display_selector>0</display_selector>
    <authentication_indicator>0</authentication_indicator> <!-- Enable/Disable 3-D Secure -->
    <template>basic-js</template>
    <css></css>
    <allowspecific>0</allowspecific>
    <max_order_total>1000</max_order_total> <!-- Custom local configuration -->
    <allowed_currencies>EUR</allowed_currencies> <!-- Custom local configuration -->
    <debug>0</debug>
    <env>STAGE</env>
</hipay_sisal>
```

You need to put the method model created previously in `model` tag.  
The payment method class is used to map these specific payment method request attributes.  

The `payment_products` tag is used to display payment methods in the payment form.  
In our *SisalPay* example, we restrict to `sisal` product code.  
`display_selector` is also disabled with a zero value.  

If the local method needs custom configurations, you can put them here.  
For example, SisalPay does not allow a transaction over 1,000 euros.  
Therefore, we have entered custom data in the `max_order_total` and `allowed_currencies` tags.

#### Add a JavaScript client template

Magento 2 provides you with a JavaScript templating engine based on knockout.js.
To display a local payment method for this type of templates, you need to declare your payment method in the rendererList in [hipay-methods.js](src/view/frontend/web/js/view/payment/hipay-methods.js).

```javascript
define(
    [
        'uiComponent',
        'Magento_Checkout/js/model/payment/renderer-list'
    ],
    function (
        Component,
        rendererList
    ) {
        'use strict';
        rendererList.push(
            {
                type: 'hipay_hosted',
                component: 'HiPay_FullserviceMagento/js/view/payment/method-renderer/hipay-hosted'
            },
            {
                type: 'hipay_cc',
                component: 'HiPay_FullserviceMagento/js/view/payment/method-renderer/hipay-cc'
            },
            {
            	// New local method with hosted template
                type: 'hipay_sisal',
                component: 'HiPay_FullserviceMagento/js/view/payment/method-renderer/hipay-sisal'
            }
        );
        /** Add view logic here if needed */
        return Component.extend({});
    }
);
```


In your declaration, you must enter a component name; you can override hipay-hosted like [hipay-sisal.js](src/view/frontend/web/js/view/payment/method-renderer/hipay-sisal.js):

```javascript

define(
    [
        'HiPay_fullserviceMagentp/js/view/payment/method-renderer/hipay-hosted', //@override hipay-hosted
    ],
    function (Component) {
        'use strict';
        return Component.extend({
            defaults: {
                template: 'HiPay_FullserviceMagento/payment/hipay-hosted', //template file
                redirectAfterPlaceOrder: false
            },
	        getCode: function() {
	            return 'hipay_sisal'; /:Declare your payment method code
	        },
            isActive: function() {
                return true;
            }
        });
    }
);
```

If you want to create a custom template, put its name in default value template and create your file in `src/view/frontend/web/template/payment/`.

Finally, enter a node in [checkout_index_index.xml](src/view/frontend/layout/checkout_index_index.xml) to merge your method renders.

```xml
...
<!-- Merge payment method renders here -->
<item name="children" xsi:type="array">
    <item name="hipay-payments" xsi:type="array">
        <item name="component" xsi:type="string">HiPay_FullserviceMagento/js/view/payment/hipay-methods</item>
        <item name="methods" xsi:type="array">
            <item name="hipay_hosted" xsi:type="array">
                <item name="isBillingAddressRequired" xsi:type="boolean">true</item>
            </item>
           <item name="hipay_cc" xsi:type="array">
                <item name="isBillingAddressRequired" xsi:type="boolean">true</item>
            </item>
            <!-- Local payment method -->
            <item name="hipay_sisal" xsi:type="array">
                <item name="isBillingAddressRequired" xsi:type="boolean">true</item>
            </item>
        </item>
    </item>
</item>
...
```


### API MODE

The API MODE is used to **request a new order** to the HiPay Enterprise API.
As with the CcMethod, you can provide form fields on the checkout.

Examples are based on the Qiwi Wallet payment method.

#### Method class

Follow the same steps as for the HOSTED MODE, but directly override [FullserviceMethod.php](src/Model/FullserviceMethod.php).

Please see the example below with *Qiwi Wallet* in `API` mode:  

```php
namespace Hipay\FullserviceMagento\Model\Method;

use HiPay\FullserviceMagento\Model\FullserviceMethod;

class QiwiWallet extends FullserviceMethod{
	
	const HIPAY_METHOD_CODE               = 'hipay_qiwiwallet';
	
	/**
	 * @var string
	 */
	protected $_code = self::HIPAY_METHOD_CODE;
	
	/**
	 * Payment Method feature
	 *
	 * @var bool
	 */
	protected $_canRefund = false;
	
	/**
	 * Payment Method feature
	 *
	 * @var bool
	 */
	protected $_canRefundInvoicePartial = false;
	
	/**
	 * @var string[] keys to import in payment additional information
	 */
	protected $_additionalInformationKeys = ['qiwiuser'];
	
	
	
}
```

The import point here is `$_additionalInformationKeys` property. All contained keys are automatically assigned to payment additional information.  

####  Payment method configuration 

Follow the same steps as for the HOSTED MODE. Just remove unused tags in system.xml (qiwi-wallet.xml here) and config.xml.

Set `model` and `payment_method` values in config.xml.

```xml
<hipay_qiwiwallet>
	<model>HiPay\FullserviceMagento\Model\Method\QiwiWallet</model>
	<payment_method>HiPay\FullserviceMagento\Model\Request\PaymentMethod\QiwiWallet</payment_method>
    <active>0</active>
    <title>Qiwi wallet</title>
    <payment_action>Sale</payment_action>
    <order_status>pending</order_status>
    <payment_products>qiwi-wallet</payment_products> <!-- Enter the payment code value, see payment products collection in PHP SDK -->
    <payment_products_categories>ewallet</payment_products_categories>
    <authentication_indicator>0</authentication_indicator> <!-- Enable/Disable 3-D Secure -->
    <allowspecific>0</allowspecific>
    <allowed_currencies>RUB</allowed_currencies> <!-- Custom local configuration -->
    <debug>0</debug>
    <env>STAGE</env>
</hipay_qiwiwallet>
```

Change `id` in qiwi-wallet.xml. In this case, we do not need custom fields:  

```xml
<?xml version="1.0" encoding="UTF-8"?>
<include xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Config:etc/system_include.xsd">
<group id="hipay_qiwiwallet" translate="label" type="text" sortOrder="6" showInDefault="1" showInWebsite="1" showInStore="1">
				<label>HiPay Enterprise Qiwi Wallet API</label>
				<comment></comment>
				<!-- 
					Includes tag import configuration from another file.
					base_top.xml containing configuration fields will appear at the top
					like enabled, title, order statuses...
				-->
				<include path="HiPay_FullserviceMagento::system/method/base_top.xml"/>
                
                 <!-- no custom fields needed -->
                
                
                <!-- 
					Includes tag import configuration from another file.
					base_bottom.xml containing configuration fields will appear at the bottom
					like test mode, debug, sort order...
				-->
                <include path="HiPay_FullserviceMagento::system/method/base_bottom.xml"/>
                
			</group>
</include>
```

Don't forget to add your payment method in [payment.xml](src/etc/payment.xml) and include [qiwi-wallet.xml](src/etc/adminhtml/system/method/qiwi-wallet.xml) file in [system.xml](src/etc/adminhtml/system.xml).


#### Add JavaScript client template

Overall, it is the same method as for the HOSTED MODE, but you have to implement the validation of the form and data sending in the renderer JavaScript file.  
Then, create your html template to add your form fields.   


[hipay-qiwiwallet.js](src/view/frontend/web/js/view/payment/method-renderer/hipay-qiwiwallet.js)  
```javascript
define(
    [
     	'jquery',
        'Magento_Checkout/js/view/payment/default',
    ],
    function ($, Component) {
        'use strict';
        return Component.extend({
            defaults: {
                template: 'HiPay_FullserviceMagento/payment/hipay-qiwiwallet',
                qiwiUserId: '',
                redirectAfterPlaceOrder: false,
                afterPlaceOrderUrl: window.checkoutConfig.payment.hiPayFullservice.afterPlaceOrderUrl,
                paymentForm: $("co-qiwiwallet-form")
            },
            /**
             * Handler used by transparent
             */
            placeOrderHandler: null,
            validateHandler: null,
            
            /**
             * @param {Function} handler
             */
            setPlaceOrderHandler: function (handler) {
                this.placeOrderHandler = handler;
            },

            /**
             * @param {Function} handler
             */
            setValidateHandler: function (handler) {
                this.validateHandler = handler;
            },
            initialize: function() {
                var self = this;
                this._super();

            },
            initObservable: function () {
                this._super()
                    .observe([
                        'qiwiUserId',
                    ]);
                
                this.paymentForm.validation();
                return this;
            },
            getData: function() {
                return {
                    'method': this.item.method,
                    'additional_data': {
                        'qiwiuser': this.qiwiUserId(),
                    }
                };
            },
	        getCode: function() {
	            return 'hipay_qiwiwallet';
	        },
	        /**
	         * Needed by transparent.js
	         */
	        context: function() {
                return this;
            },
            isActive: function() {
                return true;
            },
            /**
             * @returns {Boolean}
             */
            isShowLegend: function () {
                return true;
            },
            validate: function(){
            	return (this.paymentForm.validation && this.paymentForm.validation('isValid'));

            },
            /**
             * After place order callback
             */
	        afterPlaceOrder: function () {
	        	 $.mage.redirect(this.afterPlaceOrderUrl);
	        },
        });
    }
);
```

Part of [hipay-qiwiwallet.html](src/view/frontend/web/template/payment/hipay-qiwiwallet.html)  
```html
...
 <form class="form" id="co-qiwiwallet-form" action="#" method="post" data-bind="mageInit: {'validation':[]}">
		
			<fieldset data-bind="attr: {class: 'fieldset payment items ' + getCode(), id: 'payment_form_' + getCode()}">
			    <!-- ko if: (isShowLegend())-->
			    <legend class="legend">
			        <span><!-- ko i18n: 'Qiwi Wallet Information'--><!-- /ko --></span>
			    </legend><br />
			    <!-- /ko -->
			    <div class="field number required">
			        <label data-bind="attr: {for: getCode() + '_qiwiuser'}" class="label">
			            <span><!-- ko i18n: 'Qiwi User Phone Number'--><!-- /ko --></span>
			        </label>
			        <div class="control">
			            <input type="text" name="payment[qiwiuser]" class="input-text" value=""
			                   data-bind="attr: {
			                                    autocomplete: off,
			                                    id: getCode() + '_qiwiuser',
			                                    title: $t('Qiwi User Phone Number'),
			                                    'data-container': getCode() + '-qiwiuser',
			                                    'data-validate': JSON.stringify({'required-text':true, 'no-whitespace':'#' + getCode() + '_qiwiuser'})},
			                              enable: isActive($parents),
			                              value: qiwiUserId,
			                              valueUpdate: 'keyup' "/>
			        </div>
			    </div>
			</fieldset>
	</form>
...
```

Finally, enter a node in [checkout_index_index.xml](src/view/frontend/layout/checkout_index_index.xml) to merge your method renders.
