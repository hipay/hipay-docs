# Configuration

Before going through this section, please read all instructions and make sure you've gone through the [[Prerequisites and recommendations]] section. 

# Core wrapper (advanced integration)

The HiPay Fullservice SDK for Android contains a layer referred to as the *Core wrapper* which is basically a helpful wrapper of the HiPay Fullservice platform's REST API. By using it, you won't have to send HTTP requests or deal with XML or JSON deserialization. The Core wrapper will take care of this for you.

It exposes models and methods that allow you to easyly send payment requests. In facts, the built-in payment screen itself relies on the Core wrapper for performing order requests, retrieving transaction details, etc.

The sections below describe the main interfaces you can rely on to perform order requests, tokenize credit cards, retrieving transaction details, etc. The payment workflow implementation details and the user interface is up to you.

### Tokenizing a credit or debit card

To tokenize a credit card, you need to leverage the *Secure Vault* client, as below.

This step is mandatory in order to make payments with credit or debit cards, either for one shot or recurring payments.

#### Generate a card token
```Java
new SecureVaultClient(this)
	.generateToken(
	
		"4111111111111111", // card number
		"12", // card expiry month
		"2020", // card expiry year
		"John Doe", // card holder
		"496", // security code
		true, // multi use
		
		// request callback
		new SecureVaultRequestCallback() {
			@Override
			public void onSuccess(PaymentCardToken paymentCardToken) {
				// Tokenization completed successfully!
			}
			
			@Override
			public void onError(Exception error) {
				// Tokenization failed, probe exception for details
			}
		}
	);
```

### Updating a token or retrieving information about a token

You can also update a token by using the *Secure Vault*'s `updatePaymentCard(String, String, String, String, String, UpdatePaymentCardCallback)` method.

To get information about a previsouly generated token, use the Secure Vault's `lookupPaymentCardWithToken(String, String, LookupPaymentCardCallback)` method.

### Request a new order (make payments)

You can make payments by using the *Gateway Client* and its `requestNewOrder(OrderRequest, OrderRequestCallback)` method. This request will both create a new order based on the information you provided and will process the payment simultaneously.

Therefore, you need to provide your order ID, the amount of the transaction, the payment product (Visa, MasterCard, PayPal, etc.) and the related payment method information (i.e. the token if it's a credit or debit card). You can also provide many other optional information as well.

```Java
/* Create an order request which
 * contains information about your order */
OrderRequest request = new OrderRequest();

request.setAmount(155.50f);
request.setCurrency("EUR");
request.setOrderId("TEST_9641952");
request.setShortDescription("Outstanding shirt.");
request.setOperation(OrderRelatedRequest.OrderRequestOperation.Sale);

/* Below, optional properties are defined as well.
 * Check the HPFPaymentPageRequest's documentation
 * for the full list of parameters */
request.getCustomer().setCountry("FR");
request.getCustomer().setFirstname("John");
request.getCustomer().setLastname("Doe");
request.getCustomer().setEmail("yourclient@domain.com");

request.setPaymentProductCode(PaymentProduct.PaymentProductCodeVisa);

/* Payment method info, in this case,
 * we reuse the token which has been
 * generated in the previous section */
CardTokenPaymentMethodRequest cardTokenPaymentMethodRequest = new CardTokenPaymentMethodRequest(
                        "f39bfab2b6c96fa30dcc0e55aa3da4125a49ab03",
                        Transaction.ECI.SecureECommerce,
                        CardTokenPaymentMethodRequest.AuthenticationIndicator.Bypass);
                        
request.setPaymentMethod(cardTokenPaymentMethodRequest);

new GatewayClient(this).requestNewOrder(request, new OrderRequestCallback() {

	@Override
	public void onSuccess(Transaction transaction) {
		// Check the transaction object, particularly its state
	}
	
	@Override
	public void onError(Exception error) {
		// Transaction failed, probe exception for details
	}
});    
```

When requesting a new order, do not forget to check the *state* of the newly created transaction.

### Get the payment products enabled on your account

In order to get the exact list of payment methods enabled on your account, you can leverage the `getPaymentProductsForRequest` method of the *Gateway Client*. You need to provide this endpoint with information about your order (by using a *PaymentPageRequest*, because the list of payment products may change depending on order-related information (i.e. the currency). 

```Java
PaymentPageRequest request = new PaymentPageRequest();

request.setAmount(155.50f);
request.setCurrency("EUR");

// We just want the payment cards products
request.setPaymentProductCategoryList(
	Arrays.asList(	
		PaymentProduct.PaymentProductCategoryCodeCreditCard,
		PaymentProduct.PaymentProductCategoryCodeDebitCard))
);

new GatewayClient(this).
	getPaymentProducts(request, new OrderRequestCallback() {
	
		@Override
		public void onSuccess(List<PaymentProduct> paymentProducts) {
			// Check the PaymentProduct objects
		}
	
		@Override
		public void onError(Exception error) {
			// The request failed, probe exception for details
		}
	});
```

### Requesting information about a transaction (check transaction state)

You can get the transactions related to an order or get information about a specific transaction by using the methods: 

- `getTransactionWithReference(String, TransactionDetailsCallback)`
- `getTransactionsWithOrderId(String, TransactionsDetailsCallback)`

Below is an example with the transactions linked to the merchant order ID "TEST_89897":

```Java
new GatewayClient(this)
	.getTransactionsWithOrderId("TEST_89897", new TransactionsDetailsCallback() {
	
		@Override
		public void onSuccess(List<Transaction> transactions) {
			// Check the transactions list
		}
		
		@Override
		public void onError(Exception error) {
			// The request failed, probe exception for details
		}
	})
```

[apple-scheme]: https://developer.apple.com/library/ios/featuredarticles/iPhoneURLScheme_Reference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40007899