# Usage (making payments)

Basically, there are two ways to make payments:

- With the built-in native payment screen (easiest integration),
- With a custom integration using the *core wrapper* (advanced integration).

The first integration is the easiest one, allowing you to accept payments in your Android app very quickly. In this scenario, your customers are presented with a built-in native payment screen. Yet, you won't be able to do much customization of the payment workflow.

However, you can integrate the *core wrapper* yourself. In this case, you build your own payment workflow and your own form. You can thus customize the payment experience to fit your needs. On the downside, you have to take care of building the whole user interface, creating and sending orders, etc.

The easiest integration (built-in native payment screen) is described below. The advanced one (customized integration) is described in the next section.

## Built-in native payment screen (easiest integration)

### Code example

This method is used by the demo application. Do not hesitate to test the demo app for a comprehensive example of the built-in payment screen integration.

In this example, we assume that you will test the integration in an activity named `DemoActivity`, but it can be anywhere in your code base. 

Please find below the full code example. Details can be found in comments as well.


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
    	request.setShortDescription("Outstanding shirt");
    	
    	/* Below, optional properties are defined as well.
    	 * Check the PaymentPageRequest documentation
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
         * assuming that the signature is not null */
    	PaymentScreenActivity.start(this, request, mSignature, customTheme);
    }
}
```

### Implementation note 
The *signature* parameter is required for security purposes.  
Please refer to the [Generating a server-side signature](#generating-a-server-side-signature) section for details.


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

This example will present the built-in payment screen to your users when the `onClickPayButton` method is called (you may add a button targeting this method upon a touch).  
Once the payment workflow finishes, the result is called in the activity through your `onActivityResult(int, int, Intent)` method.

You may copy and paste all or part of the example above.

Please find below some additional details.

### Payment page request

As mentioned in the comments, some parameters are optional. However, we strongly encourage you to provide some parameters such as the *name* of your customers if you have it. By doing so, the card holder's name will be filled in automatically.

Not all the parameters have been set in the example of the payment page request definition above. There are many properties that you can use in order to provide more details about the order, for instance: 

- `multiUse` to tell the Secure Vault that you may re-use the credit card token in the future for recurring payments; 
- `paymentProductCategoryList` to tell which categories of payment products should appear on the payment screen; 
- `paymentProductList` to configure precisely the payment methods which should appear on the payment screen;
- `customData` to send additional data alongside the transaction which you can get back later.

Check out the `PaymentPageRequest` class documentation for more information about all the properties you can set.

### Callback method

You will likely need to modify the implementation of the `onActivityResult(int, int, Intent)` method in order to end your check-out process, present a confirmation message to your users, etc.

### Localization Errors

In this integration mode, some popups can be shown when errors occured with a title and description. So if you want to customize the localization in the SDK, you can override keys in yours `strings.xml` files in your project.

<u>Example</u> :
<xml>

    <string name="alert_transaction_loading_title">Override string</string>

</xml>

| KEY                                           | VALUE                                                                                                                                        | CONDITION                                                                                                                      |
| --------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| error_title_default | An error occurred||
| error_title_connection | Network error | No available Internet connection |
| error_body_payment_products | No payment product found| Fetching product payment list from HiPay is empty |
| error_body_default | Your request could not be completed | - HTTP API Error |
| error_body_bcmc_app_not_found | Bancontact app could not be found| Unable to find Bancontact application on the device user|
| alert_deleting_payment_card_title|Deleting payment card| The user want to delete its card from device storage|
| alert_deleting_payment_card_body| Are you sure that you want to delete this payment card?| The user want to delete its card from device storage|
| transaction_error_declined_title | Transaction declined| Status of the received transaction is "Declined"|
| transaction_error_declined | Please check your entries and try again| Status of the received transaction is "Declined" or "Error"|
| transaction_error_other|An error occurred while processing your transaction| Status of the received transaction is "Error"  |
| alert_transaction_loading_title| Transaction in progress| The user wants to quit the payment screen while the transaction is processed|
| alert_transaction_loading_body| Your transaction is still being processed. Are you sure that you want to leave the screen?|The user wants to quit the payment screen while the transaction is processed|
