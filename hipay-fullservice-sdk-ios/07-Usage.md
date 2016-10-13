# Usage (making payments)

Basically, there are two ways to make payments:

- The use of the built-in native payment screeen (easiest integration)
- The custom integration by using the *Core wrapper* (hardest integration)

The first integration is the easiest one, allowing you to accept payments in your iOS app very quickly. In this scenario, your customer is presented with a built-in native payment screen. However, you won't be able to customize the payment workflow that much.

On the other hand, you can integrate the *Core wrapper* yourself. In this case, you build your own payment workflow and your own form. In this case, you can customize the payment experience to fit your needs. The downside is that you have to take care of building the whole user interface, creating and sending orders, etc.

The easiest integration (built-in native payment screen) is described below. The hardest one (customized integration) is described in the next section.

## Built-in native payment screen (easiest)

### Example code
This method is used by the demo appliation. Do not hesitate to test the demo app for a comprehensive example of the built-in payment screen integration.

In this example, we assume that you will test the integration in a controller named `DemoViewController`, but it can be anywhere in your code base. 

Below is the full code example. You will find details in comments as well 

#### Objective-C

```Objective-C
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

```Objective-C
// DemoViewController.m

/* This is an controller example,
 * the place where you put this code
 * is really up to you. */
@implementation DemoViewController

/* We assume that you have defined a button in a XIB with
 * the method "payButtonTouched" as touch callback. */
- (IBAction)payButtonTouched {
    
    /* Create a payment page request which
     * contains information about your order */
    HPFPaymentPageRequest *request = [[HPFPaymentPageRequest alloc] init];
    request.amount = @(155.50);
    request.currency = @"EUR";
    request.orderId = @"TEST5987";
    request.shortDescription = @"Outstanding shirt.";
    
    /* Below, optional properties are defined as well.
     * Check the HPFPaymentPageRequest's documentation
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

/* This is an controller example,
 * the place where you put this code
 * is really up to you. */
class DemoViewController: UIViewController, HPFPaymentScreenViewControllerDelegate {

    /* We assume that you have defined a button in a XIB with
     * the method "payButtonTouched" as touch callback. */
    func payButtonTouched() {

        /* Create a payment page request which
         * contains information about your order */
        let request = HPFPaymentPageRequest();
        request.amount = 155.50;
        request.currency = "EUR";
        request.orderId = "TEST5987";
        request.shortDescription = "Outstanding shirt.";
        
        /* Below, optional properties are defined as well.
         * Check the HPFPaymentPageRequest's documentation
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

### Implementation note 
The *signature* parameter is required for security purposes.  
Please refer to the [Generating a signature section](#generating-a-signature-server-side) for details.

This example will present the built-in payment screen to your user when the `payButtonTouched` method is called (you may add a button targeting this method upon a touch). Once the payment workflow finishes, the `HPFPaymentScreenViewControllerDelegate` protocol's methods will be called. 

You may copy and paste all or part of the example above.

Below are some additional details:

### Payment page request

As mentionned in the comments, some parameters are optional. However, we strongly encourage you to provide some parameters such as the name of your customer if you have it. By doing so, the card holder name will be filled automatically.

Not all the parameters have been set in the example of payment page request definition above. There are many properties that you can use in order to provide more details about the order, for instance: 

- `multiUse` to tell the Secure Vault that you may reuse the credit card token in the future for reccurring payments; 
- `paymentProductCategoryList` to tell categories of payment products which should appear on the payment screen; 
- `paymentProductList` to configure precisely the payment methods which should appear on the payment screen;
- `customData` to send additional data alongside the transaction which you can get back later.

Check out the `HPFPaymentPageRequest` class documentation for more information about all the properties you can set.

### Delegate's methods

You will likely need to modify the implementation of the `HPFPaymentScreenViewControllerDelegate` methods in order to end your check-out process, present a confirmation message to your user, etc.
