# Custom data

With this module you have the possibility of sending additional informations in the transaction that can then be viewed on HiPay's back office. In order to do this, you can use the the filter functionality from Wordpress.
Custom data can be received and processed later in HiPay notifications.

Please see the documentation on Devdocs: https://developer.wordpress.org/reference/functions/add_filter/

To add custom information with transactions, you need to create an handler for the filter "_hipay_wc_request_custom_data_" in a custom module or in your theme.

This can be done as follows:

    <?php
    /**
     * HiPay Enterprise SDK WooCommerce
     *
     * 2018 HiPay
     *
     * NOTICE OF LICENSE
     *
     * @author    HiPay <support.tpp@hipay.com>
     * @copyright 2018 HiPay
     * @license   https://github.com/hipay/hipay-enterprise-sdk-woocommerce/blob/master/LICENSE.md
     */
    
    /**
     * This is an example of how to add custom data to the gateway transaction
     * You can add your own function/class, you just have to hook the 'hipay_wc_request_custom_data' filter
     *
     * If you want to use this example, copy/paste this file in your theme or custom plugin and requires it.
     *
     */
    class Hipay_Custom_Data
    {
    
        private static $instance;
    
        public function __construct()
        {
            add_filter('hipay_wc_request_custom_data', array($this, 'getCustomData'), 10, 3);
        }
    
        /**
         * Return yours customs data in a json for gateway transaction request
         *
         * @param $customData
         * @param $order
         * @param $params
         * @return mixed
         */
        public function getCustomData($customData, $order, $params)
        {
            // An example of adding custom data
            if ($order) {
                $customData['my_field_custom_1'] = $order->id;
            }
    
            return $customData;
        }
    
        public static function get_instance()
        {
            if (null === self::$instance) {
                self::$instance = new self();
            }
            return self::$instance;
        }
    }
    
    Hipay_Custom_Data::get_instance();
