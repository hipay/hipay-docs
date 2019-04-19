# Custom data

With this extension, you have the possibility of sending additional information with the transaction details that can then be viewed in your HiPay back office. To do so, you can use the filter functionality from WordPress.
Custom data can be received through HiPay notifications and processed later.

For more information, please refer to the documentation on Devdocs: https://developer.wordpress.org/reference/functions/add_filter/.

To add custom data to transactions details, you must create a handler for the filter "_hipay_wc_request_custom_data_" in a custom module or in your theme.

This can be done as follows:

    <?php
    /**
     * HiPay Enterprise extension for WooCommerce
     *
     * 2019 HiPay
     *
     * NOTICE OF LICENSE
     *
     * @author    HiPay <https://support.hipay.com/>
     * @copyright 2019 HiPay
     * @license   https://github.com/hipay/hipay-enterprise-sdk-woocommerce/blob/master/LICENSE.md
     */
    
    /**
     * This is an example of how to add custom data to the transaction gateway.
     * You can add your own function/class, you just have to hook the 'hipay_wc_request_custom_data' filter.
     *
     * If you want to use this example, copy/paste this file in your theme or custom plugin and require it.
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
         * Returns your custom data in a JSON format for gateway transaction requests
         *
         * @param $customData
         * @param $order
         * @param $params
         * @return mixed
         */
        public function getCustomData($customData, $order, $params)
        {
            // This is an example of how to add custom data.
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
