# SDK installation

Before starting the installation, please read all the instructions and make sure you've gone through the Prerequisites and recommendations section.

It is recommended to use Composer to install the HiPay Enterprise SDK for PHP.

## How to use Composer

If your project is not based on Composer, you must install it and create a composer.json file in your root project directory.

+ [Composer installation](https://getcomposer.org/doc/00-intro.md)
+ [Composer project setup](https://getcomposer.org/doc/01-basic-usage.md#composer-json-project-setup)

For the next steps, you need a command line interface; all commands are executed in your root project directory.

### SDK installation

You can now install the SDK with the following command line:

```
$ composer require hipay/hipay-fullservice-sdk-php
```

If no errors are displayed, the module is installed.

## Autoloading

For libraries that specify autoload information, Composer generates a vendor/autoload.php file. You can simply include this file to get autoloading for free.  
`require __DIR__ . '/vendor/autoload.php';`

For more information: https://getcomposer.org/doc/01-basic-usage.md#autoloading
