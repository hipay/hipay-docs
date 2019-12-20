# Omnichannel APIs

## Objectives

This document aims to describe the omnichannel strategy behind the HiPay Enterprise platform and how to leverage its features through our APIs.
Among other features, the POS capabilities of the HiPay Enterprise platform are outlined hereafter.

## Acronyms and abbreviations

The following acronyms and abbreviations are used in this document.

| Acronym   | Full name |
|----------|-------------|
| POS | Point of sale |
| mPOS | Mobile point of sale |
| API | Application programming interface |

## Overview

First of all, we invite you to visit our [corporate website](https://hipay.com/en/hipay-omnicanal) for an overview of HiPay's omnichannel solutions.

The HiPay Enterprise platform centralizes payment management both for your online and physical stores. The HiPay Gateway allows you to process electronic payments from your online store as well as to initialize transactions on in-store payment terminals, through a single platform.

![Omnichannel API](images/omnichannel-platform.jpg)

Basically, the HiPay Gateway covers three scenarios:

- processing standard e-commerce payment transactions,
- initializing transactions on a payment terminal provided by HiPay or on any terminal embedding the HiPay POS software,
- storing data related to your in-store transactions that have been processed by a third party (in-store gateways).

These scenarios and their integration methods are described below.

# Scenarios

## E-commerce processing

Standard e-commerce processing of payment transactions is described in the [HiPay Enterprise platform overview](/getting-started/platform-hipay-enterprise/overview/). Please refer to this documentation in order to integrate your online shop with HiPay for e-commerce processing (i.e., when customers provide their debit or credit card details through a payment form).

## Initializing transactions on payment terminals (POS)

### Use cases

HiPay provides you with APIs allowing to initialize transactions on payment terminals, including mPOS terminals. For instance, these APIs are useful for:

- store-to-web scenarios, where customers place e-commerce orders on an in-store device (such as a tablet) and pay on a physical payment terminal;
- mobile point of sale scenarios, where customers confirm their in-store order on a device combining the cash register application and the payment terminal.

### Overview

The workflow for managing transactions on physical payment terminals and for charging your customers is the same as the e-commerce one. In fact, in-store transactions are just payment transactions, with basically the same behavior than e-commerce transactions.

The HiPay Enterprise Gateway makes publicly available its omnichannel `/order` API, which allows you to both process e-commerce payments and initialize in-store transactions on payment terminals.

![Omnichannel API](images/omnichannel-pos-api.jpg)

If your system is already integrated with the HiPay Enterprise Gateway for e-commerce payments, you may re-use or refactor your integration in order to leverage it for omnichannel purposes. Please see below to get all the technical details of the POS integration.

### Provisioning payment terminals

To be able to initialize transactions on payment terminals through the HiPay platform, you need to get provisioned payment terminals from HiPay. Depending on your needs, you can get such terminals in different ways:

- Payment terminals configured with the proper HiPay software can be shipped directly from our partners network,
- Your existing payment terminals can be updated by your fleet maintainer in order to embed the HiPay POS software.

Please [submit a request](https://support.hipay.com/hc/en-us/requests/new) on our Support Center to get provisioned payment terminals.

### Initializing a transaction

#### Requirements

Before going through this section, we recommend you to have payment terminals properly configured to be managed through the HiPay Enterprise platform. Once your terminals are configured, HiPay will provide you with a list of unique IDs for your payment terminals.

Moreover, this section refers to APIs which are not specific to in-store payments but generally used in HiPay's transactional workflow, featuring some specific parameters related to POS and physical payment terminals.

Implementing POS scenarios through HiPay is thus easier if you are already familiar with the [HiPay Enterprise platform overview](/getting-started/platform-hipay-enterprise/overview/) and if you have already implemented and tested the [HiPay Enterprise Gateway APIs](/doc-api/enterprise/gateway/).

#### Creating transactions

To initialize a transaction, you need to send a request to the **`POST /order`** service endpoint. You can find the full list of parameters for this endpoint as well as live testing tools in the [HiPay Omnichannel Order API section](/doc-api/omnichannel/api/#!/payments/requestNewOrder). This API is the same as the one used to process e-commerce payments.

In order to initialize transactions on payment terminals, you:

- must send complementary parameters,
- must set some parameters to specific values,
- should provide complementary parameters.

These parameters are listed below.

| Field name | Format | Req. | Specific to POS | Description |
| --- | :---: | --- | :---: | --- |
| `eci` | Numeric | Yes | No | Electronic Commerce Indicator. The ECI indicates the security level at which the payment information is processed between the cardholder and the merchant. In this case, **its value must be set to `10` (Point of sale payment)**.
| `payment_product` | Alphanumeric | No | No | Payment method used to proceed with checkout. In the case of POS payments, you don't know in advance the brand of the card which will be used with the payment terminal. Therefore, **this parameter must not be sent for POS transactions.**
| `initialize_payment_terminal` | Numeric | Yes | Yes | Tells the platform if a payment terminal should be initialized with the related transaction. As this section describes how to initialize payment terminals, **its value must be set to `1`**.
| `payment_terminal_id` | Numeric | No | Yes | ID of the payment terminal on which you need to initialize the transaction. These IDs are supplied by HiPay for each provisioned payment terminal. **You must provide a value if you want to send the transaction to a specific payment terminal.**
| `store_id` | Numeric | No | Yes | ID of the store in which customers process their payment. These IDs are supplied by HiPay when we register your stores in our system. **Providing a value is strongly recommended.**
| `order_point` | Alphabetic | No | No | Order point used by customers to place orders. This parameter accepts two values: `web` or `store`. If your customer is ordering from a tablet in your store, you must provide `store` (the `web` value is only useful for e-reservation orders). **Providing a value is strongly recommended.**
| `pos_transaction_lifetime` | Numeric | No | Yes | Lifetime of the transaction on the payment terminal, in seconds. If the transaction has not been processed within this timeframe, it will transition to the *expired* status. We recommend you to provide a value of `60`, which equals to 1 minute.

You also need to send other global parameters related to the order, such as the amount, the customer's e-mail address, etc. All these parameters are documented in the [HiPay Omnichannel Gateway API section](/doc-api/omnichannel/api/#!/payments/requestNewOrder).

#### Call example

Here is an example of a cURL call for initializing a transaction on a payment terminal in the test environment:

```json
curl -X POST \
--header 'Content-Type: multipart/form-data' \
--header 'Accept: application/json' \
--header 'Authorization: Basic OTQ2NTgzNjUuc3RhZ2Utc2VjdXJlLWdhdGV3YXkuaGlwYXktdHBwLmNvbTpUZXN0X1JoeXBWdktpUDY4VzNLQUJ4eUdoS3Zlcw==' \
-F orderid=1502198470978 \
-F eci=10 \
-F description='SLIM FIT DOBBY OXFORD SHIRT' \
-F currency=EUR \
-F amount=140.00 \
-F language=fr_FR \
-F email=customer@example.com \
-F firstname=John \
-F lastname=Doe \
-F initialize_payment_terminal=1 \
-F pos_transaction_lifetime=60 \
-F payment_terminal_id=62 \
-F store_id=1 \
-F order_point=store \
'https://stage-secure-gateway.hipay-tpp.com/rest/v1/order'
```

You can also perform test API calls from [our live Omnichannel testing tool](/doc-api/omnichannel/api/#!/payments/requestNewOrder).

### Payment on mobile point of sale (mPOS)

For making payments on a mPOS payment terminal plugged with an iPhone or iPod, then you need to use our iOS SDK. In order for the mPOS terminal to receive transactions, you need to call the `connect` method of the iOS SDK beforehand.

Once a transaction has been initialized through the API mentioned in the previous sections, call the `wakeUp` method of the iOS SDK to force the mPOS payment terminal to get the transaction.

For using the HiPay Enterprise SDK for iOS in your application to process transactions on a mPOS, please read [its documentation](/doc/hipay-enterprise-sdk-ios/).

The mPOS does not manage receipt printing. This receipt is present in the notification (upon the Authorization) and [transaction API](/doc-api/omnichannel/api/#!/transaction/getOrderTransactions).

### Examples

The following are XML and JSON receipt format examples.

Response in XML format
```xml
<?xml version="1.0" encoding="UTF-8"?>
<receipt>
  <0>'    Welcome to Hipay    '</0>
  <1>''</1>
  <2>'Terminal:       PP0EH1P6'</2>
  <3>'Merchant:001022087654321'</3>
  <4>'ECR:         ECR_HIPIPP1'</4>
  <5>'STAN:             166528'</5>
  <6>''</6>
  <7>'AID:      A0000000031010'</7>
  <8>'            Visa Prepaid'</8>
  <9>'Card:      ********7178'</9>
  <10>'Cardnr:               0'</10>
  <11>'Exp.Date:         09\\/19'</11>
  <12>''</12>
  <13>'Date:20-04-18  16:53:43'</13>
  <14>'AC:     9DC69FE62E764D38'</14>
  <15>'Processor:       OmniPay'</15>
  <16>'Auth. code:       7CD700'</16>
  <17>'Auth. resp. code:     00'</17>
  <18>''</18>
  <19>'AMOUNT:       EUR 140</19>
  <20>00'</20>
  <21>'REF:        800000074628'</21>
  <22>''</22>
  <23>'Payment agreement        '</23>
  <24>''</24>
  <25>'CLIENT TICKET TO PRESERVE'</25>
  <26>''</26>
  <27>'         GoodBye         '</27>
</receipt>
```

Response in JSON format
```json
{
   "receipt": {
      "0": "'    Welcome to Hipay    '",
      "1": "''",
      "2": "'Terminal:       PP0EH1P6'",
      "3": "'Merchant:001022087654321'",
      "4": "'ECR:         ECR_HIPIPP1'",
      "5": "'STAN:             166528'",
      "6": "''",
      "7": "'AID:      A0000000031010'",
      "8": "'            Visa Prepaid'",
      "9": "'Card:      ********7178'",
      "10": "'Cardnr:               0'",
      "11": "'Exp.Date:         09\\/19'",
      "12": "''",
      "13": "'Date:20-04-18  16:53:43'",
      "14": "'AC:     9DC69FE62E764D38'",
      "15": "'Processor:       OmniPay'",
      "16": "'Auth. code:       7CD700'",
      "17": "'Auth. resp. code:     00'",
      "18": "''",
      "19": "'AMOUNT:       EUR 140",
      "20": "00'",
      "21": "'REF:        800000074628'",
      "22": "''",
      "23": "'Payment agreement        '",
      "24": "''",
      "25": "'CLIENT TICKET TO PRESERVE'",
      "26": "''",
      "27": "'         GoodBye         '"
    }
}
```

### Transaction lifecycle

When the transaction is initialized on the payment terminal, you may display a waiting screen on your application, indicating that the customer must pay on the terminal.

Once the transaction is paid, you need to have a technical callback in place allowing you to know the transaction status (whether it is paid or declined). This will enable you to confirm the order and display the proper feedback screen on your cash register application.

Transaction status updates are delivered through *HTTP server-to-server notifications*. These notifications are exactly the same for both e-commerce and payment terminal transactions. **Please refer to the [*server-to-server notifications* section of the HiPay Enterprise platform overview](/getting-started/platform-hipay-enterprise/overview/#server-to-server-notifications)** to learn how to manage notifications and status updates.

Regarding the *status* and *state* parameters described in the server-to-server notifications section mentioned above, here are some basic scenarios that you will encounter.

#### The transaction is properly paid on the terminal

In that case, the workflow would be:

1. The transaction is initialized, its state is set to **`pending`** and its status to **`174`** (Awaiting Terminal).
2. The customer pays the transaction on the payment terminal.
3. Your server receives a HTTP notification with a status update. The transaction *state* is set to **`completed`** and its *status* to **`118`** (Captured).

#### The transaction is declined on the terminal

In that case, the workflow would be:

1. The transaction is initialized, its state is set to **`pending`** and its status to **`174`** (Awaiting Terminal).
2. The customer encounters an error and the payment is refused.
3. Your server receives a HTTP notification with a status update. The transaction *state* is set to **`declined`** and its *status* to **`113`** (Refused).

Please note that "Refused" is just an example and you may receive other statuses depending on the error. However, in case of an error, the *state* value will always be either `declined` or `error`.

#### The transaction has expired

In that case, the workflow would be:

1. The transaction is initialized with a lifetime of 60 seconds, its state is set to **`pending`** and its status to **`174`** (Awaiting Terminal).
2. The customer waits one minute and does not make the payment.
3. Your server receives a HTTP notification with a status update. The transaction *status* is set to **`114`** (Expired).

#### List of statuses

In order to get the complete list of statuses, please refer to the [HiPay Enterprise platform overview appendices](/getting-started/platform-hipay-enterprise/appendices/).

## Refund a transaction paid on payment terminals (POS)

You can make a refund on a transaction paid on payment terminal exactly like a e-commerce transaction.
With the Token we can refund the customer without being in store.

# Omnichannel SDK

## Objective

The main goal is to have an easy to integrate SDK, that allows you to create a transaction from your point-of-sale software using a POS terminal.


## Diagram

![Omnichannel flow](images/omnichannel_flow.png)

1. The point-of-sale software sends a request payment to the Omnichannel SDK.
2. SDK initialize the payment on the POS Terminal e.g the amount is displayed on the screen and the customer should process the payment.
3. POS Terminal sends the result payment to the Omnichannel SDK in its format.
4. Omnichannel SDK sends the result to the point-of-sale software with a custom object.
5. Transaction request is sent to HiPay's server
6. Result of the transaction request

## iOS 

### Requirements

- iOS >= 9
- XCode >= 11.2.1
- Cocoapods
- POS terminal with Concert protocol version 3

### Installation

You have to use [CocoaPods](https://cocoapods.org/) to install the HiPay Omnichannel SDK for iOS.

Add this line to your project's `Podfile`:

	pod 'HiPayOmnichannelConcertV3'

Then, run the following command in the same directory as your `Podfile`:

	pod install

This will install the **.framework** file in your project

### Initialization

First of all, to use the SDK, you have to  set the **Configuration** object in your ```AppDelegate.swift```. An error is thrown if your configuration is not correctly set.

| Variable name |	Description |	Type |	Values |
|---|---|---|---|---|
| environment<b>*</b>  |	Environment in which the transaction is going to be created |	Enum | Stage<br>Production |
| ipAddress<b>*</b> | POS terminal IPV4 address |	String | e.g. "192.168.1.10" 
| apiPublicUsername* | HiPay username used by authentication |	String | e.g. "123456789.stage-secure-gateway.hipay-tpp[.]com" |
| apiPublicPassword<b>*</b> | HiPay password used by authentication | String | e.g.  "Test_AB1234578903bd5eg" |
| debug | Enable debug mode (display all prints) |Bool | e.g. False

<b>*</b> Mandatory parameters


```swift
import HiPayOmnichannelConcertV3

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    // Override point for customization after application launch.

    do {
        try Configuration.shared.setConfiguration(environment: .Stage,
                                                    ipAddress: "192.168.1.1",
                                                    apiPublicUsername: "username",
                                                    apiPublicPassword: "password",
                                                    debug: false)
    } catch ConfigurationError.invalidIPAddress {
        // Invalid IP Adress
    } catch {
        // Others
    }

    return true
}
```

### Request payment

For each payment, you have to create a **RequestPayment** object with theses variables below. When your **RequestPayment** is created, you execute it with the corresponding method ```execute()```. Your class has to conform to the **RequestPaymentDelegate** delegate to receive a response.

| Variable name |	Description |	Type |	Values |
|---|---|---|---|---|
| **transactionType*** | Type of transaction to be processed | Enum | Debit<br>Credit<br>Cancellation<br>Duplicata<br>Authorization |
| **forceAuthorization*** | Whether the authorization should be forced or not. | Boolean | Default: False |
| **amount*** | Amount of the transaction in the smaller unit of the currency | Float | e.g. 9.99 |	
| **currency*** | ISO 4217 three-digit currency code | Enum | e.g. ".EUR" | 
| orderID | Order number of your request payment. If you not set an identifier, we will generated it for you | String | e.g. "Order_12345"
| mid | Acquirer contract number (maximum length of 7 characters)|	String | e.g. "12345678" 
| cart | Cart object ([More informations](https://support.hipay.com/hc/fr/articles/115001660469-Payment-Gateway-Shopping-cart-management)) | Cart | - |
| customer | Customer's information object (id, firstName, lastName, email) | Customer | - |
| customData | Custom data (only value type Bool / Int / Float / String are accepted) | Dictionary | - |

<b>*</b> Mandatory parameters


```swift
@IBAction func payTapped(_ sender: Any) {

  do {
        let requestPayment = try RequestPayment(transactionType: .Debit,
                                            forceAuthorization: true,
                                            amount: 9.99,
                                            currency: .EUR,
                                            orderID: "order_12345",
                                            mid: "1234567",
                                            cart: nil,
                                            customer: nil,
                                            customData: nil)

        requestPayment.delegate = self
        requestPayment.execute() // Request execution
      } catch RequestPaymentError.invalidAmount {
          // handle invalid amount
      } catch RequestPaymentError.invalidMID {
          // handle invalid MID
      } catch {
          // Others
      }
}
```


In this part, we will show you how to add more details about your request payment.
You can add the cart of the transaction, creating an **Item** for each article ( [More informations about cart](https://support.hipay.com/hc/fr/articles/115001660469-Payment-Gateway-Shopping-cart-management) ).

```swift
    /// Cart
    var cart = Cart()    
    var table = Item(productReference: "A2343SSS",
                        type: .Good,
                        name: "Table",
                        quantity: 2,
                        unitPrice: 150.99,
                        taxRate: 0.0,
                        totalAmount: 301.98)
    table.productCategory = .HomeAppliances
    
    var chair = Item(productReference: "B7762NN",
                        type: .Good,
                        name: "Chair",
                        quantity: 4,
                        unitPrice: 79.49,
                        taxRate: 0.0,
                        totalAmount: 317.96)
    chair.productCategory = .HomeAppliances
    chair.productDescription = "A wooden chair"
    
    cart.items.append(table)
    cart.items.append(chair)
```

**Customer** parameter, you can set all personal information such as his email, first name, last name and an unique identifier.
```swift

    /// Customer
    let customer = Customer(id: "1234",
                            firstName: "John",
                            lastName: "Doe",
                            email: "johnDoe@test.com")
```

In the **custom data** parameter, you can set all the values of you want to retrieve in HiPay's backoffice.

```swift
    /// Custom data
    var customData = [String:Any]()
    customData["foo1"] = "foo2"
    customData["price"] = 12.30
    customData["event"] = 34
    customData["newCustomer"] = true
```

### Response payment

After the transaction has been processed through the HiPay's servers, you will receive a response from the Omnichannel SDK.

```swift
class ViewControlller: UIViewController, RequestPaymentDelegate {

  // ... code example above

  // Mandatory function from RequestPaymentDelegate delegate
  func requestDidEnd(_ response: ResponsePayment) {
        print(response)

        if (response.paymentStatus == .Success) {
            // Handle Success
        }
        else {
            // Handle Failure
        }
  }
}
```

The below table describes the **ResponsePayment** object properties, notice that all these properties are in read-only :

| Variable name |	Description |	Type |	Values |
|---|---|---|---|---|
| paymentStatus	| Status received from the TPE regarding the payment. |	Enum 	| Success <br>Failure |
| errorDescription | Error description | String | e.g. : "The network is unavailable" |	 
| errorCode |	Error code | String | e.g. : "1003" |
| amount | Amount of the transaction | Float | e.g. : 9.99 |
| currency| ISO 4217 three-digit currency code | Enum | e.g. .EUR | 
| orderID | Order number | String | e.g. : "order_12345" |
| notificationHipaySent | Indicates whether Hipay has been notified of the transaction | Boolean | e.g. False |

### Payment example

Here you have a complete example of the code needed to request a payment and handle its response.

```swift
import UIKit
import HiPayOmnichannelConcertV3

class ViewController: UIViewController, RequestPaymentDelegate {

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
    }

    @IBAction func payTapped(_ sender: Any) {
        
        /// Cart
        var cart = Cart()
        var table = Item(productReference: "A2343SSS",
                            type: .Good,
                            name: "Table",
                            quantity: 2,
                            unitPrice: 150.99,
                            taxRate: 0.0,
                            totalAmount: 301.98)
        table.productCategory = .HomeAppliances
        table.europeanArticleNumbering = "4711892728946"
        table.discount = 20.50

        var chair = Item(productReference: "B7762NN",
                            type: .Good,
                            name: "Chair",
                            quantity: 4,
                            unitPrice: 79.49,
                            taxRate: 0.0,
                            totalAmount: 317.96)
        chair.productCategory = .HomeAppliances
        chair.productDescription = "A wooden chair"
        chair.europeanArticleNumbering = "4713716322385"
        chair.discount = 20.50

        cart.items.append(table)
        cart.items.append(chair)
        
        /// Customer
        let customer = Customer(id: "1234",
                                firstName: "John",
                                lastName: "Doe",
                                email: "johnDoe@test.com")
        
        /// Custom data
        var customData = [String:Any]()
        customData["foo1"] = "foo2"
        customData["price"] = 12.30
        customData["event"] = 34
        customData["newCustomer"] = true
        
        do {
            let requestPayment = try RequestPayment(transactionType: .Debit,
                                                forceAuthorization: true,
                                                amount: 9.99,
                                                currency: .EUR,
                                                orderID: "order_12345",
                                                mid: "1234567",
                                                cart: cart,
                                                customer: customer,
                                                customData: customData)

              requestPayment.delegate = self
              requestPayment.execute() // Request execution
        } catch RequestPaymentError.invalidAmount {
            // handle invalid amount
        } catch RequestPaymentError.invalidMID {
            // handle invalid MID
        } catch {
            // Others
        }
    }
    
    func requestDidEnd(_ response: ResponsePayment) {
        // Handle the reponsePayment object
        print(response)

        if (response.paymentStatus == .Success) {
            // Handle Success
        }
        else {
            // Handle Failure
        }
    }   
}
```


## Android 

### Requirements

- Android >= 5.0 Lollipop (API 21) 
- POS terminal with Concert protocol version 3
- Android Studio >= 3.5.3

### Installation

You have to use [jCenter](https://bintray.com/beta/#/hipayandroid/maven/hipay-omnichannel-concertv3?tab=overview) to install the HiPay Omnichannel SDK for Android.

Add this line to your `Build.gradle` application file :

    implementation 'com.hipay:hipay-omnichannel-concertv3:1.0.0'

### Initialization

First of all, to use the SDK, you have to  set the **Configuration** object. An exception is thrown if your configuration is not set correctly.

| Variable name |	Description |	Type |	Values |
|---|---|---|---|---|
| environment<b>*</b>  |	Environment in which the transaction is going to be created |	Enum | Stage<br>Production |
| ipAddress<b>*</b> | POS terminal IP address |	String | e.g. "192.168.1.10" 
| apiPublicUsername<b>*</b> | HiPay username used by authentication |	String | e.g. "123456789.stage-secure-gateway.hipay-tpp[.]com" |
| apiPublicPassword<b>*</b> | HiPay password used by authentication | String | e.g.  "Test_AB1234578903bd5eg" |
| debug | Enable debug mode (display all prints) | Bool | e.g. False

<b>*</b> Mandatory parameters


```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        try {
            Configuration.getInstance().setConfiguration(
                    Environment.STAGE,
                    "192.168.1.2",
                    "username",
                    "password",
                    false);
        } catch (InvalidIPAddressException e) {
            // Handle InvalidIPAddressException
        }
    }
}
```

### Request payment

For each payment, you have to create a **RequestPayment** object with theses variables below. When your **RequestPayment** object is created, you execute it with the corresponding method ```execute(this)```. Your class has to conform to the **RequestPaymentDelegate** interface to receive a response.

| Variable name |	Description |	Type |	Values |
|---|---|---|---|---|
| transactionType* | Type of transaction to be processed | Enum | Debit<br>Credit<br>Cancellation<br>Duplicata<br>Authorization |
| forceAuthorization* | Whether the authorization should be forced or not. | Boolean | Default: False |
| amount<b>*</b> | Amount of the transaction in the smaller unit of the currency | Float | e.g. 9.99 |	 
| currency<b>*</b> | ISO 4217 three-digit currency code | Enum | e.g. ".EUR" for Euros | 
| orderIdentifier | Order number of your request payment. If you not set an identifier, we will generated it for you | String | e.g. "Order_12345"
| mid | Acquirer contract number |	String | e.g. "12345678" 
| cart | Cart object ([More informations](https://support.hipay.com/hc/fr/articles/115001660469-Payment-Gateway-Shopping-cart-management)) | Cart | - |
| customer | Customer's information object (id, firstName, lastName, email) | Customer | - |
| customData | Custom data | Dictionary | - |

<b>*</b> Mandatory parameters

```java
    @Override
    public void onClick(View view) {
        try {
            RequestPayment requestPayment = new RequestPayment(TransactionType.TRANSACTION_TYPE_DEBIT,
                    false,
                    9.99f,
                    Currency.EUR,
                    "order_1234",
                    "1234567",
                    null,
                    null,
                    null
            );
            requestPayment.execute(this);
        } catch (InvalidAmountException e) {
            // Handle InvalidAmountException
        } catch (InvalidMIDException e) {
            // Handle InvalidMIDException
        }
    }
```

In this part, we will show you how to add more details about your request payment.
You can add the cart of the transaction, creating an **Item** for each article ( [More informations about cart](https://support.hipay.com/hc/fr/articles/115001660469-Payment-Gateway-Shopping-cart-management) ).

```java
    Item item1 = new Item("A2343SSS",
                ItemType.GOOD,
                "Table",
                2,
                150.99f,
                0.0f,
                301.98f);
    item1.setProductCategory(ItemProductCategory.HOME_APPLIANCES);

    Item item2 = new Item("B7762NN",
            ItemType.GOOD,
            "Chairs",
            4,
            79.49f,
            0.0f,
            317.96f
    );
    item2.setProductCategory(ItemProductCategory.HOME_APPLIANCES);
    item2.setProductDescription("A wooden chair");

    ArrayList<Item> itemArrayList = new ArrayList<>();
    itemArrayList.add(item1);
    itemArrayList.add(item2);

    Cart cart = new Cart(itemArrayList);
```

**Customer** parameter, you can set all personal information such as his email, first name, last name and an unique identifier.
```java
    Customer customer = new Customer("99", 
                                    "John", 
                                    "Doe", 
                                    "john.doe@example.com"
    );
```

In the **custom data** parameter, you can set all the values of you want to retrieve in HiPay's backoffice.

```java
    HashMap<String, Object> customData = new HashMap<>();
    customData.put("foo1", "foo2");
    customData.put("price", 12.30);
    customData.put("event", 34);
    customData.put("newCustomer", true);
```

### Response payment

After the transaction has been processed through the HiPay's servers, you will receive a response from the Omnichannel SDK.

```java
    @Override
    public void onFinish(ResponsePayment responsePayment) {
        if (responsePayment.getPaymentStatus() == PaymentStatus.SUCCESS) {
            // Handle success response
        }
        else {
            // Handle failed response
        }
    }
```

The below table describes the **ResponsePayment** object properties, notice that all these properties are in read-only :

| Variable name |	Description |	Type |	Values |
|---|---|---|---|---|
| paymentStatus	| Status received from the TPE regarding the payment. |	Enum 	| Success <br>Failure |
| errorDescription | Error description | String | e.g. : "The network is unavailable" |	 
| errorCode |	Error code | String | e.g. : "1003" |
| amount | Amount of the transaction | Float | e.g. : 9.99 |
| currency | ISO 4217 three-digit currency code | Enum | e.g. Currency.EUR | 
| orderIdentifier | Order number | String | e.g. : "order_12345" |
| notificationHipaySent | Indicates whether Hipay has been notified of the transaction | Boolean | e.g. False |

### Payment example

Here you have a complete example of the code needed to request a payment and handle its response.

```java
// All your imports

public class MainActivity extends AppCompatActivity implements View.OnClickListener, RequestPaymentDelegate {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        try {
            Configuration.getInstance().setConfiguration(
                    Environment.STAGE,
                    "192.168.1.2",
                    "username",
                    "password",
                    false);
        } catch (InvalidIPAddressException e) {
            // Handle InvalidIPAddressException
        }
    }


    @Override
    public void onClick(View view) {

        // Cart
        Item item1 = new Item("A2343SSS",
                ItemType.GOOD,
                "Table",
                2,
                150.99f,
                0.0f,
                301.98f);
        item1.setProductCategory(ItemProductCategory.HOME_APPLIANCES);

        Item item2 = new Item("B7762NN",
                ItemType.GOOD,
                "Chairs",
                4,
                79.49f,
                0.0f,
                317.96f
        );
        item2.setProductCategory(ItemProductCategory.HOME_APPLIANCES);
        item2.setProductDescription("A wooden chair");

        ArrayList<Item> itemArrayList = new ArrayList<>();
        itemArrayList.add(item1);
        itemArrayList.add(item2);

        Cart cart = new Cart(itemArrayList);

        // Customer
        Customer customer = new Customer("99",
                "John",
                "Doe",
                "john.doe@example.com"
        );

        // CustomData
        HashMap<String, Object> customData = new HashMap<>();
        customData.put("foo1", "foo2");
        customData.put("price", 12.30);
        customData.put("event", 34);
        customData.put("newCustomer", true);

        try {
            RequestPayment requestPayment = new RequestPayment(TransactionType.TRANSACTION_TYPE_DEBIT,
                    false,
                    9.99f,
                    Currency.EUR,
                    "order_1234",
                    "1234567",
                    cart,
                    customer,
                    customData
            );
            requestPayment.execute(this);
        } catch (InvalidAmountException e) {
            // Handle InvalidAmountException
        } catch (InvalidMIDException e) {
            // Handle InvalidMIDException
        }
    }

    @Override
    public void onFinish(ResponsePayment responsePayment) {
        if (responsePayment.getPaymentStatus() == PaymentStatus.SUCCESS) {
            // Handle success response
        }
        else {
            // Handle failed response
        }
    }
}
```



## Errors code

Should an error occur, here is a list of all the possible related codes and descriptions.

| Code |	Description |
|---|---|
| 1000 | An unknown error occurred |
| 1001 | Timeout expired |
| 1002 | Authentication failed with HiPay |
| 1003 | The network is unavailable |
| 1004 | POS terminal Not Connected |
| 1005 | Parsing transaction status failed |
| 1006 | Cancelled transaction (REASON) |
| 1007 | Parsing received frame failed |
| 1008 | Parsing created frame from POS terminal failed |
| 1009 | Unknown MID|
