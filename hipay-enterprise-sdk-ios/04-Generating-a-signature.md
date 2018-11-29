# Signature

In order to perform some actions with the HiPay Enterprise SDK for iOS, such as making a payment or getting transaction details, you need to generate a *signature* on the server side beforehand.

This *signature* will be used as an additional **parameter** for these `HPFGatewayClient` methods:

- Request a new order
- Initialize a hosted payment page
- Get transaction details

The signature parameter is necessary for security purposes and must be generated on your server beforehand for each order to be paid on the HiPay Enterprise SDK for iOS. You can get more details about these methods in the ["Advanced usage"](#usage-making-payments-core-wrapper-advanced-integration) section.

## Signature calculation

The signature is the [hash](#https://en.wikipedia.org/wiki/Cryptographic_hash_function) of these four parameters concatenation:

- Order ID
- Amount (formatted with two decimal places, example: 72.10)
- Currency
- Secret passphrase

The signature has to be generated beforehand on the **server side** in order not to show the **secret passphrase** in client code.  
That is why the code examples below are executed in PHP/Python somewhere on your servers and not embedded in the iOS app directly.

![Hashing Algorithm example](images/hashing_algorithm.png)

Please find below some code examples for signature generation.
Note that we chose the **SHA-1**, you need to adapt your server side code accordingly to the **hashing algorithm** you selected.

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

The codes above generate a signature which is to be used by the HiPay Enterprise SDK for iOS.

To get the **secret passphrase** of your account, go to the "Integration" section of your HiPay Enterprise back office, then to "Security Settings".

![Secret passphrase example](images/passphrase.png)

You may choose a secret passphrase if you don't have one already. This passphrase is also used to process server-to-server notifications.

## Mobile app implementation

The iOS SDK needs to use the signature in order to make transactions.  

Please find below an iOS code example querying the merchant's server (yours) to get the signature and then initializing the SDK's payment page with the signature.

#### Objective-C
```objectivec
- (void) requestSignature {

    NSString *orderId = @"TEST_89897";

    /* Assuming that the server url takes the orderId as argument
     * and generates the signature after retrieving
     * the required data (amount and currency) in database */

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

                        /* Once we get the signature, we can instantiate
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

#### Swift
```swift
func requestSignature() {
        let orderID = "TEST_89897"

        /* Assuming that the server url takes the orderId as argument
         * and generates the signature after retrieving
         * the required data (amount and currency) in database */

        let dataUrl = "https://your-server.com/api/signature?orderid=" + orderID

        if let url = URL.init(string: dataUrl) {
            let downloadTask = URLSession.shared.dataTask(with: url) { (data, response, error) in

                DispatchQueue.main.async {

                    if error != nil {
                        // Handle Error
                    }

                    if let urlContent = data {
                        do {
                            let json = try JSONSerialization.jsonObject(with: urlContent, options: JSONSerialization.ReadingOptions.mutableContainers)

                            if let signatureDictionary = json as? [String: Any] {
                                guard let signature = signatureDictionary["signature"] as? String,
                                    let amount = signatureDictionary["amount"] as? NSNumber,
                                    let currency = signatureDictionary["currency"] as? String else {

                                        //Handle Error
                                        return;
                                }

                                /* Once we get the signature, we can instantiate
                                 * and present the payment screen */
                                let paymentPageRequest = HPFPaymentPageRequest.init()
                                paymentPageRequest.orderId = orderID
                                paymentPageRequest.amount = amount
                                paymentPageRequest.currency = currency

                                let paymentScreen = HPFPaymentScreenViewController.init(request: paymentPageRequest, signature: signature)
                                paymentScreen.delegate = self

                                self.present(paymentScreen, animated: true, completion: nil)
                            }
                            else {
                                //Handle Error
                            }
                        }
                        catch {
                            // Handle Error
                        }
                    }
                    else {
                        // Handle Error
                    }

                }
            }

            downloadTask.resume()
        }
        else {
            // Handle Error
        }
    }
```

For the sake of this example, we assume that the response is in JSON format.
Once you get the signature, you can create a `HPFPaymentPageRequest` with the necessary parameters and then present the `HPFPaymentScreenViewController` screen. You will get more details about the *payment screen* in the next sections.
