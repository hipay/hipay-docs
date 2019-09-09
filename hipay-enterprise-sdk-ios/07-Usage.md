# Usage (making payments)

Basically, there are two ways to make payments:

- The [Fastest integration](#usage-making-payments-fastest-integration), allowing you to accept payments in your iOS app very quickly. In this scenario, your customers are presented with a built-in native payment screen. Yet, you won't be able to do much customization of the payment workflow.

- The [Advanced integration](#usage-making-payments-advanced-integration) using the _core wrapper_. In this case, you build your own payment workflow and your own form. You can thus customize the payment experience to fit your needs. On the downside, you have to take care of building the whole user interface, creating and sending orders, etc.

## Fastest integration

This method is used by the demo application. Do not hesitate to test the demo app for a comprehensive example of the built-in payment screen integration.

In this example, we assume that you will test the integration in a controller named `DemoViewController`, but it can be anywhere in your code base.

Please find below the full code example (you may copy and paste all or part of the example above). Details can be found in comments as well.

#### Objective-C

```objectivec
// DemoViewController.h

#import <HiPayFullservice/HiPayFullservice.h>

// You need to implement the HPFPaymentScreenViewControllerDelegate protocol
@interface DemoViewController : UIViewController <HPFPaymentScreenViewControllerDelegate>
{
    NSString * signature;
}
- (IBAction)payButtonTouched;

@end
```

```objectivec
// DemoViewController.m

/* This is a controller example,
 * the place where you put this code
 * is really up to you. */
@implementation DemoViewController

/* We assume that you have defined a button in an XIB with
 * the method "payButtonTouched" as touch callback. */
- (IBAction)payButtonTouched {

    /* Create a payment page request which
     * contains information about your order */
    HPFPaymentPageRequest *request = [[HPFPaymentPageRequest alloc] init];
    request.amount = @(155.50);
    request.currency = @"EUR";
    request.orderId = @"TEST5987";
    request.shortDescription = @"Outstanding shirt";

    /* Below, optional properties are defined as well.
     * Check the HPFPaymentPageRequest documentation
     * for the full list of parameters */
    request.customer.country = @"FR";
    request.customer.firstname = @"John";
    request.customer.lastname = @"Doe";
    request.customer.email = @"yourclient@domain.com";

    // Tells HiPay to bypass 3-D Secure
    request.authenticationIndicator = HPFAuthenticationIndicatorBypass;

    /* Now, we instantiate and present the payment
     * screen, using the payment page request */
    HPFPaymentScreenViewController *viewController = [HPFPaymentScreenViewController paymentScreenViewControllerWithRequest:request signature:signature];
    viewController.delegate = self;
    [self presentViewController:viewController animated:YES completion:nil];
}

/* Below are the implementation methods of the
 * HPFPaymentScreenViewControllerDelegate protocol */

- (void)paymentScreenViewController:(HPFPaymentScreenViewController *)viewController didEndWithTransaction:(HPFTransaction *)transaction {
    // Transaction object received, check its "state" property to know if the transaction was completed
}

- (void)paymentScreenViewController:(HPFPaymentScreenViewController *)viewController didFailWithError:(NSError *)error {
    // Payment workflow did fail, check the error object for more info
}

- (void)paymentScreenViewControllerDidCancel:(HPFPaymentScreenViewController *)viewController {
    // The user did cancel the payment workflow
}
```

#### Swift

```Swift
// DemoViewController.swift

/* This is a controller example,
 * the place where you put this code
 * is really up to you. */
class DemoViewController: UIViewController, HPFPaymentScreenViewControllerDelegate {

    /* We assume that you have defined a button in an XIB with
     * the method "payButtonTouched" as touch callback. */
    func payButtonTouched() {

        /* Create a payment page request which
         * contains information about your order */
        let request = HPFPaymentPageRequest();
        request.amount = 155.50;
        request.currency = "EUR";
        request.orderId = "TEST5987";
        request.shortDescription = "Outstanding shirt";

        /* Below, optional properties are defined as well.
         * Check the HPFPaymentPageRequest documentation
         * for the full list of parameters */
        request.customer.country = "FR";
        request.customer.firstname = "John";
        request.customer.lastname = "Doe";
        request.customer.email = "yourclient@domain.com";

        // Tells HiPay to bypass 3-D Secure
        request.authenticationIndicator = HPFAuthenticationIndicator.Bypass;

        /* Now, we instantiate and present the payment
         * screen, using the payment page request */
        let viewController = HPFPaymentScreenViewController(request: request, signature: signature)
        viewController.delegate = self
        self.presentViewController(viewController, animated: true, completion: nil)
    }

    /* Below are the implementation methods of the
     * HPFPaymentScreenViewControllerDelegate protocol */

    func paymentScreenViewController(viewController: HPFPaymentScreenViewController, didEndWithTransaction transaction: HPFTransaction) {
        // Transaction object received, check its "state" property to know if the transaction was completed
    }

    func paymentScreenViewController(viewController: HPFPaymentScreenViewController, didFailWithError error: NSError) {
        // Payment workflow did fail, check the error object for more info
    }

    func paymentScreenViewControllerDidCancel(viewController: HPFPaymentScreenViewController) {
        // The user did cancel the payment workflow
    }
}
```

**Implementation note**
The _signature_ parameter is required for security purposes in the HPFPaymentScreenViewController initialization. Please refer to the [Signature](#signature) section for details.

This example will present the built-in payment screen to your users when the `payButtonTouched` method is called (you may add a button targeting this method upon a touch). Once the payment workflow finishes, the `HPFPaymentScreenViewControllerDelegate` protocol methods will be called.

Please find below some additional details.

### Payment page request

As mentioned in the comments, some parameters are optional. However, we strongly encourage you to provide some parameters such as the name of your customers if you have it. By doing so, the card holder's name will be filled in automatically.

Not all the parameters have been set in the example of the payment page request definition above. There are many properties that you can use in order to provide more details about the order, for instance:

- `multiUse` to tell the Secure Vault that you may re-use the credit card token in the future for recurring payments;
- `paymentProductCategoryList` to tell which categories of payment products should appear on the payment screen;
- `paymentProductList` to configure precisely the payment methods which should appear on the payment screen;
- `customData` to send additional data alongside the transaction which you can get back later.

Check out the `HPFPaymentPageRequest` class documentation for more information about all the properties you can set.

### Delegate methods

You will likely need to modify the implementation of the `HPFPaymentScreenViewControllerDelegate` methods in order to end your check-out process, present a confirmation message to your users, etc.

### Localization Errors

In this integration mode, some popups can be shown when errors occured with a title and description.
So if you want to customize the localization in the SDK or even add new languages, you can add _localizable.strings_ files in your XCode project. After that, you can use keys below to override existing values.

> HPF_ERROR_APPLE_PAY_MESSAGE = "We have a problem with Apple Pay";

<br/>

| KEY                                           | VALUE                                                                                                                                        | CONDITION                                                                                                                      |
| --------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| HPF_CARD_STORED_TOUCHID_NOT_ACTIVATED_MESSAGE | Please check that Touch ID is activated                                                                                                      | - TouchID is not enabled to use card storage feature                                                                         |
| HPF_ERROR_NETWORK_OTHER_MESSAGE               | Your request could not be completed.                                                                                                         | - Connection error or server error when receiving the transaction <br/>- Error while retrieving the list of available payments |
| HPF_ALERT_TRANSACTION_LOADING_MESSAGE         | Your transaction is still being processed. Are you sure that you want to leave this screen?                                                  | - The user clicks on the "Cancel" button on the screen while sending a transaction                                             |
| HPF_ERROR_APPLE_PAY_MESSAGE                   | Your request could not be completed.                                                                                                         | - Apple Pay Configuration Error <br/>- Payment Page Display Error <br/>- Apple Pay Token Token Failed                          |
| HPF_CARD_SCAN_PERMISSION                      | To scan your payment card, allow camera permission.                                                                                          | - Camera was not allowed                                                                                                       |
| HPF_CARD_SWITCH_TOUCHID_DESCRIPTION           | Would you like to activate Touch ID for securing future payments with this card? A fingerprint recognition will be required for further use. | - TouchID is not enabled to use card storage feature                                                                         |
| HPF_TRANSACTION_ERROR_DECLINED_MESSAGE        | Please check your entries and try again.                                                                                                     | - Status of the received transaction is "_Declined_"                                                                           |
| HPF_TRANSACTION_ERROR_DECLINED_RESET_MESSAGE  | Please use another payment method.                                                                                                           | - Second attempt failed with the same means of payment                                                                         |
| HPF_TRANSACTION_ERROR_OTHER_MESSAGE           | An error occurred while processing your transaction.                                                                                         | - Status of the received transaction is "_Error_"                                                                              |
| HPF_ERROR_HTTP_CLIENT_MESSAGE                 | Your request could not be completed.                                                                                                         | - 4xx Client HTTP Error <br/>- statusCode = HPFErrorCodeHTTPClient                                                             |
| HPF_ERROR_NETWORK_UNAVAILABLE_MESSAGE         | No Internet connection is available.                                                                                                         | - Display after receiving the transaction <br/>- Network error statusCode = HPFErrorCodeHTTPNetworkUnavailable                 |
| HPF_ERROR_NETWORK_OTHER_MESSAGE               | Your request could not be completed.                                                                                                         | - Error raised after receiving transaction <br/>- Other unhandled error                                                        |
