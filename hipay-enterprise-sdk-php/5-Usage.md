# How to use

## Autoloading

For libraries that specify autoload information, Composer generates a vendor/autoload.php file. You can simply include this file to get autoloading for free.  
`require __DIR__ . '/vendor/autoload.php';`

For more information: https://getcomposer.org/doc/01-basic-usage.md#autoloading


## How to make a request

You need to instantiate 4 objects to make a request:  

- A configuration object which implements [ConfigurationInterface](https://github.com/hipay/hipay-fullservice-sdk-php/blob/master/lib/HiPay/Fullservice/HTTP/Configuration/ConfigurationInterface.php)
- A client provider inherited from abstract class [\HiPay\Fullservice\HTTP\ClientProvider](https://github.com/hipay/hipay-fullservice-sdk-php/blob/master/lib/HiPay/Fullservice/HTTP/ClientProvider.php)
- A [Gateway](https://github.com/hipay/hipay-fullservice-sdk-php/blob/master/lib/HiPay/Fullservice/Gateway/Client/GatewayClient.php) client
- An [order request](https://github.com/hipay/hipay-fullservice-sdk-php/blob/master/lib/HiPay/Fullservice/Gateway/Request/Order/OrderRequest.php)

#### Gateway example:

```php
//Create a configuration object
// By default Configuration object is configured in Stage mode (Configuration::API_ENV_STAGE)
// To use Production mode, the third argument of the constructor must be Configuration::API_ENV_PRODUCTION
// Ex : $config = new \HiPay\Fullservice\HTTP\Configuration\Configuration("username","password", Configuration::API_ENV_PRODUCTION);
$config = new \HiPay\Fullservice\HTTP\Configuration\Configuration("username","password");

//Instantiate client provider with configuration object
$clientProvider = new \HiPay\Fullservice\HTTP\SimpleHTTPClient($config);

//Create your gateway client
$gatewayClient = new \HiPay\Fullservice\Gateway\Client\GatewayClient($clientProvider);

//Instantiate order request
$orderRequest = new \HiPay\Fullservice\Gateway\Request\Order\OrderRequest();
$orderRequest->orderid = "123456";
$orderRequest->operation = "Sale";
//etc.

//Make a request and return \HiPay\Fullservice\Gateway\Model\Transaction.php object
$transaction = $gatewayClient->requestNewOrder($orderRequest);

```

<div class="alert alert-danger">
<i class="fa fa-exclamation-triangle"></i>
Server side tokenization is deprecated. Please refer to our <a href="https://developer.hipay.com/doc/hipay-enterprise-sdk-js/">SDK JS</a>, <a href="https://developer.hipay.com/doc/hipay-enterprise-sdk-ios">SDK IOS</a> or <a href="https://developer.hipay.com/doc/hipay-enterprise-sdk-android">SDK Android</a> documentations to use client side tokenization (Direct Post).
</div>
