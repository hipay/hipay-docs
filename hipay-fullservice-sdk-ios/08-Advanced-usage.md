## Core wrapper (advanced integration)

The HiPay Fullservice SDK for iOS contains a layer referred to as the *Core wrapper* which is basically a helpful wrapper of the HiPay Fullservice platform's REST API. By using it, you won't have to send HTTP requests or deal with XML or JSON deserialization. The Core wrapper will take care of this for you.

It exposes models and methods that allow you to easyly send payment requests. In facts, the built-in payment screen itself relies on the Core wrapper for performing order requests, retrieving transaction details, etc.

The sections below describe the main interfaces you can rely on to perform order requests, tokenize credit cards, retrieving transaction details, etc. The payment workflow implementation details and the user interface is up to you.

### Tokenizing a credit or debit card

To tokenize a credit card, you need to leverage the *Secure Vault* client, as below.

This step is mandatory in order to make payments with credit or debit cards, either for one shot or recurring payments.

#### Objective-C
```Objective-C
[[HPFSecureVaultClient sharedClient]
 generateTokenWithCardNumber:@"4111111111111111"
             cardExpiryMonth:@"12"
              cardExpiryYear:@"2020"
                  cardHolder:@"John Doe"
                securityCode:@"496"
                    multiUse:YES
        andCompletionHandler:^(HPFPaymentCardToken * _Nullable cardToken,
                               NSError * _Nullable error) {
     
     /* The cardToken object should be defined with your token
      * if the tokenization was completed successfully.
      * Otherwise, the error object will be defined */
     
 }];
```

#### Swift
```Swift
HPFSecureVaultClient.sharedClient()
    .generateTokenWithCardNumber("4111111111111111",
        cardExpiryMonth: "12",
        cardExpiryYear: "2016",
        cardHolder: "John Doe",
        securityCode: "496",
        multiUse: true) { (cardToken: HPFPaymentCardToken?, error: NSError?) -> Void in
            
            /* The cardToken object should be defined with your token
             * if the tokenization was completed successfully.
             * Otherwise, the error object will be defined */
}
```

### Updating a token or retrieving information about a token

You can also update a token by using the *Secure Vault*'s method named `updatePaymentCardWithToken:requestID:setCardExpiryMonth:cardExpiryYear:cardHolder:completionHandler:`.

To get information about a previsouly generated token, use the Secure Vault's method named `lookupPaymentCardWithToken:requestID:completionHandler:`.

### Request a new order (make payments)

You can make payments by using the *Gateway Client* and its *requestNewOrder* method. This request will both create a new order based on the information you provided and will process the payment simultaneously.

