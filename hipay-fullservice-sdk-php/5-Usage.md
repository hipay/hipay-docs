# How to use

## Autoloading

For libraries that specify autoload information, Composer generates a vendor/autoload.php file. You can simply include this file to get autoloading for free.  
`require __DIR__ . '/vendor/autoload.php';`

For more information: https://getcomposer.org/doc/01-basic-usage.md#autoloading


## How to make a request

You need to instantiate 4 objects to make a request:  

- A configuration object which implements [ConfigurationInterface](../blob/master/lib/HiPay/Fullservice/HTTP/Configuration/ConfigurationInterface.php)
- A client provider inherited from abstract class [\HiPay\Fullservice\HTTP\ClientProvider](../blob/master/lib/HiPay/Fullservice/HTTP/ClientProvider.php)
- A [Secure Vault](../blob/master/lib/HiPay/Fullservice/SecureVault/Client/SecureVaultClient.php) or [Gateway](../blob/master/lib/HiPay/Fullservice/Gateway/Client/GatewayClient.php) client
- An [order request](../blob/master/lib/HiPay/Fullservice/Gateway/Request/Order/OrderRequest.php)

#### Gateway example:

```php
//Create a configuration object
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

#### Secure Vault example:

```php
//Create a configuration object
$config = new \HiPay\Fullservice\HTTP\Configuration\Configuration("username","password");

//Instantiate client provider with configuration object
$clientProvider = new \HiPay\Fullservice\HTTP\SimpleHTTPClient($config);

//Create your gateway client
$vaultClient = new \HiPay\Fullservice\SecureVault\Client\SecureVaultClient($clientProvider);

//Instantiate Secure Vault request
$generateTokenRequest = new \HiPay\Fullservice\SecureVault\Request\GenerateTokenRequest();
$generateTokenRequest->card_number = "4111111111111111";
$generateTokenRequest->card_expiry_month = "02";
$generateTokenRequest->card_expiry_year = "2017";
//etc.

//Make a request and return HiPay\Fullservice\SecureVault\Model\PaymentCardToken.php object
$paymentCardToken=  $vaultClient->requestGenerateToken($generateTokenRequest);

```
