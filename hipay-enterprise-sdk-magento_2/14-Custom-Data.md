# Custom data

It is possible to send additional information in the transaction that can then be viewed in the HiPay back office.
To do so, you can use the functionality of Magento 2 which relies on the notion of plugin.

Please refer to the [Magento PHP Developer Guide](http://devdocs.magento.com/guides/v2.0/extension-dev-guide/plugins.html).

To add custom information to transactions, you need to create a plugin in your custom module.
This plugin must observe _HiPay\FullserviceMagento\Model\Request\Order_
and extend the method _getCustomData_ with afterGetCustomData.

In your di.xml, define the plugin with:

    <type name="HiPay\FullserviceMagento\Model\Request\Order">
        <plugin name="hipay_custom_data" type="\{YOUR_COMPANY}\{MODULE}\Plugin\CustomDataPlugin" sortOrder="01" disabled="false"/>
    </type>

Create a class CustomDataPlugin and implement a method:

     /**
       *  Complete general getCustomData with HiPay’s data
       *
       * @see \HiPay\FullserviceMagento\Helper\Data
       * @param \HiPay\FullserviceMagento\Model\Request\Order $subject
       * @param $result
       * @return array
       */
      public function afterGetCustomData(Order $subject, $result)
      {
      }

You can put what you want in the $result variable.
You can get inspiration from our CustomDataPlugin plugin located in our module:

https://github.com/hipay/hipay-fullservice-sdk-magento2/blob/master/Plugin/CustomDataPlugin.php