Therefore, you need to provide your order ID, the amount of the transaction, the payment product (Visa, MasterCard, PayPal, etc.) and the related payment method information (i.e. the token if it's a credit or debit card). You can also provide many other optional information as well.

#### Objective-C
```Objective-C
/* Create an order request which
 * contains information about your order */
HPFOrderRequest *request = [[HPFOrderRequest alloc] init];
request.amount = @(155.50);
request.currency = @"EUR";
request.orderId = @"TEST_9641952";
request.shortDescription = @"Outstanding shirt.";
request.operation = HPFOrderRequestOperationSale;
    
/* Below, optional properties are defined as well.
 * Check the HPFPaymentPageRequest's documentation
 * for the full list of parameters */
request.customer.country = @"FR";
request.customer.firstname = @"John";
request.customer.lastname = @"Doe";
request.customer.email = @"yourclient@domain.com";
    
/* Payment method info, in this case,
 * we reuse the token which has been
 * generated in the previous section */
request.paymentProductCode = HPFPaymentProductCodeVisa;
request.paymentMethod = [HPFCardTokenPaymentMethodRequest
    cardTokenPaymentMethodRequestWithToken:@"f39bfab2b6c96fa30dcc0e55aa3da4125a49ab03"
                                       eci:HPFECISecureECommerce
                   authenticationIndicator:HPFAuthenticationIndicatorBypass];
    
[[HPFGatewayClient sharedClient] requestNewOrder:request signature:signature
                           withCompletionHandler:^(HPFTransaction * _Nullable transaction,
                                                   NSError * _Nullable error) {
    /* Check the transaction object, particularly transaction.state.
     * Or check the error object if the request failed. */
}];
    
```

#### Swift
```Swift
/* Create an order request which
 * contains information about your order */
let request = HPFOrderRequest()
request.amount = 155.50;
request.currency = "EUR"
request.orderId = "TEST_859674"
request.shortDescription = "Outstanding shirt."
request.operation = HPFOrderRequestOperation.Sale
    
/* Below, optional properties are defined as well.
 * Check the HPFPaymentPageRequest's documentation
 * for the full list of parameters */
request.customer.country = "FR"
request.customer.firstname = "John"
request.customer.lastname = "Doe"
request.customer.email = "yourclient@domain.com"
    
/* Payment method info, in this case,
 * we reuse the token which has been
 * generated in the previous section */
request.paymentProductCode = HPFPaymentProductCodeVisa
request.paymentMethod = HPFCardTokenPaymentMethodRequest(
    token: "f39bfab2b6c96fa30dcc0e55aa3da4125a49ab03",
    eci: HPFECI.HPFECISecureECommerce,
    authenticationIndicator: HPFAuthenticationIndicator.Bypass)
    
HPFGatewayClient.sharedClient()
    .requestNewOrder(request, signature: signature) { (
        transaction: HPFTransaction?,
        error: NSError?) -> Void in
    /* Check the transaction object, particularly transaction.state. 
     * Or check the error object if the request failed. */
}
```


### Implementation note 
The signature parameter is needed for security purposes.  
Please refer to the [Generating a signature server-side](#generating-a-signature-server-side) for details.

When requesting a new order, do not forget to check the state of the newly created transaction. Moreover, you may need to redirect your user to a web page if the *forwardUrl* property is defined. In this case, you need to define the redirect URL properties on your order request object: *acceptURL*, *declineURL*, etc. In order for the HiPay platform to properly redirect the user to your app, we advise you to use redirect URLs based on [iOS app URL schemes][apple-scheme] (ex. : `myapp://order-result`).

### Get the payment products enabled on your account

In order to get the exact list of payment methods enabled on your account, you can leverage the `getPaymentProductsForRequest:withCompletionHandler:` method of the *Gateway Client*. You need to provide this endpoint with information about your order (by using a , because the list of payment products may change depending on order-related information (i.e. the currency). 

#### Objective-C
```Objective-C
HPFPaymentPageRequest *request = [[HPFPaymentPageRequest alloc] init];
request.amount = @(155.50);
request.currency = @"EUR";
    
// We just want the payment cards products
request.paymentProductCategoryList = @[HPFPaymentProductCategoryCodeCreditCard,
                                       HPFPaymentProductCategoryCodeDebitCard];
    
[[HPFGatewayClient sharedClient] getPaymentProductsForRequest:request
                                        withCompletionHandler:^(NSArray<HPFPaymentProduct *> * _Nonnull paymentProducts,
                                                                NSError * _Nullable error) {
    // Check the paymentProducts array
}];
```

#### Swift
```Swift
let request = HPFPaymentPageRequest();
request.amount = 155.50;
request.currency = "EUR";
    
// We just want the payment cards products
request.paymentProductCategoryList = [
    HPFPaymentProductCategoryCodeCreditCard,
    HPFPaymentProductCategoryCodeDebitCard
]
    
HPFGatewayClient.sharedClient()
    .getPaymentProductsForRequest(request) { (
        paymentProducts: [HPFPaymentProduct],
        error: NSError?) -> Void in
    // Check the paymentProducts array
}
```

### Requesting information about a transaction (check transaction state)

You can get the transactions related to an order or get information about a specific transaction by using the methods: 

- `getTransactionWithReference:withCompletionHandler:`
- `getTransactionsWithOrderId:withCompletionHandler:`

Below is an example with the transactions linked to the merchant order ID "TEST_89897":

#### Objective-C
```Objective-C
[[HPFGatewayClient sharedClient] getTransactionsWithOrderId:@"TEST_89897" signature:signature
                                      withCompletionHandler:^(NSArray<HPFTransaction *> * _Nullable transactions,
                                                              NSError * _Nullable error) {
    // Check the transactions array
}];
```

#### Swift
```Swift
HPFGatewayClient.sharedClient()
    .getTransactionsWithOrderId("TEST_89897", signature: signature) { (
        transactions: [HPFTransaction]?,
        error: NSError?) -> Void in
    // Check the transactions array
}
```


### Implementation note 
The *signature* parameter is required for security purposes.  
Please refer to the [Generating a signature section](#generating-a-signature-server-side) for details.

[apple-scheme]: https://developer.apple.com/library/ios/featuredarticles/iPhoneURLScheme_Reference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40007899
