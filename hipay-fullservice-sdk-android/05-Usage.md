# Usage (making payments)

Basically, there are two ways to make payments:

- The use of the built-in native payment screeen (easiest integration)
- The custom integration by using the *Core wrapper* (hardest integration)

The first integration is the easiest one, allowing you to accept payments in your Android app very quickly. In this scenario, your customer is presented with a built-in native payment screen. However, you won't be able to customize the payment workflow that much.

On the other hand, you can integrate the *Core wrapper* yourself. In this case, you build your own payment workflow and your own form. In this case, you can customize the payment experience to fit your needs. The downside is that you have to take care of building the whole user interface, creating and sending orders, etc.

The easiest integration (built-in native payment screen) is described below. The hardest one (customized integration) is described in the next section.

## Built-in native payment screen (easiest)

### Example code

This method is used by the demo application. Do not hesitate to test the demo app for a comprehensive example of the built-in payment screen integration.

In this example, we assume that you will test the integration in an activity named `DemoActivity`, but it can be anywhere in your code base. 

Below is the full code example. You will find details in comments as well.


```Java
/* This is an activity example,
 * the place where you put this code
 * is really up to you. */
public class DemoActivity extends AppCompatActivity {
	...

    private String mSignature;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_demo);
    }
    
    /* We assume that you have defined a button in the
     * associated layout activity with
     * the method "onClickPayButton" as touch callback. */
    public void onClickPayButton(View view) {
    
    	/* Create a payment page request which
    	 * contains information about your order */
    	PaymentPageRequest request = new PaymentPageRequest();
    	
    	request.setAmount(155.50f);
    	request.setCurrency("EUR");
    	request.setOrderId("TEST5987");
    	request.setShortDescription("Outstanding shirt.");
    	
    	/* Below, optional properties are defined as well.
    	 * Check the PaymentPageRequest's documentation
    	 * for the full list of parameters */
    	request.getCustomer().setCountry("FR");
    	request.getCustomer().setFirstname("John");
    	request.getCustomer().setLastname("Doe");
    	request.getCustomer().setEmail("yourclient@domain.com");
    	
    	// Tells HiPay to bypass 3-D Secure
    	request.setAuthenticationIndicator(CardTokenPaymentMethodRequest.AuthenticationIndicator.Bypass);
    	
    	CustomTheme customTheme = new CustomTheme(
                android.R.color.holo_blue_light,
                android.R.color.holo_blue_dark,
                android.R.color.white);
                
        /* Now, we instantiate and present the payment
    	 * screen, using the payment page request, the signature 
    	 * and the above custom theme,
         * assuming the signature is not null */
    	PaymentProductsActivity.start(this, request, mSignature, customTheme);
    }
}
```

### Implementation note 
The *signature* parameter is required for security purposes.  
Please refer to the [Generating a signature section](#generating-a-signature-server-side) for details.


### Transaction callback

```Java
public class DemoActivity extends AppCompatActivity {
	...

	@Override
	public void onActivityResult(int requestCode, int resultCode, Intent data) {
		super.onActivityResult(requestCode, resultCode, data);
	
		if (requestCode == PaymentPageRequest.REQUEST_ORDER) {
		
			if (resultCode == R.id.transaction_succeed) {
			
				Bundle transactionBundle = data.getBundleExtra(Transaction.TAG);
				Transaction transaction = Transaction.fromBundle(transactionBundle);
				// Transaction object received, check its "state" property to know if the transaction was completed

			} else if (resultCode == R.id.transaction_failed) {
			
				Bundle exceptionBundle = data.getBundleExtra(Errors.TAG);
				ApiException exception = ApiException.fromBundle(exceptionBundle);
				// Payment workflow did fail, check the exception object for more info
			}
		}
	}
}
```

This example will present the built-in payment screen to your user when the `onClickPayButton` method is called (you may add a button targeting this method upon a touch).  
Once the payment workflow finishes, the result will come back in the activity through your `onActivityResult(int, int, Intent)` method.

You may copy and paste all or part of the example above.

Below are some additional details:

### Payment page request

As mentionned in the comments, some parameters are optional. However, we strongly encourage you to provide some parameters such as the *name* of your customer if you have it. By doing so, the card holder name will be filled automatically.

Not all the parameters have been set in the example of payment page request definition above. There are many properties that you can use in order to provide more details about the order, for instance: 

- `multiUse` to tell the Secure Vault that you may reuse the credit card token in the future for reccurring payments; 
- `paymentProductCategoryList` to tell categories of payment products which should appear on the payment screen; 
- `paymentProductList` to configure precisely the payment methods which should appear on the payment screen;
- `customData` to send additional data alongside the transaction which you can get back later.

Check out the `PaymentPageRequest` class documentation for more information about all the properties you can set.

### Callback method

You will likely need to modify the implementation of the `onActivityResult(int, int, Intent)` method in order to end your check-out process, present a confirmation message to your user, etc.
