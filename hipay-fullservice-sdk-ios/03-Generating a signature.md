# Generating a signature server-side

In order to perform some actions with the HiPay Fullservice SDK for iOS such as execute a payment or get transaction details, you need to generate a *signature* on the server side beforehand.

The *signature* will be used as an additional **parameter** for these `HPFGatewayClient`'s methods: 

- Request new order
- Initialize hosted payment page
- Get transaction(s) details

The signature parameter is necessary for security purposes and must be generated on your server beforehand, for each order to be paid on the HiPay Fullservice SDK for iOS. You can get more details about these methods in the a ["Advanced usage" section](#usage-making-payments-core-wrapper-advanced-integration).

## Signature calculation

The signature is the **SHA-1 hash** of these four parameters concatenation:

- Order ID
- Amount (formatted with two decimal places, example: 72.10)
- Currency
- Secret pass phrase

The signature has to be generated beforehand on the **server side** in order not to show the **secret pass phrase** in client code.  
Which is why the example codes below are executed in PHP/Python and not embedded in the iOS app directly. This code has to be hosted somewhere on your servers.

Below are some example codes for signature generation

#### PHP
```PHP
<?php

$orderId = 'TEST_89897';
$amount = 14.1;
$currency = 'EUR';
$passPhrase = '32JUWB3veDWWmHySNJvtvPyBnqrDFEHbaP3jr';

$signature = sha1($orderId . number_format($amount, 2) . $currency . $passPhrase);

echo $signature;

```

#### Python
```Python
import hashlib

orderId = 'TEST_89897'
amount = 14.1
currency = 'EUR'
passPhrase = '32JUWB3veDWWmHySNJvtvPyBnqrDFEHbaP3jr'

signature = hashlib.sha1(orderId + '{0:.2f}'.format(amount) + currency + passPhrase)

print(signature.hexdigest())
```

The codes above generate a signature which aims to be used by the HiPay Fullservice SDK for iOS.

To get the **secret pass phrase** of your account, go to the "Integration" section of the HiPay Fullservice back office and then go to "Security Settings".

![Secret pass phrase example](images/demo/passphrase.png)

You may choose a secret pass phrase if you don't have one already. This pass phrase is also used to process server-to-server notifications.

## Mobile app implementation

The iOS SDK needs to use the signature in order to perform transactions.  

Below is an iOS code example querying the merchant server (yours) to get the signature and then, initializing the SDK's payment page with the signature.


```objectivec
- (void) requestSignature {

    NSString *orderId = @"TEST_89897";

    /* Assuming the server url takes the orderId as argument
     * and generates the signature after retrieving 
     * the required data in database */

    NSString *dataUrl = [NSString stringWithFormat:@"https://your-server.com/api/signature?orderid=", orderId];
    NSURL *url = [NSURL URLWithString:dataUrl];

    NSURLSessionDataTask *downloadTask = [[NSURLSession sharedSession]
            dataTaskWithURL:url completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {

                dispatch_async(dispatch_get_main_queue(), ^{

                    NSError *jsonError = nil;
                    if (data == nil || error != nil) {
                        // Handle Error and return
                        return;
                    }
                    
                    // assuming the server's response is in JSON format
                    NSDictionary* signatureDictionary = [NSJSONSerialization JSONObjectWithData:data options:0 error:&jsonError];
                    if (jsonError == nil) {

                        NSString *signature = signatureDictionary[@"signature"];
                        NSString *amount = signatureDictionary[@"amount"];
                        NSString *currency = signatureDictionary[@"currency"];

                        /* Once we got the signature, we can now instantiate
                         * and present the payment screen */
                        HPFPaymentPageRequest *paymentPageRequest = [[HPFPaymentPageRequest alloc] init];
                        paymentPageRequest.orderId = orderId;
                        paymentPageRequest.amount = @(amount);
                        paymentPageRequest.currency = currency;

                        HPFPaymentScreenViewController *paymentScreen = [HPFPaymentScreenViewController paymentScreenViewControllerWithRequest:paymentPageRequest signature:signature];
                        paymentScreen.delegate = self;

                        [self presentViewController:paymentScreen animated:YES completion:nil];

                    } else {
                        // handle the error
                    }
                });
            }];

    [downloadTask resume];

}
```


For the sake of this example, we assume that the response is in JSON format.
Once you get the signature, you can create a `HPFPaymentPageRequest` with the necessary parameters and then present the `HPFPaymentScreenViewController` screen. You will get more details about the *payment screen* in the next sections.